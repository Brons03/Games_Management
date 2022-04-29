<template>
<div>
  <div style="margin-top: 40px" class="index">
    <div class="side">
      <el-link href="/admin/content/editor" :underline="false" target="_blank" class="add-link">
    <el-button type="primary" size="large" icon="el-icon-edit">发布赛事</el-button>
      </el-link>
      <div style="height:10px;"></div>
    <side-menu id="side-menu" @indexSelect="listByCategory" ref="sideMenu"></side-menu>
    </div>
      <div class="home">
        <div id="header-div">
          <div style="height:40px"></div>
          
          <search-bar @onSearch="searchResult" ref="searchBar"></search-bar>
          <carousel></carousel>
          <!-- <quick-nav style="float: right;margin-top: -450px;margin-right: 480px"></quick-nav> -->
        </div>
        <!-- <update-card id="update-card"></update-card> -->
        <games id="games"></games>
        <!-- <slogan id="slogan"></slogan> -->
      </div>
      <side-rank id="side-rank"></side-rank>
  </div>
</div>
</template>

<script>
  import SearchBar from './SearchBar'
  import Carousel from './Carousel'
  import QuickNav from './QuickNav'
  // import Slogan from './Slogan'
  // import UpdateCard from './UpdateCard'
  import Games from './Games'
  import SideMenu from './SideMenu'
  import SideRank from './SideRank'
  export default {
    name: 'AppIndex',
    components: {Carousel, QuickNav, Games, SideMenu, SearchBar, SideRank},
    methods: {
      listByCategory () {
        var _this = this
        var cid = this.$refs.sideMenu.cid
        var url = '/api/categories/' + cid + '/books'
        this.$axios.get(url).then(resp => {
          if (resp && resp.data.code === 200) {
            _this.$refs.booksArea.books = resp.data.result
            _this.$refs.booksArea.currentPage = 1
          }
        })
      },
      searchResult () {
        var _this = this
        this.$axios
          .get('/api/search?keywords=' + this.$refs.searchBar.keywords, {
          }).then(resp => {
          if (resp && resp.data.code === 200) {
            _this.books = resp.data.result
          }
        })
      }
    }
  }
</script>

<style scoped>
  .index{
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
  .home {
    width: 900px;
    margin-left: 20px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }

  #header-div {
    padding-bottom: 20px;
    padding-left: 5px;
    background-color: white;
  }
  #side-rank{
    margin-left: 20px;
  }
</style>
