<template lang="">
  <div class="content">
    <div class="container">
      <div class="Search__container">
        <input
        class="Search__input"
        @keyup.enter='requestData'
        placeholder='npm package name'
        type='search' name='search'
        v-model='package'
        >
        <button class='Search__button' @click='requestData'>Find</button>
      </div>
      <div class="error-message" v-if="showError">

      </div>
      <hr>
      <h1 class="title" v-if="loaded"></h1>
      <div class="Chart__container" v-if="loaded">
        <div class="Chart__title">
          Downloads per Day <span></span>
          <hr>
        </div>
        <div class="Chart__content">
          <line-chart v-if="loaded" :chart-data="downloads" :chart-labels="labels"></line-chart>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="">
import LineChart from '@/components/LineChart'

export default {
  components: {
    LineChart
  },
  props: {},
  data () {
    return {
      downloads: [],
      labels: [],
      // from: '2017-05-01',
      // to: '2017-05-02',
      package: null,
      packageName: '',
      period: 'last-month',
      loaded: false,
      showError: false,
      errorMessage: 'Please enter package name'
    }
  },
  mounted () {
    if (this.$route.params.package) {
      this.package = this.$route.params.package
      this.requestData()
    }
  },
  methods: {
    resetState () {
      this.loaded = false
      this.showError = false
    },
    requestData () {
      if (this.package === null || this.package === '' || this.package === 'undefined') {
        this.showError = true
        return
      }
      this.resetState()
      this.$http.get(`https://api.npmjs.org/downloads/range/${this.period}/${this.package}`)
      .then((response) => {
        console.log(response.data)
        this.downloads = response.data.downloads.map(download => download.downloads)
        this.labels = response.data.downloads.map(download => download.day)
        this.packageName = response.data.package
        this.setURL()
        this.loaded = true
      })
      .catch((err) => {
        this.errorMessage = err.response.data.error
        this.showError = true
      })
    },
    setURL () {
      history.pushState({info: `npm-stats ${this.package}`}, this.package, `/#/${this.package}`)
    }
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
