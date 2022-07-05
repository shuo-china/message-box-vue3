<template>
  <div>
    <button @click="showMessageBox">MessageBox</button>
    <button @click="showConfirmMessageBox">ConfirmMessageBox</button>
    <button @click="showPromptMessageBox">PromptMessageBox</button>
  </div>
</template>

<script setup>
import { MessageBox } from "./MyUI";

const showMessageBox = () => {
  MessageBox({
    confirmBtnText: "确定",
    title: "MessageBox",
    content: "This is content",
  })
    .then(() => {
      console.log("MessageBox resolve");
    })
    .catch(() => {
      console.log("MessageBox reject");
    });
};

const showConfirmMessageBox = () => {
  MessageBox.confirm({
    showCancelBtn: true,
    cancelBtnText: "取消",
    title: "MessageBox.confirm",
    content: "This is content",
  })
    .then(() => {
      console.log("ConfirmMessageBox resolve");
    })
    .catch(() => {
      console.log("ConfirmMessageBox reject");
    });
};

const showPromptMessageBox = () => {
  MessageBox.prompt({
    confirmBtnText: "确定",
    showCancelBtn: true,
    cancelBtnText: "取消",
    title: "PromptMessageBox.prompt",
    content: "PromptMessageBox is content",
  })
    .then((v) => {
      console.log("ConfirmMessageBox resolve", v);
    })
    .catch(() => {
      console.log("ConfirmMessageBox reject");
    });
};
</script>

<style></style>
