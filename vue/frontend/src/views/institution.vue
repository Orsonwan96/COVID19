
<template>
  <div class="display-view container">
    <!-- 展示数据和可视化 -->
    <div class="display-info ">
      <el-row :gutter="24" >
        <el-col :md="17" :sm="24" :xs="24">
          <institution-card class="display-author" :name="name" v-loading="loading"></institution-card>
          <div
              class="display-data"
              v-loading="isLoading"
              element-loading-text="loading……">
            <institution-paper :paper="paper" v-loading="loading"></institution-paper>
          </div>
        </el-col>
        <el-col :md="7" :sm="24" :xs="24" class="right">
          <div class="display-visual">
            <p class="up">Bio-entities<el-tooltip effect="dark" content="3 types of bio-entities ranked by frequency mentioned in papers published. The second column is the number of papers." placement="top-start">
              <i class="el-icon-question"></i>
            </el-tooltip></p>
            <entityList class="display-visual" :entity="entity" v-loading="loading"></entityList>
            <wordcloud :wordcloud="wordcloud" v-loading="loading"></wordcloud>
            <bar class="down" :bar="bar" v-loading="loading"></bar>
          </div>
          <co-author :coauthor="coauthor" :flag="flag" v-loading="loading"></co-author>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
    import AuthorCard from "../components/general/institutionCard";
    import AuthorPaper from "../components/general/paper";
    import CoAuthors from "../components/general/coauthor";
    import Bar from "../components/general/bar";
    import entityList from "../components/general/list";
    import wordcloud from "../components/general/wordcloud";

    import $axios from "../util/axios";

    export default {
      components: {
        "institution-card": AuthorCard,
        "institution-paper": AuthorPaper,
        "co-author": CoAuthors,
        "bar": Bar,
        "entityList": entityList,
        "wordcloud": wordcloud
      },
      data() {
        return {
          name: [],
          d: [],
          card: [],
          paper: [],
          coauthor: [],
          bar: [],
          wordcloud: [],
          entity: [],
          flag: "",
          loading: true,
        }
      },
      created() {
        this.get_data()

      },
      watch: {
        '$route': 'get_data'
      },
      methods: {
        get_data() {
          this.loading = true
          $axios.get("/displayInstitution/" + this.$route.params.id).then(response => {
            this.loading = false
            let d = response.data
            this.name = d["name"]
            this.paper = [d["articles"], d["clinicals"]]
            this.bar = d["bar"]
            this.coauthor = d["coauthor"]
            this.entity = d["wordcloud"]
            this.wordcloud = d["wordcloud"]
            this.flag = "ins"
          })
        }
      }
    }

</script>

<style>

    .container {
        max-width: 1200px;
         width: 100%;
        margin: 0 auto;
    }
    .right{
        margin: 0 auto;
    }
    .display-author{
        width: 100%;
    }
    .display-view {
        width: 100%;
         margin: 0 auto;
    }

    .display-query {
        margin-bottom: 1.5em;
    }
    .display-query #query {
        font-weight: bold;
        font-size: 1.5em;
    }

    .display-info {
        min-height: 30em;
        margin:auto;
    }


    .display-data {
        border-right: 1px solid rgba(150,150,150,0.2);
    }

    .display-visual .title {
        text-transform: capitalize;
        margin-bottom: 1em;
    }

    .el-divider--horizontal {
        margin:0 0;
    }

    .up {
        margin-top: 1.8em;
    }

    .down {
        margin-bottom: 1em;
    }

</style>