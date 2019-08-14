<template>
    <div class="player-bottom">
        <div class="player-container">
            <div class="cover">
                <img class="cover move" src='https://images.pexels.com/photos/995301/pexels-photo-995301.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940' alt="cover">
            </div>
            <div class="info">
                <div class="details">
                    <div class="title">this is a song</div>
                    <div class="album">this is the album</div>
                </div>
                <div class="progress-bar">
                    <div class="meter">
                        <span></span>
                        <!-- <span style="width: 50%"></span> -->
                    </div>
                </div>
            </div>
            <div class="buttons">
                <span class="mdi mdi-pause"></span>
                <span class="mdi mdi-skip-next"></span>
            </div>
        </div>
    </div>
</template>

<script>
export default {

};
</script>

<style lang="scss" scoped>
.player-bottom{
    margin:0 10px;
    background-color: #FFFFFF;
    height: 90px;
    width: calc(100%-20px);
    border-radius: 50px;
    box-shadow: 1px 1px 20px rgba(0, 0, 0, 0.05);
    .player-container{
        height: 100%;
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        .cover{
            margin: 5px;
            position: relative;
            // animation: cover-move 1s linear alternate infinite;
            img{
                height: 70px;
                width: 70px;
                border-radius: 50%;
            }
            .move{
                animation: cover-move 2s linear forwards, rotation-big 5s linear 2s infinite;
            }
            .active{
                animation: rotation-small 5s linear infinite;
            }
        }
        .buttons{
            margin-right: 15px;
            .mdi{
                font-size: 38px
            }
        }
        .info{
            width: 48%;
            .details{
                .title{
                    // margin-top: 13px;
                    font-size: 1rem;
                    font-weight: 600;
                    margin-bottom: 5px;
                    color: #646464;
                }
                .album{
                    font-size: .75rem;
                    // margin-top: 4px;
                    font-weight: 200;
                    color: #646464;
                    margin-bottom: 10px;
                }
            }
            .progress-bar{
                .meter {
                    height: 3px;  /* Can be anything */
                    position: relative;
                    background: #EBEBEB;
                    border-radius: 30px;
                    padding: 0px;
                }
                .meter > span {
                    width: 0%;
                    display: block;
                    height: 100%;
                    border-radius: 30px;
                    background-color: #50A5F4;
                    position: relative;
                    overflow: hidden;
                    animation: progress 5s linear infinite
                }
            }
        }
    }
}
@keyframes rotation-small {
    0%   {transform: rotate(0deg);}
    100% {transform: rotate(360deg)}
}
@keyframes rotation-big {
    0%   {transform: rotate(180deg) scale(3);}
    100% {transform: rotate(540deg) scale(3)}
}
@keyframes progress{
    0%   {width: 0%;}
    100% {width: 100%}
}
@keyframes cover-move {
    0%   {
        // position: absolute;
        top: 0px;
        left: 0px;
        position: absolute;
        transform: rotate(0deg) scale(1);
    }
    100% {
        // position: absolute;
        top: -400px;
        left: 150px;
        position: absolute;
        transform: rotate(180deg) scale(3);
    }
}
</style>
