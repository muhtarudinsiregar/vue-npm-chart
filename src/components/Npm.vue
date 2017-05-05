<template lang="">
  <div class="col-md-8 offset-md-2">
    <form class="form-inline">
      <input required v-model="from" type="date" class="form-control mb-2 mr-sm-2 mb-sm-0" id="inlineFormInput" placeholder="From">
      <input required v-model="to" type="date" class="form-control mb-2 mr-sm-2 mb-sm-0" id="inlineFormInputGroup" placeholder="To">
      <input v-model="packages" type="text" class="form-control" id="inlineFormInputGroup" placeholder="Packages">
      <button type="button" class="btn btn-primary form-control" v-on:click="getList ()">Submit</button>
    </form>
    <div class="col-md-8 Chart">
      <h2>Chart</h2>
      <chart :chartData="downloads" :labels="labels"></chart>
    </div>
  </div>
</template>
<script lang="">
import Chart from './Chart'

export default {
  components: {
    Chart
  },
  name: 'npm',
  data () {
    return {
      downloads: '',
      from: '2017-05-01',
      to: '2017-05-02',
      packages: 'react',
      showChart: false,
      labels: ''
    }
  },
  methods: {
    getList () {
      this.$http.get(`https://api.npmjs.org/downloads/range/${this.from}:${this.to}/${this.packages}`)
      .then((response) => {
        this.downloads = response.data
        this.showChart = true
        this.labels = this.packages
      }).catch((error) => {
        console.log(error.status)
      })
    }
  },
  mounted () {
    this.getList()
  }
}
</script>

<style>
.Chart {
  background: #212733;
  border-radius: 15px;
  box-shadow: 0px 2px 15px rgba(25, 25, 25, 0.27);
  margin:  25px 0;
}

.Chart h2 {
  margin-top: 0;
  padding: 15px 0;
  color:  rgba(255, 0,0, 0.5);
  border-bottom: 1px solid #323d54;
}
</style>
