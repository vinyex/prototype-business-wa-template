<template>
  <div>
    <template v-for="(list, i) in typeActionSelected">
      <component
        :is="list.component"
        :key="`action-type-${activeDataTemplate.language}-${i}`"
        class="my-2 d-flex flex-row align-items-center"
        :button-text="list.button || ''"
        :delete-action="actionTypesLimit"
        :language="activeDataTemplate.language"
        :list="list.text"
        :list-languages="list.component == 'call-phone' ? listLanguages : false"
        :type-actions="typeActions"
        @buttonText="buttonText"
        @deleteAction="deleteAction"
        @selectAction="selectAction"
      />
    </template>
    <div>
      <div class="d-flex flex-row">
        <button
          class="btn btn-default"
          :disabled="actionTypesLimit"
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
import CallPhone from "./table/CallPhone";
import VisitWebsite from "./table/VisitWebsite";

export default {
  components: {
    CallPhone,
    VisitWebsite,
  },
  props: {
    activeDataTemplate: {
      type: Object,
      default: () => ({}),
    },
    templateData: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      typeActions: [
        { component: "call-phone", text: "Call Phone Number" },
        { component: "visit-website", text: "Visit Website" },
      ],
      typeActionSelected: [
        {
          text: "Call Phone Number",
          component: "call-phone",
        },
      ],
    };
  },
  computed: {
    actionTypesLimit() {
      return this.typeActionSelected.length >= 2;
    },
    activeTemplateButtons() {
      return this.activeDataTemplate.buttons.value || [];
    },
    listLanguages() {
      if (!this.templateData && !this.templateData.language) {
        return null;
      }
      return this.templateData.language;
    },
  },
  watch: {
    activeTemplateButtons(buttons) {
      this.typeActionSelected = buttons;
    },
    typeActionSelected() {
      this.sendActionsData();
    },
  },
  methods: {
    async addAnotherButton() {
      if (this.actionTypesLimit) {
        return false;
      }

      const data = await this.typeActions.find(
        function(act) {
          return act.text !== this.typeActionSelected[0].text;
        }.bind(this)
      );
      if (data) {
        this.typeActionSelected.push({
          component: data.component,
          text: data.text,
        });
      }
    },
    buttonText({ text, language, value }) {
      const idx = this.findIdActionSelected(text, language);
      if (idx > -1) {
        // disini typesAction menambah object key button yang seharusnya tidak ada
        // setelah ada action delete dari salah satu language yang terpilih
        this.$set(this.typeActionSelected[idx], "button", value);
      }
    },
    async deleteAction({ language, value }) {
      const idx = this.findIdActionSelected(value, language);
      if (idx > -1) {
        // disini typesAction menambah object key button yang seharusnya tidak ada
        await this.typeActionSelected.splice(idx, 1);
      }
    },
    findIdActionSelected(text, language) {
      return this.typeActionSelected.findIndex(
        function(x) {
          return (
            x.text === text && this.activeDataTemplate.language === language
          );
        }.bind(this)
      );
    },
    selectAction({ text, language, value }) {
      if (this.actionTypesLimit) {
        return false;
      }

      const idxExisting = this.findIdActionSelected(text, language);
      if (idxExisting > -1) {
        const find = this.typeActions.find((act) => act.text === value);
        if (find) {
          this.typeActionSelected.splice(idxExisting, 1, find);
          this.$emit("selectAction", { text, language, value });
        }
      }
    },
    sendActionsData() {
      this.$emit("sendActionsData", this.typeActionSelected);
    },
  },
  mounted() {
    if (this.activeTemplateButtons.length > 0) {
      this.typeActionSelected = this.activeTemplateButtons;
      this.sendActionsData();
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
