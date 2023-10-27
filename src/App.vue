<template>
  <div class="column">
    <template v-if="gamepad">
      <ControllerActive :magnitude="magnitude" @toggle="toggleGo" :button="button" @update:magnitude="updateMagnitude"/>
    </template>
    <template v-else>
      <NoController />
    </template>
  </div>
</template>

<script>
import ControllerActive from './components/ControllerActive.vue';
import NoController from './components/NoController.vue';

export default {
  data() {
    return {
      gamepad: null,
      magnitude: 0,
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
