<template>
    <div>
        <el-card class="box-card">
        <div slot="header" class="clearfix">
            <span>热门赛事</span>
        </div>
        <div v-for="article in articles" :key="article.id">
                <div style="float:left;width:85%;height: 150px;">
                    <router-link class="article-link" :to="{path:'jotter/article',query:{id: article.id,type: 'game'}}"><span style="font-size: 20px"><strong>{{article.articleTitle}}</strong></span></router-link>
                    <el-divider content-position="left">{{article.articleDate}}</el-divider>
                    <router-link class="article-link" :to="{path:'jotter/article',query:{id: article.id,type: 'game'}}"></router-link>
                </div>
                <el-image
                    style="margin:18px 0 0 30px;width:100px;height: 100px"
                    :src="article.articleCover"
                    fit="cover"></el-image>
                <el-divider></el-divider>
                </div>
        </el-card>
    </div>
</template>

<script>

export default {
    name: 'SideRank',
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
        loadArticles () {
        var _this = this
        this.$axios.get('/api/article/' + this.pageSize + '/1').then(resp => {
          if (resp && resp.data.code === 200) {
            console.log(resp.data.result)
            _this.articles = resp.data.result.content
            _this.total = resp.data.result.totalElements
          }
        })
      }
    }
}
</script>

<style scoped>
.box-card {
width: 300px;
}
</style>
