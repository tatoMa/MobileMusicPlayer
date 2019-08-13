<template>
  <div>
    <h2 class="title">Popular Radio Stations</h2>
    <div class="radio-station-container">
      <div v-for="radio in data" :key="radio.url">
        <PopularRadioStation :radio=radio />
      </div>
    </div>
  </div>
</template>

<script>
import PopularRadioStation from './PopularRadioStation.vue';

export default {
  components: {
    PopularRadioStation,
  },
  name: 'PopularRadioStations',
  data() {
    return {
      data: [
        {
          url: 'https://images.pexels.com/photos/995301/pexels-photo-995301.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940',
          title: 'Rock Band',
          follower: 432124,
        },
        {
          url: 'https://images.pexels.com/photos/167636/pexels-photo-167636.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940',
          title: 'Jazz Band',
          follower: 233223,
        },
        {
          url: 'https://images.pexels.com/photos/2167139/pexels-photo-2167139.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940',
          title: 'Piano',
          follower: 24666,
        },
        {
          url: 'https://images.pexels.com/photos/1918160/pexels-photo-1918160.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940',
          title: 'Pop Band',
          follower: 323456,
        },
      ],
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.title{
  font-weight: 500;
  margin-left: 20px;
  margin-top:20px;
}
.radio-station-container{
  margin-top: 40px;
  display: flex;
  overflow-x: auto;
  height: 230px;
  &::-webkit-scrollbar {
    width: 0 !important;
  }
}
</style>
