<template>
  <div>
    <Phone />
    <slot></slot>
  </div>
</template>

<script>
import Phone from "./../../../../icon/Phone";
export default { components: { Phone } };
</script>

<style scoped>
svg,
svg path {
  width: 14px;
  height: 14px;
  fill: #00a5f4 !important;
  margin-right: 3px;
}
</style>
