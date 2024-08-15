<template>
  <Clicker v-bind="$attrs">
    <slot></slot>
  </Clicker>
</template>

<script setup>
import { ref, h, defineComponent } from "vue";
const props = defineProps({ as: { type: String, default: "div" } });

defineOptions({
  name: "ClickScaler",
});

let zIndex = ref(0);
const Clicker = defineComponent({
  render() {
    return h(
      props.as,
      {
        class: "scaler",
        style: {
          transform: `scale(${scaleRatio.value})`,
          zIndex: zIndex.value,
        },
        onClick: increment,
      },
      this.$slots.default()
    );
  },
});

const scaleRatio = ref(1);
let timerId = null;
let timerId2 = null;
function increment() {
  scaleRatio.value *= 1.5;
  zIndex.value = 1;
  if (timerId !== null) {
    clearTimeout(timerId);
  }
  if (timerId2 !== null) {
    clearTimeout(timerId2);
  }
  timerId = setTimeout(() => {
    scaleRatio.value = 1;
    timerId2 = setTimeout(() => {
      zIndex.value = 0;
    }, 500);
  }, 500);
}
</script>

<style scoped>
.scaler {
  transition: transform 0.5s ease;
  cursor: pointer;
}
</style>
