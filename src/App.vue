<template>
  <div class="container d-flex h-100">
    <div class="row align-self-center w-100">
      <div class="col-md-6 mx-auto">
        <template v-if="gamepad">
          <ControllerActive :magnitude="magnitude" @toggle="toggleGo" :button="button" @update:magnitude="updateMagnitude"/>
        </template>
        <template v-else>
          <NoController />
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css';
import ControllerActive from './components/ControllerActive.vue';
import NoController from './components/NoController.vue';

export default {
  data() {
    return {
      gamepad: null,
      magnitude: this.magnitude,
      go: false,
    };
  },
  created() {
    const pollGamepads = setInterval(() => {
      const gamepad = navigator.getGamepads()[0];
      if (gamepad) {
        this.gamepad = gamepad;
        clearInterval(pollGamepads);
      }
    }, 400);
  },
  methods: {
    toggleGo() {
      this.go = !this.go;
      if (this.go) this.vibrate();
    },
    updateMagnitude(magnitude) {
      this.magnitude = magnitude;
    },
    vibrate() {
      this.gamepad.vibrationActuator
        .playEffect('dual-rumble', {
          duration: 100,
          strongMagnitude: this.magnitude / 100,
          weakMagnitude: this.magnitude / 100,
        })
        .then(() => {
          if (this.go) this.vibrate();
        })
        .catch((err) => console.log(err));
    },
  },
  computed: {
    button() {
      return this.go ? 'Stop' : 'Go';
    },
  },
  components: {
    ControllerActive,
    NoController,
  },
};
</script>
<style>
body {
  background-color: #f5f5f5;
  margin: 0 auto;
}
</style>