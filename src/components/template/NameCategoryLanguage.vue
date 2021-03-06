<template>
  <div class="d-flex flex-column">
    <FormControl
      :key="`FormControlCategory`"
      :title="'Category'"
      :title-description="
        'Choose what type of message template you want to create.'
      "
    >
      <template #content>
        <div class="list-category" id="category">
          <div
            v-for="(list, key) in categoryList"
            :key="`category-${key}`"
            :class="{ active: list === categoryValue }"
            @click="categoryOnClick(list)"
          >
            <span class="w-100" style="margin: 0 5px;">{{ list }}</span>
          </div>
        </div>
      </template>
    </FormControl>
    <FormControl
      :key="`FormControlNama`"
      :title="'Nama'"
      :title-description="'Give your message template a name.'"
    >
      <template #content>
        <div class="form-control">
          <input
            type="text"
            class="w-100"
            name="nama"
            id="nama"
            placeholder="Enter message template name..."
            style="padding-right: 65px;"
            :maxlength="maxNama"
            :value="namaValueFormatted"
            @input="namaOnInput"
            required
          />
          <span class="counter-length">{{
            `${namaValue.length}/${maxNama}`
          }}</span>
        </div>
      </template>
    </FormControl>
    <FormControl
      :key="`FormControlLanguage`"
      :title="'Language'"
      :title-description="
        `Choose which languages your message template will be sent in. You can
          delete or add more languages later.`
      "
    >
      <template #content>
        <div class="form-control flex-column align-start">
          <div
            v-if="languageValue.length > 0"
            class="wrapper-badge d-flex flex-row"
          >
            <div
              v-for="(list, i) in languageValue"
              :key="`language-selected-${i}`"
              class="badge-selected"
            >
              <span
                >{{ list }}
                <span
                  class="delete"
                  title="Remove language"
                  @click.passive="removeLanguage(list)"
                  >&#215;</span
                ></span
              >
            </div>
          </div>
          <div class="d-flex flex-column w-100">
            <input
              type="text"
              name="languages"
              id="languages"
              placeholder="Select Languages"
              v-model="languageInput"
              @focus="languageOnFocus = true"
            />
            <div
              v-show="languageOnFocus || languageInput.length > 0"
              class="list-language"
            >
              <div
                v-for="(list, i) in filterLanguages"
                :key="`languages-id${i}`"
                @click.passive="clickLanguages(list)"
              >
                <span>{{ list }}</span>
              </div>
            </div>
          </div>
        </div>
      </template>
    </FormControl>
  </div>
</template>

<script>
import FormControl from "./../form/FormControl";
export default {
  components: { FormControl },
  props: {
    categoryList: {
      type: Array,
      default: () => [
        "Update akun",
        "Update peringatan",
        "Update janji temu",
        "Auto reply",
        "Pemecahan masalah",
        "Pembayaran diupdate",
        "Update Keuangan Pribadi",
        "Update Pemesanan",
        "Update Pengiriman",
        "Update Tiket",
        "Update Transportasi",
      ],
    },
  },
  data() {
    return {
      categoryValue: "",
      languageInput: "",
      languageList: ["Indonesia", "English US", "English UK"],
      languageOnFocus: false,
      languageValue: [],
      maxNama: 512,
      namaValue: "",
    };
  },
  computed: {
    filterLanguages() {
      return this.languageList.filter(
        function(list) {
          return (
            list.toLowerCase().indexOf(this.languageInput) > -1 &&
            !this.languageValue.some((x) => x === list)
          );
        }.bind(this)
      );
    },
    namaValueFormatted() {
      return this.namaValue.replace(" ", "_");
    },
    valuesValidate() {
      return (
        this.categoryValue !== "" &&
        this.languageValue.length > 0 &&
        this.namaValue !== "" &&
        this.namaValue.length <= 512
      );
    },
  },
  watch: {
    valuesValidate(newVal) {
      this.$emit("valuesValidate", newVal);
    },
  },
  methods: {
    categoryOnClick(list) {
      if (this.categoryValue === list) {
        this.categoryValue = "";
      } else {
        const data = this.categoryList.find((cat) => cat === list);
        if (data) {
          this.categoryValue = list;
          this.$emit("category", list);
        }
      }
    },
    clickLanguages(list) {
      const indexLanguage = this.languageValue.findIndex((x) => x === list);
      if (indexLanguage > -1) {
        this.languageValue.splice(indexLanguage, 1);
        this.languageInput = "";
      } else {
        const data = this.languageList.find((x) => x === list);
        if (data) {
          this.languageValue.push(list);
          this.languageInput = "";
          this.$emit("language", this.languageValue);
        }
      }
      this.languageOnFocus = false;
    },
    namaOnInput(e) {
      this.namaValue = e.target.value;
      this.$emit("nama", e.target.value);
    },
    removeLanguage(list) {
      const data = this.languageValue.findIndex((x) => x === list);
      if (data > -1) {
        this.languageValue.splice(data, 1);
      }
    },
  },
};
</script>

<style scoped>
.form-control {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin: 5px;
}

.form-control.flex-column {
  flex-direction: column;
}

.form-control.align-start {
  align-items: flex-start;
}

.form-control .counter-length {
  position: absolute;
  right: 65px;
  font-size: 12px;
  color: gray;
}

.list-category {
  width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  flex-wrap: wrap;
}

.list-category > div {
  display: flex;
  align-items: center;
  text-align: center;
  flex: 1 0 20%; /* explanation below */
  font-size: 13px;
  cursor: pointer;
  margin: 5px;
  height: 50px;
  background: #ffffff;
  border: 1px solid #f0f0f0f0;
  border-radius: 4px;
  transition: border-color 0.2s ease-in-out, color 0.15s step-end;
}

.list-category > div:hover {
  border-color: #000000;
}

.list-category > div.active {
  border-color: rgba(24, 119, 242, 1);
  color: rgba(24, 119, 242, 1);
}

.list-language {
  width: 100%;
  margin: 15px 0;
  border: 1px solid #f0f0f0f0;
  border-radius: 4px;
}

.list-language div {
  display: flex;
  padding: 7px 15px;
  background: #fffdfdf0;
  border: 1px solid #f0f0f0f0;
  transition: border-color 0.2s ease-in-out, color 0.15s step-end;
}

.list-language div:hover {
  cursor: pointer;
  border-color: #000;
}

.wrapper-badge {
  padding: 0;
  margin: 0;
  font-size: 13px;
  color: gray;
}

.wrapper-badge .badge-selected {
  background-color: #f0f0f0f0;
  padding: 3px 7px;
  margin: 0 5px;
  margin-bottom: 10px;
  border-radius: 3px;
  color: #000;
  cursor: pointer;
}

.wrapper-badge .badge-selected .delete {
  color: gray;
}
</style>
