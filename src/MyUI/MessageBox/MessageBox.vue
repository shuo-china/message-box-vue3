<template>
  <transition name="my-ui-messagebox-fade">
    <div class="ui-message-box" v-show="visible" @click="cancelBtnClick">
      <div class="ui-message-box-wrapper" @click.stop>
        <div class="message-box-title">
          <h1>{{ title }}</h1>
          <span class="close-btn" @click="cancelBtnClick">X</span>
        </div>
        <div class="message-box-content">
          <content-view :field="field" />
        </div>
        <div class="message-box-btn-group">
          <button class="btn btn-primary" @click="confirmBtnClick">
            {{ confirmBtnText }}
          </button>
          <button
            class="btn btn-default"
            @click="cancelBtnClick"
            v-if="showCancelBtn"
          >
            {{ cancelBtnText }}
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script setup>
import { reactive, toRefs, defineProps, defineExpose, h } from "vue";
import { fields } from "./index";

const props = defineProps({
  title: {
    type: String,
    default: "Message",
  },
  content: {
    type: String,
    default: "Message Content",
  },
  showCancelBtn: {
    type: Boolean,
    default: false,
  },
  confirmBtnText: {
    type: String,
    default: "OK",
  },
  cancelBtnText: {
    type: String,
    default: "Cancel",
  },
  field: {
    type: String,
    default: "confirm",
    validator: (value) => fields.includes(value),
  },
});

const state = reactive({
  visible: false,
  promptValue: "",
  type: "confirm",
});

const { visible } = toRefs(state);

const setVisible = (isVisible) => {
  state.visible = isVisible;
};

const confirmBtnClick = () => {
  state.type = "confirm";
  setVisible(false);
};

const cancelBtnClick = () => {
  state.type = "cancel";
  setVisible(false);
};

const ContentView = ({ field = "confirm" }) => {
  switch (field) {
    case "confirm":
      return <p>{props.content}</p>;
    case "prompt":
      return (
        <input
          value={state.promptValue}
          onInput={(e) => (state.promptValue = e.target.value)}
          class="message-input"
        />
      );
    default:
      return null;
  }
};

defineExpose({
  setVisible,
  state,
});
</script>

<style scoped>
.my-ui-messagebox-fade-enter-from,
.my-ui-messagebox-fade-leave-to {
  opacity: 0;
}

.my-ui-messagebox-fade-enter-active,
.my-ui-messagebox-fade-leave-active {
  transition: opacity 0.2s ease;
}

.ui-message-box {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.ui-message-box-wrapper {
  position: absolute;
  top: calc(50% - 100px);
  left: 50%;
  width: 420px;
  height: 180px;
  transform: translate(-50%, -50%);
  background-color: #fff;
  border-radius: 3px;
}

.ui-message-box-wrapper h1,
p {
  margin: 0;
  font-weight: normal;
}

.message-box-title {
  padding: 15px 15px 10px;
}

.message-box-title h1 {
  font-size: 18px;
  display: inline-block;
}

.close-btn {
  float: right;
  font-size: 14px;
  color: #999;
  cursor: pointer;
}

.message-box-content {
  padding: 10px 15px;
}

.message-box-content p {
  font-size: 14px;
}

.message-box-btn-group {
  position: absolute;
  right: 0;
  left: 0;
  bottom: 0;
  padding: 5px 15px 15px;
}

.message-box-btn-group button {
  float: right;
}

.btn {
  border: none;
  outline: none;
  height: 32px;
  padding: 8px 15px;
  cursor: pointer;
  border-radius: 3px;
  margin-left: 12px;
}

.btn.btn-primary {
  background-color: #409eff;
  color: #fff;
}

.btn.btn-default {
  background-color: #fff;
  border: 1px solid #ddd;
}

.message-input {
  border: none;
  outline: none;
  width: 100%;
  border: 1px solid #dedede;
  height: 32px;
  box-sizing: border-box;
}
</style>
