<template lang="">
  <div class="col-md-8 offset-md-2">
    <form class="form-inline">
      <input required v-model="from" type="date" class="form-control mb-2 mr-sm-2 mb-sm-0" id="inlineFormInput" placeholder="From">
      <input required v-model="to" type="date" class="form-control mb-2 mr-sm-2 mb-sm-0" id="inlineFormInputGroup" placeholder="To">
      <input v-model="packages" type="text" class="form-control" id="inlineFormInputGroup" placeholder="Packages">
      <button type="button" class="btn btn-primary form-control" v-on:click="getList ()">Submit</button>
    </form>
    <div class="col-md-8">
      <chart :chartData="downloads" :labels="labels" v-if="showChart"></chart>
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
      from: '',
      to: '',
      packages: '',
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
    // this.getList()
  }
}
</script>
