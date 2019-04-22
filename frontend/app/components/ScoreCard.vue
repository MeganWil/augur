<template>
  <section>
    <div style="display: inline-block;">
      <h2 style="display: inline-block; color: black !important; margin-bottom: 20px">{{ $store.state.gitRepo }}</h2>
      <h2 style="display: inline-block;margin-bottom: 20px" class="repolisting" v-if="$store.state.comparedRepos.length > 0"> compared to: </h2>
      <h2 style="display: inline-block;margin-bottom: 20px" v-for="(repo, index) in $store.state.comparedRepos">
        <span v-bind:style="{ 'color': colors[index] }" class="repolisting"> {{ repo }} </span> 
      </h2>
    </div>
      <div class="row">
        <!--  CHARTS AND OTHER CONTENT WILL GO HERE (ALL USE CASES)-->
        <div class="col col-12" v-if="loaded">
          <stream-chart title="Overall Score Over Time"
            source="codeDevelopment"
            :data="values['codeDevelopment']">
          </stream-chart>
        </div>
        <div class="col col-3" v-if="loaded">
          <relative-line-chart title="Code Development" group="code" source="codeDevelopment" :data="values['codeDevelopment']"></relative-line-chart>
        </div>
        <div class="col col-3">
          <relative-line-chart title="Issue Resolution" group="issue" source="codeDevelopment"></relative-line-chart>
        </div>
        <div class="col col-3">
          <relative-line-chart title="Community Growth" group="growth" source="codeDevelopment"></relative-line-chart>
        </div>
      </div>
    </div>
  </section>
</template>

<script>

// IMPORT THESE CHARTS WE WILL CREATE
import StreamChart from './charts/StreamChart'
import RelativeLineChart from './charts/RelativeLineChart'

module.exports = {
  data() {
    return {
      values: {},
      colors: ["#FF3647", "#4736FF","#3cb44b","#ffe119","#f58231","#911eb4","#42d4f4","#f032e6"],
      loaded: false
    }
  },
  computed: {
    repo () {
      return this.$store.state.baseRepo
    },
  },
  components: {
    //register the charts as components (ALL USE CASES)
    StreamChart,
    RelativeLineChart
  },
  created () {
    let repo = window.AugurAPI.Repo({ githubURL: this.repo })
    repo.codeDevelopment().then((data) => {
      console.log("HERE", data)
      this.values['codeDevelopment'] = this.convertKey(data, 'codeDevelopment')
      console.log("HERE", data)
      this.loaded = true
    })
  },
  methods: {
    convertKey(ary, group) {
      ary.forEach((el) => {
        el['group'] = group
        let keys = Object.keys(el)
        let field = null
        keys.forEach((key) => {
          if (el[key] != null && key != 'date'){
            console.log(key)
            field = key
          }
        })
        el['value'] = el[field]
        el['field'] = field 
      })
      return ary
    }
  }
}

</script>