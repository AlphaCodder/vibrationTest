<template>
  <div>
    <h4 class="title">Vibration controller</h4>
    <div class="field">
      <label class="label">Intensity</label>
      <div class="control">
        <input class="slider" type="range" min="0" max="100" v-model="magnitude" @input="updateMagnitude" />
      </div>
    </div>
    <div class="field">
      <label class="label">Control</label>
      <div class="control">
        <button class="button is-primary" type="button" @click="toggle">{{ button }}</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ControllerActive',
  props: ['button'],
  data() {
    return {
      magnitude: 100,
    };
  },
  methods: {
    toggle() {
      this.$emit('toggle');
    },
    updateMagnitude() {
      this.$emit('update:magnitude', this.magnitude);
    },
  },
};
</script>
