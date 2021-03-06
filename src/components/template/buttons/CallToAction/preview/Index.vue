<template>
  <div class="wrapper-outter">
    <div class="inner">
      <component
        v-for="(btn, i) in buttonsList"
        :key="i"
        :is="btn.component"
        class="btn-preview"
        >{{ btn.button }}</component
      >
    </div>
  </div>
</template>

<script>
import CallPhone from "./CallPhone";
import VisitWebsite from "./VisitWebsite";

export default {
  components: { CallPhone, VisitWebsite },
  props: {
    buttonsList: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style scoped>
.wrapper-outter {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.inner {
  border-top: 1px solid #dadde1;
  margin: 0 7px 0 9px;
  min-width: 90%;
}

.btn-preview {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  color: #00a5f4;
  font-size: 13px;
  height: 34px;
  line-height: 20px;
  padding: 0 16px;
  white-space: pre-wrap;
}

.btn-preview * {
  margin: auto;
}
</style>
