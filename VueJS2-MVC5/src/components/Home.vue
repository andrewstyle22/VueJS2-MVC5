<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="Redirect">Go and test the banckend</button>
    <input type="file" id="selectPDF" v-on:change="openPDF" accept="application/pdf" />
    <span class="error">{{errorMsg}}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: "Boilerplate: VueJS 2 + ASP.NET MVC 5",
      pdf: '',
      baseUrl: '',
      errorMsg: '',
    };
  },
  methods: {
    Redirect() {
      this.$router.push("/APITest");
    },
    openPDF(e) {
      console.log('e: ', e);
      this.pdf = window.URL.createObjectURL(e.target.files[0])
    }
  }
};
</script>
<style>
  /* global styles */

  body {
    height: 100vh;
  }

  #app, .container {
    height: 100%;
  }
</style>
