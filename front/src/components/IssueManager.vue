<template>
  <div class="mdl-layout__tab-panel is-active" id="overview">
    <issue-presentation  v-for="issue in issues" :issue="issue"></issue-presentation>
    <br>
    <paginate
    :page-count="numberPages"
    :margin-pages="4"
    :page-range="4"
    :initial-page="0"
    :container-class="'ui pagination menu'"
    :prev-text="'Précédant'"
    :next-text="'Suivant'"
    :click-handler="goToPage"
    :page-link-class="'item'"
    :prev-link-class="'item'"
    :next-link-class="'item'"
    :no-li-surround="true">
  </paginate>
</div>
</template>

<script>
import presentation from './Presentation.vue'
import axios from 'axios'
import VuejsPaginate from 'vuejs-paginate'

export default {
  name: 'IssueManager',
  components: {
    'issue-presentation': presentation,
    'paginate': VuejsPaginate
  },
  data: () => ({
    issues: [],
    numberPages: 1
  }),
  created () {
    this.getPagingList(1)
  },
  methods: {
    goToPage (pageNum) {
      this.getPagingList(pageNum)
    },
    getPagingList (pageNum) {
      axios.get('/api/issues',
        {
          params: {
            page: pageNum,
            size: 4
          }
        })
      .then(response => {
        this.issues = response.data.docs
        this.numberPages = response.data.pages
      }).catch(e => {
        console.log(e)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

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

</style>
