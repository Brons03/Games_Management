<template>
  <div>
    <el-row>
      <!-- <view-switch class="switch"></view-switch> -->
      <div class="articles-area">
      <el-card style="text-align: left">
        <div v-for="article in articles" :key="article.id" class="gameitem">
          <div style="width:85%;" class="">
            <router-link class="article-link" :to="{path:'jotter/article',query:{id: article.id,type: 'game'}}"><span style="font-size: 20px"><strong>{{article.articleTitle}}</strong></span></router-link>
            <el-divider content-position="left">{{article.articleDate}}</el-divider>
            <router-link class="article-link" :to="{path:'jotter/article',query:{id: article.id,type: 'game'}}"><p>{{article.articleAbstract}}</p></router-link>
          </div>
          <el-image
            style="margin:18px 0 0 30px;width:100px"
            :src="article.articleCover"
            fit="cover"></el-image>
        </div>
      </el-card>
    </div>
    <el-pagination
      background
      layout="total, prev, pager, next, jumper"
      @current-change="handleCurrentChangeArticle"
      :page-size="pageSize"
      :total="total">
    </el-pagination>
    </el-row>
  </div>
</template>

<script>
  import SearchBar from './SearchBar'

  export default {
    name: 'Games',
    components: {SearchBar},
    data () {
      return {
        books: [],
        currentPage: 1,
       articles: [],
        pageSize: 4,
        total: 0
      }
    },
    mounted: function () {
      this.loadArticles()
    },
    methods: {
      loadBooks () {
        var _this = this
        this.$axios.get('/api/games').then(resp => {
          if (resp && resp.data.code === 200) {
            _this.books = resp.data.result
          }
        })
      },
      handleCurrentChange: function (currentPage) {
        this.currentPage = currentPage
      },
      loadArticles () {
        var _this = this
        this.$axios.get('/api/game/' + this.pageSize + '/1').then(resp => {
          if (resp && resp.data.code === 200) {
            console.log(resp.data.result)
            _this.articles = resp.data.result.content
            _this.total = resp.data.result.totalElements
          }
        })
      },
      handleCurrentChangeArticle (page) {
        var _this = this
        this.$axios.get('/api/game/' + this.pageSize + '/' + page).then(resp => {
          if (resp && resp.data.code === 200) {
            _this.articles = resp.data.result.content
            _this.total = resp.data.result.totalElements
          }
        })
      }
    }
  }
</script>
<style scoped>

  .cover {
    width: 115px;
    height: 172px;
    margin-bottom: 7px;
    overflow: hidden;
    cursor: pointer;
  }

  img {
    width: 115px;
    height: 172px;
    /*margin: 0 auto;*/
  }
  .gameitem{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    border-bottom-style:  solid;
    border-bottom-color: #DCDFE6;
    border-bottom-width: 1px;
  }

  .title {
    font-size: 14px;
    text-align: left;
  }

  .author {
    color: #333;
    width: 102px;
    font-size: 13px;
    margin-bottom: 6px;
    text-align: left;
  }

  .abstract {
    display: block;
    line-height: 17px;
  }

  .el-icon-delete {
    cursor: pointer;
    float: right;
  }

  .switch {
    display: flex;
    position: absolute;
    left: 780px;
    top: 25px;
  }

  a {
    text-decoration: none;
  }

  a:link, a:visited, a:focus {
    color: #3377aa;
  }

</style>
