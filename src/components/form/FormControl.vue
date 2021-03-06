<template>
  <div>
    <div class="wrapper-label">
      <label>
        <div class="label">{{ title }}</div>
        <span class="sub-label">{{ titleDescription }}</span>
      </label>
      <slot v-if="$slots.content" name="content"></slot>
      <div v-else>Use slot to change content</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: "",
    },
    titleDescription: {
      type: String,
      default: "",
    },
  },
};
</script>

<style scoped>
label {
  display: flex;
  flex-direction: column;
}

.label {
  margin: 5px;
  font-weight: bold;
}

.sub-label {
  margin: 5px;
  font-size: 12px;
  color: gray;
}

.wrapper-label {
  display: flex;
  margin: 15px 24px;
  padding: 24px;
  background: #fff;
  flex-direction: column;
}
</style>
