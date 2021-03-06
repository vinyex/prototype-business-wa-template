<template>
  <div class="d-flex flex-row">
    <div class="wrapper-preview">
      <div class="section-preview">
        <h3 class="preview-title"><span>Preview</span></h3>
        <div class="preview-content">
          <div class="pre-content">
            <div class="content">
              <div>
                <div class="message">
                  <span class="inner">
                    <span>{{ contentPreview || "Content Preview" }}</span>
                  </span>
                </div>
                <time
                  :class="{
                    upper:
                      buttonsType == 'Call To Action' && buttonsList.length > 1,
                    middle:
                      buttonsType == 'Call To Action' &&
                      buttonsList.length == 1,
                  }"
                  >01.10</time
                >
              </div>
              <component
                v-if="buttonsList.length > 0"
                :is="previewBtnComponents"
                :buttons-list="buttonsList"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PreviewCallToAction from "./buttons/CallToAction/preview/Index";
import PreviewQuickReply from "./buttons/QuickReply/Preview";

export default {
  components: { PreviewCallToAction, PreviewQuickReply },
  props: {
    buttonsList: {
      type: Array,
      default: () => [],
    },
    buttonsType: {
      type: String,
      default: "",
    },
    contentPreview: {
      type: String,
      default: "",
      required: true,
    },
  },
  computed: {
    previewBtnComponents() {
      return `preview-${this.buttonsType
        .toLowerCase()
        .split(" ")
        .join("-")}`;
    },
  },
};
</script>

<style scoped>
.wrapper-preview {
  flex: 0 1 auto;
  order: 0;
  margin-left: 8px;
  min-width: 320px;
  width: 320px;
}

.section-preview {
  background-color: #e5ddd5;
  box-sizing: border-box;
  height: 100%;
  position: relative;
  z-index: 0;
  margin: 15px 24px;
  padding: 24px;
}

.section-preview::before {
  background: url(/assets/img/background-whatsapp.png);
  background-size: 366.5px 666px;
  content: "";
  height: 100%;
  left: 0;
  opacity: 0.06;
  position: absolute;
  top: 0;
  width: 100%;
}

.section-preview .preview-title {
  font-size: 13px;
  font-weight: 700;
  line-height: 1.28;
}

.preview-content {
  padding: 0;
  margin: 12px 8px;
}

.pre-content::after {
  background: url(/assets/img/pre-message-wa.png) 50% 50% no-repeat;
  background-size: contain;
  content: "";
  height: 19px;
  left: 20px;
  position: absolute;
  top: 53px;
  width: 12px;
}

.preview-content .content {
  background-color: #fff;
  border-radius: 7.5px;
  border-top-left-radius: 0;
  box-shadow: 0 1px 0.5px rgba(0, 0, 0, 0.15);
  min-height: 20px;
  position: relative;
  word-wrap: break-word;
}

.preview-content .content time {
  bottom: 3px;
  color: rgba(0, 0, 0, 0.4);
  font-size: 11px;
  height: 15px;
  line-height: 15px;
  position: absolute;
  right: 7px;
}

.preview-content .content time.upper {
  bottom: 72px;
}

.preview-content .content time.middle {
  bottom: 36px;
}

.preview-content .content .message {
  color: #262626;
  font-size: 13.6px;
  line-height: 19px;
  padding: 7px 7px 6px 9px;
  word-wrap: break-word;
}

.preview-content .content .message .inner {
  color: #282828;
  font-size: 13.6px;
  white-space: pre-wrap;
}

.preview-content .content .message .inner::after {
  content: "";
  display: inline-block;
  margin-right: 78px;
}

.message .inner span {
  overflow-wrap: break-word;
  text-align: initial;
}
</style>
