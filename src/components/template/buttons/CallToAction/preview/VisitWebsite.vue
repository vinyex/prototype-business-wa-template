<template>
  <div>
    <NewTab />
    <slot></slot>
  </div>
</template>

<script>
import NewTab from "./../../../../icon/NewTab";
export default { components: { NewTab } };
</script>

<style scoped>
svg {
  width: 14px;
  height: 14px;
  fill: #00a5f4;
  margin-right: 3px;
}
</style>
