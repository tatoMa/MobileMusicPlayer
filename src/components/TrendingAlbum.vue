<template>
  <div>
    <div>
      <div class="card">
        <div class="content">
          <img class="cover" :src=radio.url alt="">
          <div class="title">{{radio.title}}</div>
          <div class="followers">{{radio.follower}} Followers</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PopularRadioStations',
  props: {
    radio: Object,
  },
  // mounted () {
  //   console.log(this.radio);
  // },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.card{
  background-color: #FFFFFF;
  width: 130px;
  height: 110px;
  margin-top:60px;
  text-align: center;
  box-shadow: 0px 0px 30px #C4E2FF;
  border-radius: 10px;
  position: relative;
  margin-left: 13px;
    .content{
      position: absolute;
      top:-45px;
      left:20px;
      color: #646464;
      .cover{
        height:90px;
        width:90px;
        border-radius: 50%;
        z-index: 100;
      }
      .title{
        margin-top: 9px;
        font-size: .9rem;
        font-weight: 500;
      }
      .followers{
        font-size: .7rem;
        margin-top: 4px;
        font-weight: 200;
      }
    }
}
</style>
