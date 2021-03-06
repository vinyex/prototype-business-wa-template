<template>
  <div>
    <div class="card-action w-100">
      <div class="wrapper">
        <div class="button-control">
          <label>Type of Action</label>
          <SelectDropdown
            :key="`DropdownAction`"
            :list="typeActions"
            :value="list"
            @selected="selectActionChange"
          />
        </div>
        <div class="button-control">
          <label>Button Text</label>
          <div class="wrapper-button-counter d-flex flex-row">
            <div class="mr-2">
              <input
                type="text"
                class="input-button-text"
                :maxlength="maxButton"
                :value="buttonText"
                @input="
                  $emit('buttonText', {
                    text: list,
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
        </div>
        <div class="button-control">
          <label>URL Type</label>
          <SelectDropdown
            :key="`DropdownURLs`"
            :list="listUrls"
            :value="selectUrl"
            @selected="urlTypeChange"
          />
        </div>
        <div class="button-control">
          <label>Website URL</label>
          <div class="wrapper-button-counter d-flex flex-row">
            <div class="mr-2">
              <input
                type="text"
                class="input-button-text"
                v-model="websiteText"
                :maxlength="maxUrl"
                @input="
                  $emit('websiteText', {
                    text: list,
                    language: language,
                    value: $event.target.value,
                  })
                "
              />
            </div>
            <span class="counter-length">{{
              `${websiteText.length}/${maxUrl}`
            }}</span>
          </div>
        </div>
      </div>
    </div>
    <div
      v-if="deleteAction"
      class="close-icon btn-default"
      @click.passive="
        $emit('deleteAction', { language: language, value: list })
      "
    ></div>
  </div>
</template>

<script>
import SelectDropdown from "./../../../../utilities/Dropdown";

export default {
  components: { SelectDropdown },
  props: {
    buttonText: {
      type: String,
      default: "",
    },
    deleteAction: {
      type: Boolean,
      default: false,
    },
    language: {
      type: String,
      default: "",
    },
    list: {
      type: [String],
      default: "",
    },
    listUrls: {
      type: Array,
      default: () => ["Static", "Dynamic"],
    },
    typeActions: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      lists: [],
      maxButton: 20,
      maxUrl: 20,
      selectAction: "Visit Website",
      selectUrl: "Static",
      websiteText: "",
    };
  },
  methods: {
    selectActionChange(data) {
      this.$emit("selectAction", {
        text: this.list,
        language: this.language,
        value: data,
      });
    },
    urlTypeChange(data) {
      this.selectUrl = data;
      this.$emit("urlType", {
        text: this.list,
        language: this.language,
        value: data,
      });
    },
  },
};
</script>

<style scoped>
.card-action {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  justify-content: flex-start;
  background-color: #f8fafb;
  border: 1px solid #dadde1;
  border-radius: 10px;
  padding: 16px 7px;
  font-size: 0.7em;
}

.button-control {
  display: flex;
  margin: 0 5px;
  flex-direction: column;
  width: 100%;
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.4;
}

.button-control label {
  font-weight: 700;
  padding: 0;
  margin: 5px 0;
}

.button-control .counter-length {
  font-size: 12px;
  color: gray;
}

.button-control .wrapper-button-counter {
  border: 1px solid #c4c4c4;
  background: #fff;
  padding: 4px 5px;
  border-radius: 5px;
  align-items: center;
  justify-content: space-around;
}

.button-control .input-button-text {
  border: unset;
  outline: none;
  width: 100%;
}

.wrapper {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
  width: 100%;
}

.close-icon {
  display: flex;
  margin-left: 7px;
  cursor: pointer;
}
</style>
