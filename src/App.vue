<template>
  <div id="app">
    <div class="">
      <Banner></Banner>


    </div>
    <b-container>
<!--      <Alert></Alert>-->
      <forminfo></forminfo>
    </b-container>
  </div>
</template>

<script>

import Banner from "./components/Banner";
import forminfo from "./components/forminfo"
import Alert from "./components/Alert";

export default {
  name: 'App',
  components: {
    Banner,forminfo,Alert
  },
}
</script>

<style>

</style>
