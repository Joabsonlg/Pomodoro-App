<template>
  <div class="control my-4">
    <label>
      <input type="radio" value="300" v-model="time" @click="startTimer" />
      <span> 5 </span>
    </label>
    <label>
      <input type="radio" value="900" v-model="time" @click="startTimer" />
      <span>15</span>
    </label>
    <label>
      <input type="radio" value="1500" v-model="time" @click="startTimer" />
      <span>25</span>
    </label>
  </div>
  <div class="base-timer">
    <svg
      class="base-timer__svg"
      viewBox="0 0 100 100"
      xmlns="http://www.w3.org/2000/svg"
    >
      <g class="base-timer__circle">
        <circle
          class="base-timer__path-elapsed"
          cx="50"
          cy="50"
          r="45"
        ></circle>
        <path
          :stroke-dasharray="circleDasharray"
          class="base-timer__path-remaining"
          :class="remainingPathColor"
          d="
            M 50, 50
            m -45, 0
            a 45,45 0 1,0 90,0
            a 45,45 0 1,0 -90,0
          "
        ></path>
      </g>
    </svg>
    <span class="base-timer__label">{{ formattedTimeLeft }}</span>
  </div>
  <div class="buttons is-centered">
    <button
      class="button is-primary is-rounded is-inverted is-outlined"
      disabled
      @click="stopTimer"
      id="stop"
    >
      <i class="fa fa-stop"></i>
    </button>
    <button
      class="button is-primary is-rounded is-inverted is-outlined"
      disabled
      @click="playPauseTimer"
      id="playPause"
    >
      <i class="fa fa-play"></i>/<i class="fa fa-pause"></i>
    </button>
  </div>
</template>

<script>
const FULL_DASH_ARRAY = 283;
const WARNING_THRESHOLD = 10;
const ALERT_THRESHOLD = 5;

const COLOR_CODES = {
  info: {
    color: "green",
  },
  warning: {
    color: "orange",
    threshold: WARNING_THRESHOLD,
  },
  alert: {
    color: "red",
    threshold: ALERT_THRESHOLD,
  },
};

export default {
  data() {
    return {
      time: 0,
      timePassed: 0,
      timerInterval: null,
      soundurl:
        "http://soundbible.com/mp3/analog-watch-alarm_daniel-simion.mp3",
      isPaused: false,
    };
  },

  computed: {
    circleDasharray() {
      return `${(this.timeFraction * FULL_DASH_ARRAY).toFixed(0)} 283`;
    },

    audio() {
      return new Audio(this.soundurl);
    },

    formattedTimeLeft() {
      const timeLeft = this.timeLeft;
      const minutes = Math.floor(timeLeft / 60);
      let seconds = timeLeft % 60;

      if (seconds < 10) {
        seconds = `0${seconds}`;
      }

      return `${minutes}:${seconds}`;
    },

    timeLeft() {
      return this.time - this.timePassed;
    },

    timeFraction() {
      const rawTimeFraction = this.timeLeft / this.time;
      return rawTimeFraction - (1 / this.time) * (1 - rawTimeFraction);
    },

    remainingPathColor() {
      const { alert, warning, info } = COLOR_CODES;

      if (this.timeLeft <= alert.threshold) {
        return alert.color;
      } else if (this.timeLeft <= warning.threshold) {
        return warning.color;
      } else {
        return info.color;
      }
    },
  },

  watch: {
    timeLeft(newValue) {
      if (newValue === 0) {
        this.onTimesUp();
        $("input[type='radio']").prop("checked", false);
        $("input[type='radio']").prop("disabled", false);
        $("#stop").prop("disabled", true);
        $("#playPause").prop("disabled", true);
      }
      if (newValue === 12) {
        this.audio.play();
      }
    },
  },

  methods: {
    onTimesUp() {
      clearInterval(this.timerInterval);
    },

    startTimer() {
      $("input[type='radio']").prop("disabled", true);
      $("#stop").prop("disabled", false);
      $("#playPause").prop("disabled", false);
      this.onTimesUp();
      this.timePassed = 0;
      this.timerInterval = setInterval(
        function () {
          if (!this.isPaused) {
            this.timePassed++;
          }
        }.bind(this),
        1000
      );
    },

    stopTimer() {
      if (this.timeLeft <= 12) {
        this.audio.pause();
        this.audio.currentTime = 0;
      }
      this.onTimesUp();
      this.timePassed = 0;
      this.time = 0;
    },

    pauseTimer() {
      if (this.timeLeft <= 12) this.audio.pause();
      this.isPaused = true;
    },

    playTimer() {
      if (this.timeLeft <= 12) this.audio.play();
      this.isPaused = false;
    },

    playPauseTimer() {
      if (this.isPaused) {
        this.playTimer();
      } else {
        this.pauseTimer();
      }
    },
  },
};
</script>

<style scoped>
.control label input[type="radio"] {
  appearance: none;
  -webkit-appearance: none;
}
.control label input[type="radio"]:checked ~ span {
  color: #00fff1;
  box-shadow: inset -1px -1px 3px rgba(255, 255, 255, 0.1),
    inset 2px 2px 6px rgba(0, 0, 0, 1);
  text-shadow: 0 0 5px #00fff1, 0 0 20px #00fff1;
}
.control label span {
  position: relative;
  padding: 10px;
  font-size: 2rem;
  line-height: 40px;
  text-align: center;
  margin: 0 4px;
  border-radius: 50%;
  cursor: pointer;
  box-shadow: -1px -1px 3px rgba(255, 255, 255, 0.1),
    2px 2px 6px rgba(0, 0, 0, 0.8);
}
.control label span:hover {
  box-shadow: -1px -1px 3px rgba(255, 255, 255, 0.1),
    2px 2px 6px rgba(0, 0, 0, 0.8),
    inset -2px -2px 10px rgba(255, 255, 255, 0.05),
    inset 2px 2px 10px rgba(0, 0, 0, 0.8);
}
</style>

<style scoped lang="scss">
.base-timer {
  position: relative;
  width: 300px;
  height: 300px;
  margin-left: auto;
  margin-right: auto;

  &__svg {
    transform: scaleX(-1);
  }

  &__circle {
    fill: none;
    stroke: none;
  }

  &__path-elapsed {
    stroke-width: 7px;
    stroke: grey;
  }

  &__path-remaining {
    stroke-width: 7px;
    stroke-linecap: round;
    transform: rotate(90deg);
    transform-origin: center;
    transition: 1s linear all;
    fill-rule: nonzero;
    stroke: currentColor;

    &.green {
      color: rgb(65, 184, 131);
    }

    &.orange {
      color: orange;
    }

    &.red {
      color: red;
    }
  }

  &__label {
    position: absolute;
    width: 300px;
    height: 300px;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 48px;
  }
}
</style>
