<template>
  <div>
    <div class="button-control">
      <label>Button Text</label>
      <div class="wrapper-button-counter d-flex flex-row">
        <div class="d-flex flex-row align-items-center">
          <div class="mr-2">
            <input
              type="text"
              class="input-button-text"
              :maxlength="maxButton"
              :value="buttonText"
              @input="
                $emit('buttonText', {
                  language: language,
                  value: $event.target.value,
                })
              "
            />
          </div>
          <span class="counter-length">{{
            `${buttonText.length}/${maxButton}`
          }}</span>
        </div>
        <div
          v-if="deleteAction"
          class="close-icon btn-default"
          @click.passive="$emit('deleteAction', { language: language })"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    buttonText: { type: [String, Object], default: "" },
    deleteAction: { type: Boolean, default: false },
    language: { type: String, default: "" },
  },
  data: () => ({ maxButton: 20 }),
};
</script>

<style scoped>
.button-control {
  display: flex;
  margin: 0;
  flex-direction: column;
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.4;
}

.button-control label {
  font-size: 0.8em;
  font-weight: 700;
  padding: 0;
  margin: 5px 0;
}

.button-control .counter-length {
  font-size: 12px;
  color: gray;
}

.button-control .wrapper-button-counter {
  background: #fff;
  border: 1px solid #c4c4c4;
  font-size: 0.85em;
  padding: 0 5px;
  border-radius: 5px;
  align-items: center;
  justify-content: space-around;
  min-height: 38px;
}

.button-control .input-button-text {
  border: unset;
  outline: none;
  width: 100%;
}

.close-icon {
  display: flex;
  margin-left: 7px;
  cursor: pointer;
}
</style>
