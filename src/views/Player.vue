<template>
    <div>
        <div class="audio green-audio-player">
            <div class="loading">
                <div class="spinner"></div>
            </div>
            <div class="play-pause-btn">
                <svg xmlns="http://www.w3.org/2000/svg" width="18" height="24" viewBox="0 0 18 24">
                <path fill="#566574" fill-rule="evenodd" d="M18 12L0 24V0" class="play-pause-icon" id="playPause"/>
                </svg>
            </div>

            <div class="controls">
                <span class="current-time">0:00</span>
                <div class="slider" data-direction="horizontal">
                <div class="progress">
                    <div class="pin" id="progress-pin" data-method="rewind"></div>
                </div>
                </div>
                <span class="total-time">0:00</span>
            </div>

            <div class="volume">
                <div class="volume-btn">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
                    <path fill="#566574" fill-rule="evenodd" d="M14.667 0v2.747c3.853 1.146 6.666 4.72 6.666 8.946 0 4.227-2.813 7.787-6.666 8.934v2.76C20 22.173 24 17.4 24 11.693 24 5.987 20 1.213 14.667 0zM18 11.693c0-2.36-1.333-4.386-3.333-5.373v10.707c2-.947 3.333-2.987 3.333-5.334zm-18-4v8h5.333L12 22.36V1.027L5.333 7.693H0z" id="speaker"/>
                </svg>
                </div>
                <div class="volume-controls hidden">
                <div class="slider" data-direction="vertical">
                    <div class="progress">
                    <div class="pin" id="volume-pin" data-method="changeVolume"></div>
                    </div>
                </div>
                </div>
            </div>

            <audio crossorigin>
            <source src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/355309/Swing_Jazz_Drum.mp3" type="audio/mpeg">            </audio>
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

    window.addEventListener('mousedown', function(event) {
      if (!isDraggable(event.target)) return false;

      currentlyDragged = event.target;
      const handleMethod = currentlyDragged.dataset.method;

      window.addEventListener('mousemove', window[handleMethod], false);

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
.audio.green-audio-player {
  width: 100%;
  min-width: 300px;
  height: 56px;
  box-shadow: 0 4px 16px 0 rgba(0, 0, 0, .07);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 24px;
  padding-right: 24px;
  border-radius: 4px;
  user-select: none;
  -webkit-user-select: none;
  background-color: #fff;
  .play-pause-btn {
    display: none;
    cursor: pointer;
  }
  .spinner {
    width: 18px;
    height: 18px;
    background-image: url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/355309/loading.png);
    background-size: cover;
    background-repeat: no-repeat;
    animation: spin 0.4s linear infinite;
  }
  .slider {
    flex-grow: 1;
    background-color: #D8D8D8;
    cursor: pointer;
    position: relative;
    .progress {
      background-color: #000;
      border-radius: inherit;
      position: absolute;
      pointer-events: none;
      .pin {
        height: 16px;
        width: 16px;
        border-radius: 8px;
        background-color: #44BFA3;
        position: absolute;
        pointer-events: all;
        box-shadow: 0px 1px 1px 0px rgba(0,0,0,0.32);
      }
    }
  }
  .controls {
    font-family: 'Roboto', sans-serif;
    font-size: 16px;
    line-height: 18px;
    color: #55606E;
    display: flex;
    flex-grow: 1;
    justify-content: space-between;
    align-items: center;
    margin-left: 24px;
    margin-right: 24px;
    .slider {
      margin-left: 16px;
      margin-right: 16px;
      border-radius: 2px;
      height: 4px;
      .progress {
        width: 0;
        height: 100%;
        .pin {
          right: -8px;
          top: -6px;
        }
      }
    }
    span {
      cursor: default;
    }
  }
  .volume {
    position: relative;
    .volume-btn {
      cursor: pointer;
      &.open path {
        fill: #44BFA3;
      }
    }
    .volume-controls {
      width: 30px;
      height: 135px;
      background-color: rgba(0, 0, 0, 0.62);
      border-radius: 7px;
      position: absolute;
      left: -3px;
      bottom: 52px;
      flex-direction: column;
      align-items: center;
      display: flex;
      &.hidden {
        display: none;
      }
      .slider {
        margin-top: 12px;
        margin-bottom: 12px;
        width: 6px;
        border-radius: 3px;
        .progress {
          bottom: 0;
          height: 100%;
          width: 6px;
          .pin {
            left: -5px;
            top: -8px;
          }
        }
      }
    }
  }
}
svg, img {
  display: block;
}

html, body {
  height: 100%;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  background: #F8FFAE;
  background: -webkit-linear-gradient(-65deg, #43C6AC, #F8FFAE);
  background: linear-gradient(-65deg, #43C6AC, #F8FFAE);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

@keyframes spin {
  from {
    transform: rotateZ(0);
  }
  to {
    transform: rotateZ(1turn);
  }
}

</style>
