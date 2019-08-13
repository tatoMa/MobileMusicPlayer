<template>
  <div id="app">
    <div id="footer">
      <router-link to="/"><span class="mdi mdi-playlist-music "></span></router-link>
      <router-link to="/"><span class="mdi mdi-folder-outline "></span></router-link>
      <router-link to="/"><span class="mdi mdi-home  active"></span></router-link>
      <router-link to="/"><span class="mdi mdi-magnify "></span></router-link>
      <router-link to="/"><span class="mdi mdi-podcast "></span></router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
@import url(https://cdn.materialdesignicons.com/3.9.97/css/materialdesignicons.min.css);
body {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", Roboto, Ubuntu;
  background-color:#FAFAFA;
  // ::-webkit-scrollbar {
  //   width: 0px;  /* Remove scrollbar space */
  //   background: transparent;  /* Optional: just make scrollbar invisible */
  // }
  // /* Optional: show position indicator in red */
  // ::-webkit-scrollbar-thumb {
  //     background: #FF0000;
  // }
}
#footer {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  text-align: center;
  padding-bottom: 1rem;
}
.mdi{
  color:#797979;
  text-decoration: none;
  font-size: 30px;
  padding:.9rem;
}
.mdi.active{
  color:#50A5F4;
}
</style>
