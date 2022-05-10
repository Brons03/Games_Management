<template>
  <div style="margin-top: 40px" class="article">
    <div class="side">
      <el-link href="/admin/content/editor" :underline="false" target="_blank" class="add-link">
    <el-button type="primary" size="large" icon="el-icon-edit">发布组队</el-button>
      </el-link>
      <div style="height:10px;"></div>
    <side-menu id="side-menu" @indexSelect="listByCategory" ref="sideMenu"></side-menu>
    </div>
   <el-row>
      <!-- <view-switch class="switch"></view-switch> -->
      <div class="articles-area">
      <el-card style="text-align: left">
        <div v-for="article in articles" :key="article.id" class="gameitem">
          <div style="width:70%;" class="">
            <router-link class="article-link" :to="{path:'jotter/article',query:{id: article.id}}"><span style="font-size: 20px"><strong>{{article.articleTitle}}</strong></span></router-link>
            <el-divider content-position="left">{{article.articleDate}}</el-divider>
            <router-link class="article-link" :to="{path:'jotter/article',query:{id: article.id}}"><p>{{article.articleAbstract}}</p></router-link>
          </div>
          <el-image
            style="margin:18px 0 0 30px;width:30%"
            :src="article.articleCover"
            fit="cover"></el-image>
        </div>
      </el-card>
    </div>
    <el-pagination
      background
      layout="total, prev, pager, next, jumper"
      @current-change="handleCurrentChange"
      :page-size="pageSize"
      :total="total">
    </el-pagination>
    </el-row>
  </div>
</template>

<script>
  import SideMenu from './SideMenu'
  export default {
    name: 'Articles',
    components: { SideMenu },
    data () {
      return {
        articles: [],
        pageSize: 6,
        total: 0
      }
    },
    mounted () {
      this.loadArticle()
    },
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
      loadArticle () {
        var _this = this
        this.$axios.get('/api/article/' + this.pageSize + '/1').then(resp => {
          if (resp && resp.data.code === 200) {
            console.log(resp.data.result)
            _this.articles = resp.data.result.content
            _this.total = resp.data.result.totalElements
          }
        })
      },
      handleCurrentChange (page) {
        var _this = this
        this.$axios.get('/api/article/' + this.pageSize + '/' + page).then(resp => {
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
  .articles-area {
    width: 990px;

    margin-left: 20px;
    margin-right: auto;
  }

  .article-link {
    text-decoration: none;
    color: #606266;
  }

  .article-link:hover {
    color: #409EFF;
  }
  .article{
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
  .gameitem{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    border-bottom-style:  solid;
    border-bottom-color: #DCDFE6;
    border-bottom-width: 1px;
    margin-top: 10px;
  }
</style>
