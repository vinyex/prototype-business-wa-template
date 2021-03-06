<template>
  <div>
    <div class="aselect">
      <div class="selector" @click="toggle">
        <div class="label">
          <span>{{ value }}</span>
        </div>
        <div class="arrow"></div>
        <div :class="{ hidden: !visible, visible }">
          <ul>
            <li
              v-for="(item, idx) in list"
              :key="idx"
              :class="{ active: (item.text || item) === value }"
              @click="select(item)"
            >
              {{ item.text || item }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: () => "Select dropdown",
    },
    list: {
      type: Array,
      default: () => ["Default", "Array", "Change it", "By props"],
    },
  },
  data() {
    return {
      visible: false,
    };
  },
  methods: {
    select(item) {
      this.$emit("update:value", item.text || item);
      this.$emit("selected", item.text || item);
    },
    toggle() {
      this.visible = !this.visible;
    },
  },
};
</script>

<style scoped>
.aselect {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 100%;
  margin: 0;
  padding: 0;
  font-size: 1em;
}

.aselect .selector {
  position: relative;
  width: 100%;
  background: white;
  border: 1px solid #c4c4c4;
  border-radius: 5px;
  z-index: 1;
}

.aselect .selector .arrow {
  position: absolute;
  right: 10px;
  top: 40%;
  width: 0;
  height: 0;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: 7px solid rgb(85, 85, 85);
  transform: rotateZ(0deg) translateY(0px);
  transition-duration: 0.3s;
  transition-timing-function: cubic-bezier(0.59, 1.39, 0.37, 1.01);
}

.aselect .selector .expanded {
  transform: rotateZ(180deg) translateY(2px);
}

.aselect .selector .label {
  display: flex;
  min-height: 13px;
  padding: 5px;
  cursor: pointer;
}

.aselect ul {
  width: 100%;
  list-style-type: none;
  padding: 0;
  margin: 0;
  border: 1px solid gainsboro;
  position: absolute;
  z-index: 1;
  background: #fff;
}

.aselect li {
  padding: 12px;
  color: #666;
  background: #fff;
  transition: color 0.2s ease-in-out, background 0.2s ease-in-out;
}

.aselect li.active {
  color: #fff;
  background: rgba(24, 119, 242, 1);
}

.aselect li:hover {
  color: #000;
  cursor: pointer;
  background: #d5d5d5b0;
}

.aselect .current {
  background: #eaeaea;
}

.aselect .hidden {
  visibility: hidden;
}

.aselect .visible {
  visibility: visible;
}
</style>
