<template>
  <div class="flexy-flex">

    <h1>Create a Thing</h1>

    <div>
      <b-button class="mt-3" @click="startProgress" variant="success">Start</b-button>
      <b-button class="mt-3" @click="pauseProgress" variant="warning">Pause</b-button>

      <transition name="fade">
        <span v-if="!completed">
          <b-button class="mt-3" @click="cancelProgress" variant="secondary">Cancel</b-button>
        </span>
        <span v-else>
          <b-button class="mt-3" @click="cancelProgress" variant="danger">Delete</b-button>
        </span>
      </transition>
    </div>

    <div class="break"></div>

    <transition name="fade-in">
      <span v-if="!completed && showBox" class="empty-box"></span>
    </transition>

    <transition name="fade">
      <span v-if="completed && !showBox">
        <b-img src="~/assets/congratulations.jpg" fluid></b-img>
      </span>
    </transition>

    <div class="break"></div>
    <transition name="fade">
        <div v-if="loading" class="full-width">
          <b-progress>
            <b-progress-bar :value="value" :label="`${((value / max) * 100).toFixed(2)}%`" animated></b-progress-bar>
          </b-progress>
        </div>
    </transition>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        value: 0,
        max: 100,
        paused: false,
        completed: false,
        loading: false,
        showBox: true
      }
    },
    methods: {
      startProgress() {
        console.log(this.completed);
        console.log(this.showBox);
        this.paused = false;
        this.loading = true;
        this.showBox = true;

        let progressBarInterval = setInterval(() => {
          if (this.value >= 100) {
            this.value = this.max; // In case if somehow exceeds max
            clearInterval(progressBarInterval);
          } else if (this.paused) {
            clearInterval(progressBarInterval);
          } else {
            let computedValue = (this.value + 1) + (this.value * 0.1);
            this.value = (computedValue >= this.max) ? this.max : computedValue;
          }
        }, 200);
      },
      pauseProgress() {
        this.paused = true;
      },
      cancelProgress() {
        this.value = 0;
        this.loading = false;
        this.paused = true;
        this.completed = false;
      }
    },
    watch: {
      value: function(val) {
        this.completed = (this.value >= this.max);
      },
      completed: function(val) {
        this.loading = false;

        if (!val) {
          setTimeout(() => {
            console.log('yes');
            this.showBox = true;
          }, 1100);
        } else {
          this.showBox = false;
        }
      }
    }
  }
</script>

<style scoped>
  .flexy-flex {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    width: 300px;
    margin: auto;
  }

  .break {
    flex-basis: 100%;
    height: 0;
    margin: 8px 0px 8px 0px;
  }

  .full-width {
    width: 100%;
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity 1.0s;
  }

  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }

  .fade-in-enter-active {
    transition: opacity 1.0s;
  }

  .fade-in-enter, .fade-in-leave-to {
    opacity: 0;
  }

  .empty-box {
    height: 269px;
    width: 300px;
    border: 1px solid black;
  }
</style>
