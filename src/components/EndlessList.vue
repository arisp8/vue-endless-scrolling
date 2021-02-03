<template>
  <div class="endless-scrolling-list">
    <div class="search-box">
      <input type="text" v-model="searchQuery"/>
    </div>
    <p class="center" v-if="results.length == 0 && !loading">
      Start typing to search something.
    </p>
    <virtual-list
      :data-key="'pageid'"
      :data-sources="results"
      :data-component="itemComponent"
      :page-mode="true"
      />
    <loader v-if="loading" />
  </div>
</template>

<script>
import axios from 'axios';
import lodash from 'lodash';
import VirtualList from 'vue-virtual-scroll-list';
import SearchResult from './SearchResult';
import Loader from './Loader';
export default {
  name: 'EndlessList',
  components: {
    VirtualList,
    Loader
  },
  data() {
    return {
      searchQuery: '',
      currentPage: 0,
      results: [],
      itemComponent: SearchResult,
      loading: false
    }
  },
  watch: {
    searchQuery: {
      immediate: true,
      handler: lodash.debounce(function(newVal) {
        if (newVal == '') {
          return;
        }
        this.results = [];
        this.search(this.currentPage);
        this.search(this.currentPage + 1);
        this.currentPage = 2
      }, 200)
    }
  },
  mounted() {
    const vm = this;
    window.onscroll = lodash.debounce(function() {
      var distanceFromBottom = document.body.scrollHeight - window.innerHeight - window.scrollY;
      if (distanceFromBottom < 400) {
        vm.search(vm.currentPage);
        vm.currentPage++;
      }
    }, 100);
  },
  methods: {
    search(page) {
      const data = {
        action: 'query',
        format: 'json',
        list: 'search',
        continue: '-||',
        utf8: 1,
        srsearch: this.searchQuery,
        sroffset: page * 10,
        origin: '*'
      }
      const params = Object.keys(data).map(function(k) {
                return data[k] == '' ? ''
                    : encodeURIComponent(k) + '=' + encodeURIComponent(data[k])
            }).join('&')
      const searchUrl = `https://en.wikipedia.org/w/api.php?${params}`
      this.loading = true;
      axios.get(searchUrl)
        .then(response => {
          this.results = this.results.concat(response.data.query.search);
          this.loading = false;
        });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.searchmatch {
  background: #F7FFA9;
}
.center {
  text-align: center;
}
</style>
