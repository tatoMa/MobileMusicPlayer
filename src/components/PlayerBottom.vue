<template>
    <div class="player-bottom change-state">
        <div class="player-container">
            <div class="cover">
                <img class="cover change-state" src='https://images.pexels.com/photos/995301/pexels-photo-995301.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940' alt="cover">
            </div>
            <div class="info change-state">
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
            <div class="buttons-group change-buttons">
                <span class="mdi mdi-loop"></span>
                <span class="mdi mdi-skip-backward"></span>
                <span class="mdi mdi-pause"></span>
                <span class="mdi mdi-skip-forward"></span>
                <span class="mdi mdi-shuffle"></span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  mounted() {
    const audioPlayer = document.querySelector('.green-audio-player');
    const playPause = audioPlayer.querySelector('#playPause');
    const playpauseBtn = audioPlayer.querySelector('.play-pause-btn');
    const loading = audioPlayer.querySelector('.loading');
    const progress = audioPlayer.querySelector('.progress');
    const sliders = audioPlayer.querySelectorAll('.slider');
    const volumeBtn = audioPlayer.querySelector('.volume-btn');
    const volumeControls = audioPlayer.querySelector('.volume-controls');
    const volumeProgress = volumeControls.querySelector('.slider .progress');
    const player = audioPlayer.querySelector('audio');
    const currentTime = audioPlayer.querySelector('.current-time');
    const totalTime = audioPlayer.querySelector('.total-time');
    const speaker = audioPlayer.querySelector('#speaker');

    const draggableClasses = ['pin'];
    let currentlyDragged = null;

    window.addEventListener('mousedown', (event) => {
      if (!isDraggable(event.target)) return false;

      currentlyDragged = event.target;
      const handleMethod = currentlyDragged.dataset.method;

      this.addEventListener('mousemove', window[handleMethod], false);

      window.addEventListener('mouseup', () => {
        currentlyDragged = false;
        window.removeEventListener('mousemove', window[handleMethod], false);
      }, false);
    });

    playpauseBtn.addEventListener('click', togglePlay);
    player.addEventListener('timeupdate', updateProgress);
    player.addEventListener('volumechange', updateVolume);
    player.addEventListener('loadedmetadata', () => {
      totalTime.textContent = formatTime(player.duration);
    });
    player.addEventListener('canplay', makePlay);
    player.addEventListener('ended', () => {
      playPause.attributes.d.value = 'M18 12L0 24V0';
      player.currentTime = 0;
    });

    volumeBtn.addEventListener('click', () => {
      volumeBtn.classList.toggle('open');
      volumeControls.classList.toggle('hidden');
    });

    window.addEventListener('resize', directionAware);

    sliders.forEach((slider) => {
      const pin = slider.querySelector('.pin');
      slider.addEventListener('click', window[pin.dataset.method]);
    });

    directionAware();

    function isDraggable(el) {
      let canDrag = false;
      const classes = Array.from(el.classList);
      draggableClasses.forEach((draggable) => {
        if (classes.indexOf(draggable) !== -1) { canDrag = true; }
      });
      return canDrag;
    }

    function inRange(event) {
      const rangeBox = getRangeBox(event);
      const rect = rangeBox.getBoundingClientRect();
      const { direction } = rangeBox.dataset;
      if (direction == 'horizontal') {
        var min = rangeBox.offsetLeft;
        var max = min + rangeBox.offsetWidth;
        if (event.clientX < min || event.clientX > max) return false;
      } else {
        var min = rect.top;
        var max = min + rangeBox.offsetHeight;
        if (event.clientY < min || event.clientY > max) return false;
      }
      return true;
    }

    function updateProgress() {
      const current = player.currentTime;
      const percent = (current / player.duration) * 100;
      progress.style.width = `${percent}%`;

      currentTime.textContent = formatTime(current);
    }

    function updateVolume() {
      volumeProgress.style.height = `${player.volume * 100}%`;
      if (player.volume >= 0.5) {
        speaker.attributes.d.value = 'M14.667 0v2.747c3.853 1.146 6.666 4.72 6.666 8.946 0 4.227-2.813 7.787-6.666 8.934v2.76C20 22.173 24 17.4 24 11.693 24 5.987 20 1.213 14.667 0zM18 11.693c0-2.36-1.333-4.386-3.333-5.373v10.707c2-.947 3.333-2.987 3.333-5.334zm-18-4v8h5.333L12 22.36V1.027L5.333 7.693H0z';
      } else if (player.volume < 0.5 && player.volume > 0.05) {
        speaker.attributes.d.value = 'M0 7.667v8h5.333L12 22.333V1L5.333 7.667M17.333 11.373C17.333 9.013 16 6.987 14 6v10.707c2-.947 3.333-2.987 3.333-5.334z';
      } else if (player.volume <= 0.05) {
        speaker.attributes.d.value = 'M0 7.667v8h5.333L12 22.333V1L5.333 7.667';
      }
    }

    function getRangeBox(event) {
      let rangeBox = event.target;
      const el = currentlyDragged;
      if (event.type == 'click' && isDraggable(event.target)) {
        rangeBox = event.target.parentElement.parentElement;
      }
      if (event.type == 'mousemove') {
        rangeBox = el.parentElement.parentElement;
      }
      return rangeBox;
    }

    function getCoefficient(event) {
      const slider = getRangeBox(event);
      const rect = slider.getBoundingClientRect();
      let K = 0;
      if (slider.dataset.direction == 'horizontal') {
        const offsetX = event.clientX - slider.offsetLeft;
        const width = slider.clientWidth;
        K = offsetX / width;
      } else if (slider.dataset.direction == 'vertical') {
        const height = slider.clientHeight;
        const offsetY = event.clientY - rect.top;
        K = 1 - offsetY / height;
      }
      return K;
    }

    function rewind(event) {
      if (inRange(event)) {
        player.currentTime = player.duration * getCoefficient(event);
      }
    }

    function changeVolume(event) {
      if (inRange(event)) {
        player.volume = getCoefficient(event);
      }
    }

    function formatTime(time) {
      const min = Math.floor(time / 60);
      const sec = Math.floor(time % 60);
      return `${min}:${(sec < 10) ? (`0${sec}`) : sec}`;
    }

    function togglePlay() {
      if (player.paused) {
        playPause.attributes.d.value = 'M0 0h6v24H0zM12 0h6v24h-6z';
        player.play();
      } else {
        playPause.attributes.d.value = 'M18 12L0 24V0';
        player.pause();
      }
    }

    function makePlay() {
      playpauseBtn.style.display = 'block';
      loading.style.display = 'none';
    }

    function directionAware() {
      if (window.innerHeight < 250) {
        volumeControls.style.bottom = '-54px';
        volumeControls.style.left = '54px';
      } else if (audioPlayer.offsetTop < 154) {
        volumeControls.style.bottom = '-164px';
        volumeControls.style.left = '-3px';
      } else {
        volumeControls.style.bottom = '52px';
        volumeControls.style.left = '-3px';
      }
    }
  },
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
            .change-state{
                animation: cover-move 1s linear forwards, cover-rotation-big 5s linear 1s infinite;
            }
            .active{
                animation: cover-rotation-small 5s linear infinite;
            }
        }
        .buttons{
            margin-right: 15px;
            .mdi{
                font-size: 38px
            }
        }
        .buttons-group{
            display: none;
            text-align: center;
            width: 100%;
            .mdi{
                font-size: 38px;
                padding-right: 13px;
                padding-left: 13px;
            }
        }
        .change-buttons{
            display:block;
            animation: buttons-group-move 0.2s forwards 1s;
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
        .change-state{
            .details{
                animation: title-move 1s ease-in forwards;
            }
            .progress-bar{
                animation: progress-move 1s ease-in forwards;
            }
        }
    }
}
.change-state{
    background-color: rgba(255, 255, 255, 0);
    height: 90px;
    width: calc(100%-20px);
    border-radius: 50px;
    box-shadow: 0 0 0;
}
@keyframes cover-rotation-small {
    0%   {transform: rotate(0deg);}
    100% {transform: rotate(360deg)}
}
@keyframes cover-rotation-big {
    0%   {transform: rotate(90deg) scale(3);}
    100% {transform: rotate(450deg) scale(3)}
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
        transform: rotate(90deg) scale(3);
    }
}
@keyframes title-move {
    0% {
        position: absolute;
        top: 0px;
        left: 0px;
        transform: scale(1);
    }
    100% {
        position: absolute;
        text-align: center;
        top: -120px;
        left: 50%;
        transform: translate(-50%, -50%) scale(1.5);
    }
}
@keyframes progress-move {
    0% {
        position: absolute;
        top: 0px;
        left: 0px;
        transform: scale(1);
    }
    100% {
        position: absolute;
        text-align: center;
        top: -50px;
        left: 50%;
        width: 50%;
        transform: translate(-50%, -50%) scale(1.5);
    }
}
@keyframes buttons-group-move {
    0% {
        position: absolute;
        top: 0px;
        left: 0px;
        opacity: 0
    }
    100% {
        position: absolute;
        op: -120px;
        left: 50%;
        transform: translate(-50%, -50%);
        opacity: 1;
    }
}
</style>
