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
            :data="values">
          </stream-chart>
        </div>
        <div class="col col-4" v-if="loaded1">
          <relative-line-chart title="Code Development" group="code" source="codeDevelopment" :data="values['codeDevelopment']"></relative-line-chart>
           
           /*inserting buttons radio functions*/
      <input  type="radio" name="rb" id="rb1" class="first" />
      <label for="rb1">Buisnesses Viewing Projects</label>
      <input   type="radio" name="rb" id="rb2" class="second"/>
      <label for="rb2">Community Manager Monitoring</label>
      <input  type="radio" name="rb" id="rb3" class="third"/>
      <label for="rb3">Individual Contributors</label>
      /*End of Inserting button radio functions*/
          
          
        </div>
        <div class="col col-4" v-if="loaded2">
          <relative-line-chart title="Issue Resolution" group="issue" source="issueResolution" :data="values['issueResolution']"></relative-line-chart>
          
          /*Inserting button radio functions*/          
      <input  type="radio" name="rb" id="rb1" class="first" />
      <label for="rb1">Buisnesses Viewing Projects</label>
      <input   type="radio" name="rb" id="rb2" class="second"/>
      <label for="rb2">Community Manager Monitoring</label>
      <input  type="radio" name="rb" id="rb3" class="third"/>
      <label for="rb3">Individual Contributors</label>
          /*End of Inserting button radio functions*/
          
        </div>
        <div class="col col-4" v-if="loaded3">
          <relative-line-chart title="Community Growth" group="growth" source="communityGrowth" :data="values['communityGrowth']"></relative-line-chart>
     
     /*inserting button radio functions*/          
      <input  type="radio" name="rb" id="rb1" class="first" />
      <label for="rb1">Buisnesses Viewing Projects</label>
      <input   type="radio" name="rb" id="rb2" class="second"/>
      <label for="rb2">Community Manager Monitoring</label>
      <input  type="radio" name="rb" id="rb3" class="third"/>
      <label for="rb3">Individual Contributors</label>
      /*End of Inserting button radio functions*/
      
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
      loaded1: false,
      loaded2: false,
      loaded3: false
    }
  },
  computed: {
    repo () {
      return this.$store.state.baseRepo
    },
    loaded () {
      return this.loaded1 && this.loaded2 && this.loaded3
    }
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
      this.loaded1 = true
    })
    repo.issueResolution().then((data) => {
      this.values['issueResolution'] = this.convertKey(data, 'issueResolution')
      this.loaded2 = true
    })
    // repo.communityGrowth().then((data) => {
    //   console.log("HERE", data)
    //   this.values['communityGrowth'] = this.convertKey(data, 'communityGrowth')
    //   console.log("HERE", data)
    //   this.loaded3 = true
    // })
    this.loaded3 = true
  },
  methods: {
    convertKey(ary, group) {
      ary.forEach((el) => {
        
        let keys = Object.keys(el)
        let field = null
        keys.forEach((key) => {
          if (el[key] != null && key != 'date'){
            field = key
          }
        })
        el['value'] = el[field]
        el['field'] = field 
        el['group'] = group
      })
      return ary
    }
  }
}

</script>
