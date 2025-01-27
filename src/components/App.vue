<template>
  <div id="app">
    <Header
      :main-btn-text="headerMainBtnText"
      :title="stepTemplateMessage ? nama : 'New Message Template'"
      :validate-next-btn="headerBtnValidate"
      @secondaryAction="secondaryAction"
      @mainAction="mainAction"
    />
    <div>
      <keep-alive>
        <component
          :is="contentComponent"
          :template-data="templateMessageProps"
          @category="category = $event"
          @language="language = $event"
          @nama="nama = $event"
          @valuesValidate="valuesValidate"
        />
      </keep-alive>
    </div>
  </div>
</template>

<script>
import Header from "./template/Header";
import NameCategoryLanguage from "./template/NameCategoryLanguage";
import TemplateMessage from "./template/TemplateMessage";

export default {
  components: {
    Header,
    NameCategoryLanguage,
    TemplateMessage,
  },
  data() {
    return {
      accessToken: "7b4a219b9884e4bd9239698b8103271ad49ad91a",
      category: "",
      contentComponent: "name-category-language",
      contentListComponent: ["name-category-language", "template-message"],
      headerNextBtnValidate: false,
      language: [],
      nama: "",
      stepTemplateComplete: false,
    };
  },
  computed: {
    headerBtnValidate() {
      if (this.stepCategoryLanguage) {
        return this.headerNextBtnValidate;
      } else if (this.stepTemplateMessage) {
        return this.stepTemplateComplete;
      }
    },
    headerMainBtnText() {
      switch (this.contentComponent) {
        case "name-category-language":
          return "Lanjutkan";

        case "template-message":
          return "Kirim";

        default:
          return "";
      }
    },
    stepCategoryLanguage() {
      return this.contentComponent === "name-category-language";
    },
    stepTemplateMessage() {
      return this.contentComponent === "template-message";
    },
    templateMessageProps() {
      if (!this.headerNextBtnValidate) {
        return false;
      }

      return {
        category: this.category,
        language: this.language,
        nama: this.nama,
      };
    },
  },
  methods: {
    mainAction() {
      if (this.stepCategoryLanguage) {
        this.contentComponent = "template-message";
      } else if (this.stepTemplateMessage) {
        alert("Terkirim");
      }
    },
    secondaryAction() {
      if (this.stepTemplateMessage) {
        this.contentComponent = "name-category-language";
      }
    },
    valuesValidate(val) {
      this.headerNextBtnValidate = val;
    },
  },
};
</script>

<style>
@import "../css/app.css";

body {
  background: #f0f0f0f0;
}

.w-100 {
  width: 100%;
}

.d-flex {
  display: flex;
}

.flex-row {
  flex-direction: row;
}

.flex-column {
  flex-direction: column;
}

.align-items-center {
  align-items: center;
}

.align-items-stretch {
  align-items: stretch;
}

.justify-content-center {
  justify-content: center;
}

.justify-content-between {
  justify-content: space-between;
}

.btn {
  padding: 0 12px;
  margin: 0 12px;
}

.btn-default {
  margin: 5px 0;
  padding: 5px 7px;
  border: none;
  outline: none;
  border-radius: 4px;
  color: #000000d9;
  background-color: #0000000d;
  letter-spacing: -0.3px;
  transition: background-color 0.2s ease-in-out;
}

.btn-default:hover {
  background-color: #d5d5d5b0;
}

button:disabled {
  cursor: 'not-allowed';
  opacity:  0.4;
}

.my-1 {
  margin: 0.25rem 0;
}

.my-2 {
  margin: 0.5rem 0;
}

.mr-2 {
  margin-right: 0.5rem;
}

.close-icon:after{
  display: inline-block;
  content: "\00d7"; /* This will render the 'X' */
}
</style>
