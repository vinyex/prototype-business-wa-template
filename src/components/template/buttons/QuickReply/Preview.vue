<template>
  <div class="absolute w-100">
    <div class="d-flex flex-row align-items-stretch">
      <div
        v-for="btn in buttonsList"
        :key="`reply-val-${btn}`"
        class="btn btn-default justify-content-center"
      >
        <span>{{ btn }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    buttonsList: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style scoped>
.absolute {
  position: absolute;
}

.d-flex {
  flex-wrap: wrap;
}

.btn-default {
  display: flex;
  flex: 1 1 65px;
  width: 100%;
  font-size: 0.9em;
  background-color: #fff;
  color: #00a5f4;
  margin: 1.75px;
  text-align: center;
}

.btn-default:hover {
  background-color: rgba(255, 255, 255, 0.8);
}
</style>
