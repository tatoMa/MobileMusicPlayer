<template>
  <div class="home">
    <PopularRadioStations/>
    <TrendingAlbums/>
  </div>
</template>

<script>
import PopularRadioStations from '@/components/PopularRadioStations.vue';
import TrendingAlbums from '@/components/TrendingAlbums.vue';

export default {
  name: 'home',
  components: {
    PopularRadioStations,
    TrendingAlbums,
  },
};
</script>
