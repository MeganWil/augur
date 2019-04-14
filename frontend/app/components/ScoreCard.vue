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
        <div class="col col-12">
          <stream-chart title="Overall Score Over Time"></stream-chart>
        </div>
        <div class="col col-3">
          <relative-line-chart title="Growth, Maturity, and Decline" group="gmd"></relative-line-chart>
        </div>
        <div class="col col-3">
          <relative-line-chart title="Diversity and Inclusion" group="diversityinclusion"></relative-line-chart>
        </div>
        <div class="col col-3">
          <relative-line-chart title="Value" group="value"></relative-line-chart>
        </div>
        <div class="col col-3">
          <relative-line-chart title="Risk" group="risk"></relative-line-chart>
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
      colors: ["#FF3647", "#4736FF","#3cb44b","#ffe119","#f58231","#911eb4","#42d4f4","#f032e6"]
    }
  },
  components: {
    //register the charts as components (ALL USE CASES)
    StreamChart,
    RelativeLineChart
  }
}

</script>