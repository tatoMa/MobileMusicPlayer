<template>
    <div class="footer">
      <router-link to="/"><span class="mdi mdi-playlist-music "></span></router-link>
      <router-link to="/"><span class="mdi mdi-folder-outline "></span></router-link>
      <router-link to="/"><span class="mdi mdi-home  active"></span></router-link>
      <router-link to="/"><span class="mdi mdi-magnify "></span></router-link>
      <router-link to="/"><span class="mdi mdi-podcast "></span></router-link>
    </div>
</template>

<script>
export default {

};
</script>

<style lang="scss" scoped>
.footer {
  width: 100%;
  text-align: center;
  padding-bottom: 1.4rem;
  padding-top: 1.6rem;
}
.mdi{
  color:#797979;
  text-decoration: none;
  font-size: 30px;
  padding:.9rem;
  &:hover{
    color:#50A5F4;
  }
}
.mdi.active{
  color:#50A5F4;
}
</style>
