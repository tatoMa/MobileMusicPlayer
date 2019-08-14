<template>
  <div class="home">
    <div class="home-main">
      <PopularRadioStations class="inactive"/>
      <TrendingAlbums class="inactive"/>
      <TrendingAlbums class="inactive"/>
      <TrendingAlbums class="inactive" style="margin-bottom:150px"/>
      <div class="bottom">
        <PlayerBottom/>
        <FooterBottom class="inactive"/>
      </div>
    </div>
    <div class="home-player">

    </div>
  </div>
</template>

<script>
import PopularRadioStations from '@/components/PopularRadioStations.vue';
import TrendingAlbums from '@/components/TrendingAlbums.vue';
import PlayerBottom from '@/components/PlayerBottom.vue';
import FooterBottom from '@/components/FooterBottom.vue';

export default {
  name: 'home',
  components: {
    PopularRadioStations,
    TrendingAlbums,
    PlayerBottom,
    FooterBottom,
  },
};
</script>
<style scoped lang="scss">
.bottom{
  position: fixed;
  left: 0px;
  bottom: 0px;
  width: 100%;
  z-index: 5;
  background-color:#FAFAFA;
}
.inactive{
  animation: hide 0.5s linear forwards;
  // opacity:0;
}
@keyframes hide{
    0%   {opacity:100}
    100% {opacity:0}
}
// .home{
//   display: flex;
//   flex-direction: column;
//   justify-content: center;
//   align-content: center;
// }
</style>
