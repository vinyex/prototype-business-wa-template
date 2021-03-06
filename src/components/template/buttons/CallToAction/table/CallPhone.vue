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
          <label>Negara</label>
          <SelectDropdown
            :key="`DropdownCountry`"
            :list="listLanguages"
            :value.sync="selectCountry"
            @selected="selectedCountry"
          />
        </div>
        <div class="button-control">
          <label>Phone Number</label>
          <div class="wrapper-button-counter d-flex flex-row">
            <div class="mr-2">
              <input
                type="text"
                class="input-button-text"
                v-model="phoneCall"
                :maxlength="maxPhone"
                @input="
                  $emit('phoneNumber', {
                    text: list,
                    language: language,
                    value: $event.target.value,
                  })
                "
              />
            </div>
            <span class="counter-length">{{
              `${phoneCall.length}/${maxPhone}`
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
    listLanguages: {
      type: Array,
      default: () => [],
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
      maxPhone: 20,
      phoneCall: "",
      selectAction: "Call Phone Number",
      selectCountry: "",
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
    selectedCountry(data) {
      this.$emit("selectCountry", {
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
  background: #fff;
  border: 1px solid #c4c4c4;
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
