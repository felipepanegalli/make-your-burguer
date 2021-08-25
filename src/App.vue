<template>
  <div>
    <Navbar :logo="logo_scr" :alt="app_name" />
    <router-view />
    <Footer />
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";

export default {
  components: {
    Navbar,
    Footer,
  },
  data() {
    return {
      logo_scr: "/img/logo.png",
      app_name: "Make Your Burger",
    };
  },
};
</script>

<style>
* {
  font-family: "Roboto", sans-serif;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-weight: 300;
}

.main-container {
  margin: 50px;
  min-height: calc(100vh - 276px);
}

h1 {
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>
