<template>
  <div class="home">
    <PopularRadioStations/>
  </div>
</template>

<script>
// @ is an alias to /src
import PopularRadioStations from '@/components/PopularRadioStations.vue';

export default {
  name: 'home',
  components: {
    PopularRadioStations,
  },
};
</script>
