<template>
  <div>
    <BtnQuickReply
      v-for="(btn, i) in quickReplySelected"
      :key="`quick-reply-btn-${i}`"
      :button-text="btn"
      :delete-action="quickReplySelected.length >= 2"
      :language="activeDataTemplate.language"
      @buttonText="buttonText(i, $event)"
      @deleteAction="deleteAction(i)"
      class="my-2 d-flex flex-row align-items-center"
    />
    <div>
      <div class="d-flex flex-row">
        <button
          class="btn btn-default"
          :disabled="quickReplySelected.length >= 3"
          @click.passive="addAnotherButton"
        >
          <div class="d-flex flex-row w-100">
            <div>&plus; Add Another Button</div>
          </div>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import BtnQuickReply from "./Button";

export default {
  components: {
    BtnQuickReply,
  },
  props: {
    activeDataTemplate: { type: Object, default: () => ({}) },
  },
  data() {
    return {
      quickReplySelected: [""],
    };
  },
  computed: {
    activeQuickReplySelected() {
      return this.activeDataTemplate.buttons
        ? this.activeDataTemplate.buttons.value
        : [];
    },
  },
  watch: {
    activeQuickReplySelected(val) {
      this.quickReplySelected = val;
    },
    quickReplySelected() {
      this.sendQuickReplyData();
    },
  },
  methods: {
    addAnotherButton() {
      this.quickReplySelected.push("");
    },
    buttonText(idx, data) {
      if (this.activeDataTemplate.language === data.language) {
        this.quickReplySelected.splice(idx, 1, data.value);
      }
    },
    deleteAction(index) {
      this.quickReplySelected.splice(index, 1);
    },
    sendQuickReplyData() {
      this.$emit("quickReplyData", this.quickReplySelected);
    },
  },
  mounted() {
    if (this.activeQuickReplySelected.length > 0) {
      this.quickReplySelected = this.activeQuickReplySelected;
      this.sendQuickReplyData();
    }
  },
};
</script>

<style scoped>
.btn-default {
  font-size: 0.8em;
  margin-top: 12px;
  padding: 12px 15px;
}

button:disabled {
  cursor: not-allowed;
}
</style>
