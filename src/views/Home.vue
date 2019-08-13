<template>
  <div class="home">
    <PopularRadioStations/>
    <TrendingAlbums/>
    <TrendingAlbums/>
    <TrendingAlbums style="margin-bottom:150px"/>
    <div class="bottom">
      <PlayerBottom/>
      <FooterBottom/>
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
// .home{
//   display: flex;
//   flex-direction: column;
//   justify-content: center;
//   align-content: center;
// }
</style>
