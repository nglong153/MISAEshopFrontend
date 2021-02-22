<template>
  <div id="app">
      <Header />
      <div class="navbar-content-wrap-container" style="
    width: 100%;
    height: calc(100vh - 60px);">
        <Navbar />
        <Content />
      </div>
  </div>
</template>

<script>
import Navbar from "@/components/layout/Navbar";
import Header from "@/components/layout/Header";
import Content from "@/components/layout/Content";

export default {
  name: "App",

  components: {
    Navbar,
    Header,
    Content,
  },

  data: () => ({
    //
  }),
};
</script>

<style lang='scss'>
@import "./styles/index.scss";

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50 !important;
}

#nav a.router-link-exact-active {
  color: #00577b;
}

.navbar-content-wrap-container{
  width: 100%;
  height: calc(100vh - 66px);
  display: flex;
}

.navbar .navbar-content .nav-item.router-link-exact-active {
  background-color: #026b97;
}
.navbar .navbar-content .nav-item.router-link-exact-active:focus{
  background-color : #00577b;
}

</style>
