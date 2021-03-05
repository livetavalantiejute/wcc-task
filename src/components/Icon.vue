<template>
  <svg
    version="1.1"
    xmlns="http://www.w3.org/2000/svg"
    x="0px"
    y="0px"
    :viewBox="viewBox"
    xml:space="preserve"
    :width="width"
    :height="height"
    :fill="fill"
  >
    <slot />
  </svg>
</template>

<script>
export default {
  props: {
    width: {
      type: [Number, String],
      default: 27,
    },
    height: {
      type: [Number, String],
      default: 25,
    },
    fill: {
      type: String,
      default: 'currentColor',
    },
    viewBox: {
      type: [Number, String],
      default: "0 0 512 512"
    }
  },
};
</script>