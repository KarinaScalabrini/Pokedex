<template>
  <HeaderPage/>
  <ContainerPage/>
</template>

<script>
import HeaderPage from './components/HeaderPage.vue';
import ContainerPage from './components/ContainerPage.vue';

export default {
  name: 'App',
  components: {
    HeaderPage,
    ContainerPage
  }
}
</script>

<style>
body {
  width:100%;
  height:100%;
  padding: 0px;
  margin:0px;
  padding:0px;
  background-image: url("./assets/fundo.png");
  background-repeat: no-repeat;
  background-size: cover;

}
</style>
