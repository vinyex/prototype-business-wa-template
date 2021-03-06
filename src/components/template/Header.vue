<template>
  <div>
    <div class="wrapper w-100">
      <div class="d-flex flex-row justify-content-between align-items-center">
        <div>{{ title }}</div>
        <div class="d-flex flex-row justify-content-between align-items-center">
          <button class="btn" @click.passive="$emit('secondaryAction')">{{ secondaryBtnText }}</button>
          <button
            class="btn primary"
            :class="{
              'good-togo': validateNextBtn,
            }"
            :disabled="!validateNextBtn"
            @click.passive="validateNextBtn ? $emit('mainAction') : false"
          >
            {{ mainBtnText }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    mainBtnText: {
      type: String,
      default: "",
    },
    secondaryBtnText: {
      type: String,
      default: "Batalkan",
    },
    title: {
      type: String,
      default: "",
    },
    validateNextBtn: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style scoped>
div {
  box-sizing: border-box;
}

button:disabled {
  cursor: 'not-allowed';
  opacity:  0.4;
}

.wrapper {
  position: sticky;
  top: 0;
  padding: 12px 24px;
  background-color: #fff;
}

.primary {
  background-color: rgba(24, 119, 242, 1);
  color: white;
  border: none;
  border-radius: 3px;
  padding: 5px 17px;
}

.good-togo {
  opacity: unset;
}
</style>
