<template>
  <div>
    <Search @searchResult="searchHandler"></Search>
    <PicturePage @hidePage="hidePage"  :dataPage="dataPage"></PicturePage>
    <div id="home" @scroll="scroll">
      <div class="columns">
        <div class="contain" v-for="data in firstColumn" @click="pickPicture(data)">
          <img :src="data.urls.small" alt="">
          <div class="contain-inf" >
            <p>{{data.user.name}}</p>
          </div>
        </div>
      </div>
      <div class="columns">
        <div  class="contain" v-for="data in secondColumn" @click="pickPicture(data)">
          <img :src="data.urls.small" alt="">
          <div class="contain-inf">
            <p>{{data.user.name}}</p>
          </div>
        </div>
      </div> 
      <div class="columns">
        <div class="contain" v-for="data in thirdColumn" @click="pickPicture(data)">
          <img :src="data.urls.small" alt="">
          <div class="contain-inf">
            <p>{{data.user.name}}</p>
          </div>
        </div>
      </div>
      <div></div>
    </div>
  </div>
</template>

<script>
const appId = '9031921175042d5a0c24e5fa71974ff708aa898ad5850cd5cbb9b900714fec26'
import axios from 'axios'
import Search from '../components/search.vue'
import PicturePage from '../components/picture-page.vue'
export default {
  async asyncData(){
    let options = {params: {client_id: appId, page: 1, per_page: 24} }
    let response = await axios.get(`https://api.unsplash.com/photos/`, options)
    return {
      photos: response.data,
      firstColumn: [],
      secondColumn: [],
      thirdColumn: [],
      options: options,
      timer: null,
      searchValue: null,
      dataPage: null,
    }
  },
  components: { Search, PicturePage },
  methods: {
    splitData(data) {
      for (var i = 0; i < data.length; i++) {
        if (i < 8) {
          this.firstColumn.push(data[i])
        } else if (i > 8 && i <= 16) {
          this.secondColumn.push(data[i])
        } else {
          this.thirdColumn.push(data[i])
        }
      }
    },
    scroll(e) {
      let scroll = e.target.scrollHeight - 1200 <= parseInt(e.target.scrollTop)
      if (scroll) {
        clearTimeout(this.timer)
        this.timer = setTimeout(() =>{
          this.load()
        }, 100)
      }
      if (scroll && this.searchValue) {
        clearTimeout(this.timer)
        this.timer = setTimeout(() => {
          this.loadSearchResult()
        }, 100)
      }
    },
    load() {
      this.options.params.page++
      axios.get(`https://api.unsplash.com/photos/`, this.options).then(response => {
        this.splitData(response.data)
      })
    },
    searchHandler(response, query) {
      if (response) {
        this.searchValue = query
        this.clearColums()
        this.splitData(response.data.results)
        document.getElementById('home').scrollTop = 0  // sorry 
      } else {
        this.searchValue = null 
        this.clearColums()
        this.options.params.page = 0
        this.load()
      }
    },
    loadSearchResult() {
      this.options.params.page++
      let options = {params:{client_id: appId, query: this.searchValue, page: this.options.params.page, per_page: 24}} 
      axios.get('https://api.unsplash.com/search/photos' , options).then(response => {
         this.splitData(response.data.results) 
      })
    },
    clearColums() {
      this.firstColumn = [], this.thirdColumn = [], this.secondColumn = [] 
    },
    pickPicture(data) {
      this.dataPage= data
    },
    hidePage() {
      this.dataPage = null
    }
  },
  created() {
    this.splitData(this.photos)
  }
}
</script>

<style lang="scss">
 #home {
    font-family: 'asd';
    justify-content: center;
    height: 100px;
    display: flex;
    flex-flow: row wrap;
    align-items: flex-start;
    height: 100vh;
    overflow: scroll;
    .columns {
      border: 1px solid rgba(0,0,0, .0);
    }
    .contain {
      border: 1px solid rgba(0,0,0, .05);
      padding: 10px;
      margin: 3px 10px;
      cursor: zoom-in;
      position: relative;
      .contain-inf {
        opacity: 0;
        background-color: rgba(255, 255, 255, 1);
        position: absolute;
        margin: 0;
        height: 60px;
        color: rgba(0,0,0, .6);
        width: 95.7%;
        bottom: 3px;
        transition: all .14s;
        display: flex;
        justify-content: flex-end;
        align-items: center;
        p {
          font-size: .8em;
          margin: 0 15px;
        }
        h5 {
        }
      }
      &:hover {
        .contain-inf {
          opacity: 1;
        }
      }
    }
 }
</style>
