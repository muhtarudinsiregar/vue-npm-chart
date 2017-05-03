<template lang="">
  <div class="container">
    <div class="offset-md-2 col-md-8">
      <form class="form-inline">
        <input required v-model="from" type="date" class="form-control mb-2 mr-sm-2 mb-sm-0" id="inlineFormInput" placeholder="From">

        <input required v-model="to" type="date" class="form-control" id="inlineFormInputGroup" placeholder="To">
        <input v-model="packages" type="text" class="form-control" id="inlineFormInputGroup" placeholder="To">

        <button type="button" class="btn btn-primary" v-on:click="getList ()">Submit</button>
      </form>
    </div>
  </div>
</template>
<script lang="">
export default {
  name: 'npm',
  data () {
    return {
      downloads: '',
      from: '',
      to: '',
      packages: ''
    }
  },
  methods: {
    getList () {
      this.$http.get(`https://api.npmjs.org/downloads/range/${this.from} : ${this.to}/${this.packages}`)
      .then((response) => {
        this.downloads = response
        console.log(this.downloads)
      }).catch((error) => {
        console.log(error.bodyText.error)
      })
    }
  },
  mounted () {
  }
}
</script>
