<template>
  <div>
    <div class="template-message d-flex flex-row justify-content-between w-100">
      <div class="d-flex flex-row">
        <FormControl :key="'LanguageSection'" :title="'Languages'">
          <template #content>
            <div class="wrapper-list-languages">
              <div class="outer" v-if="listLanguages.length > 0">
                <div
                  v-for="(lang, id) in listLanguages"
                  :key="`id-list-lang-${id}`"
                  :class="{ active: lang == selectedLanguage }"
                  @click.passive="selectLanguage(lang)"
                  class="language"
                >
                  <IconCircle />
                  <span>{{ lang }}</span>
                </div>
              </div>
            </div>
          </template>
        </FormControl>
      </div>
      <div class="d-flex flex-column w-100">
        <FormControl
          :key="'FormControlBody'"
          :title="'Body'"
          :title-description="
            `Enter the text for your message in the language you've selected.`
          "
        >
          <template #content>
            <div class="form-control">
              <textarea
                class="body-message w-100"
                cols="30"
                rows="10"
                :placeholder="`Enter text in ${selectedLanguage} language`"
                @keyup="contentPreviewChange"
                v-model="contentPreview"
              ></textarea>
            </div>
          </template>
        </FormControl>
        <FormControl
          :key="'FormControlButtons'"
          :title="'Buttons'"
          :title-description="controlButtonsDesc"
        >
          <template #content>
            <div class="d-flex flex-row form-control">
              <select
                class="buttons-selectlist"
                @change="buttonsOnSelected"
                v-model="buttonsTemplate"
              >
                <option
                  v-for="(btn, i) in buttonsSelectList"
                  :key="i"
                  class="list-btn"
                  >{{ btn.title }}</option
                >
              </select>
            </div>
            <div class="main-action-wrapper">
              <component
                v-if="buttonsTemplate != '' && componentButtons"
                :is="componentButtons"
                :active-data-template="activeDataTemplate"
                :template-data="templateData"
                @sendActionsData="receiveActionsData"
                @quickReplyData="receiveQuickReply"
              />
            </div>
          </template>
        </FormControl>
      </div>
      <PreviewMessage
        :buttons-list="activeButtonsList"
        :buttons-type="activeButtonsType"
        :content-preview="
          activeDataTemplate.body ||
            `Enter text in ${selectedLanguage} language`
        "
      />
    </div>
  </div>
</template>

<script>
import CallToAction from "./../template/buttons/CallToAction/Index";
import FormControl from "./../form/FormControl";
import IconCircle from "./../icon/Circle";
import PreviewMessage from "./../template/PreviewMessage";
import QuickReply from "./../template/buttons/QuickReply/Index";

export default {
  components: {
    CallToAction,
    FormControl,
    IconCircle,
    PreviewMessage,
    QuickReply,
  },
  props: {
    templateData: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      activeDataTemplate: {},
      buttonsSelectList: [
        { title: "None", component: false },
        { title: "Call To Action", component: "CallToAction" },
        { title: "Quick Reply", component: "QuickReply" },
      ],
      buttonsTemplate: "None",
      contentPreview: "",
      dataTemplateMessages: [],
      idxLanguage: 0,
    };
  },
  computed: {
    activeButtonsType() {
      return this.activeDataTemplate.buttons
        ? this.activeDataTemplate.buttons.type
        : "";
    },
    activeButtonsList() {
      return this.activeDataTemplate.buttons
        ? this.activeDataTemplate.buttons.value
        : [];
    },
    componentButtons() {
      if (this.buttonsTemplate != "") {
        const component = this.buttonsSelectList.find(
          ({ title }) => title === this.buttonsTemplate
        );
        if (component) {
          return component.component;
        }
      }

      return false;
    },
    controlButtonsDesc() {
      switch (this.buttonsTemplate) {
        case "Call To Action":
          return "Create up to 2 buttons that let customers respond to your message or take action.";

        case "Quick Reply":
          return "Create up to 3 buttons that let customers respond to your message or take action.";

        default:
          return "Create buttons that let customers respond to your message or take action.";
      }
    },
    listLanguages() {
      return this.templateData.language;
    },
    selectedLanguage() {
      return this.templateData.language[this.idxLanguage];
    },
  },
  watch: {
    listLanguages() {
      this.getDataTemplateMessages();
    },
  },
  methods: {
    buttonsOnSelected(e) {
      this.$set(this.activeDataTemplate.buttons, "type", e.target.value);
      this.$set(this.activeDataTemplate.buttons, "value", []);
    },
    contentPreviewChange(e) {
      this.$set(this.activeDataTemplate, "body", e.target.value);
    },
    async getDataTemplateMessages() {
      await this.listLanguages.forEach(
        function(lang) {
          this.dataTemplateMessages.push({
            nama: this.templateData.nama,
            category: this.templateData.category,
            language: lang,
            body: "",
            buttons: {
              type: "None",
              value: [],
            },
          });
        }.bind(this)
      );
      this.updateActiveTemplate(this.idxLanguage);
    },
    receiveActionsData(data) {
      this.$set(this.activeDataTemplate.buttons, "value", data);
    },
    receiveQuickReply(data) {
      this.$set(this.activeDataTemplate.buttons, "value", data);
    },
    async selectLanguage(lang) {
      const index = this.listLanguages.findIndex((x) => x === lang);
      if (index > -1) {
        this.idxLanguage = index;
        await this.updateActiveTemplate(index);
        this.contentPreview = this.activeDataTemplate.body;
        this.buttonsTemplate = this.activeDataTemplate.buttons.type;
      }
    },
    updateActiveTemplate(index) {
      this.activeDataTemplate = this.dataTemplateMessages[index];
    },
    updateDataTemplateMessage() {
      const find = this.dataTemplateMessages.find(
        function(template) {
          return template.language === this.activeDataTemplate.language;
        }.bind(this)
      );
      if (find) {
        this.dataTemplateMessages.splice(this.idxLanguage, 1, find);
      }
    },
  },
  mounted() {
    this.getDataTemplateMessages();
  },
};
</script>

<style scoped>
.template-message {
  width: 100%;
  overflow-x: scroll;
  height: calc(100vh - 53px);
}

.form-control {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin: 5px;
}

.body-message {
  padding: 12px 15px;
  font-size: 14px;
  border: 1px solid gray;
  border-radius: 8px;
  outline: none;
}

::v-deep.wrapper-label {
  width: 750px;
  max-width: 750px;
}

.wrapper-list-languages {
  display: flex;
  flex-direction: row;
  align-items: stretch;
  justify-content: flex-start;
}

.wrapper-list-languages .outer {
  display: flex;
  flex-direction: column;
  padding-top: 8px;
  padding-bottom: 8px;
  width: 300px;
  height: 100%;
  background-color: #fff;
  overflow-y: hidden;
  overflow-x: hidden;
}

.wrapper-list-languages .language {
  display: flex;
  flex-direction: row;
  align-items: center;
  cursor: pointer;
  background: #fff;
  border-radius: 4px;
  padding: 7px;
  margin: 2px 0;
  transition: background 0.2s ease-in-out;
}

.wrapper-list-languages .language:hover {
  background: #d5d5d5b0;
}

.wrapper-list-languages .language.active {
  color: rgba(24, 119, 242, 1);
  font-weight: 700;
  background: rgba(23, 119, 244, 0.1);
}

.wrapper-list-languages .language.active:hover {
  background: rgba(23, 119, 244, 0.2);
}

.main-action-wrapper {
  padding: 0;
  margin: 7px 5px;
}

.buttons-selectlist {
  font-size: 0.85em;
}
</style>
