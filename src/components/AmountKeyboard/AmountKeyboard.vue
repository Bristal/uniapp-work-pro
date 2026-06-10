<template>
  <view class="amount-keyboard">
    <u-input v-model="innerValue" :placeholder="placeholder" :disabled="disabled" :border="border" @focus="onInputFocus"
      @click="onInputClick" />
    <u-keyboard mode="number" :show="showKeyboard" :showTips="false" :dotDisabled="dotDisabled" @cancel="onCancel"
      @change="onChange" @confirm="onConfirm" @backspace="onDelete"></u-keyboard>
  </view>
</template>

<script setup>
import { ref, computed, watch } from "vue";

// 定义props
const props = defineProps({
  // 控制键盘显示隐藏（可选，也可以由组件内部控制）
  show: {
    type: Boolean,
    default: null,
  },
  // 是否禁用小数点
  dotDisabled: {
    type: Boolean,
    default: false,
  },
  // v-model绑定值
  modelValue: {
    type: [String, Number],
    default: "",
  },
  // 输入框占位符
  placeholder: {
    type: String,
    default: "请输入金额",
  },
  // 是否禁用输入框
  disabled: {
    type: Boolean,
    default: false,
  },
  // 是否显示输入框边框
  border: {
    type: Boolean,
    default: true,
  },
  // 限制输入小数点后几位
  decimalLimit: {
    type: Number,
    default: 2,
  },
  // 限制小数点前几位
  integerLimit: {
    type: Number,
    default: 5,
  },
});

// 定义emit
const emit = defineEmits([
  "update:modelValue",
  "confirm",
  "cancel",
  "update:show",
]);

// 内部维护的值
const innerValue = ref("");
const showKeyboard = ref(false);

// 监听外部值变化，同步到内部
watch(
  () => props.modelValue,
  (newVal) => {
    console.log("外部输入值变化", newVal);
    innerValue.value = newVal;
  },
  { immediate: true }
);

// 监听外部传入的show状态，同步到内部
watch(
  () => props.show,
  (newVal) => {
    if (newVal !== null) {
      showKeyboard.value = newVal;
    }
  },
  { immediate: true }
);

// 监听内部键盘状态变化，向外发射事件
watch(showKeyboard, (newVal) => {
  if (props.show !== null) {
    emit("update:show", newVal);
  }
});

// 按键变化事件
const onChange = (value) => {
  // 获取小数点前和小数点后的部分
  const [integerPart, decimalPart] = innerValue.value.split(".");
  console.log("integerPart", integerPart, "decimalPart", decimalPart);
  // 限制小数点前的位数
  if (
    integerPart.length === props.integerLimit &&
    !decimalPart &&
    value !== "." &&
    !innerValue.value.includes(".")
  ) {
    return;
  }
  // 有小数点
  if (decimalPart) {
    if (decimalPart.length >= props.decimalLimit) {
      // 已经达到小数位数限制，不再允许输入
      return;
    }
    // 不允许输入多个小数点
    if (value === ".") {
      return;
    }
  }
  // 如果第一位是0，后面不能再输入0
  if (innerValue.value === "0" && value !== ".") {
    return;
  }
  // 如果第一位是小数点，前面补0
  if (value === "." && innerValue.value === "") {
    innerValue.value = "0.";
  } else {
    innerValue.value += value;
  }

  // 更新v-model
  emit("update:modelValue", innerValue.value);
};

// 删除按键事件，每次删除最后一位
const onDelete = () => {
  if (innerValue.value.length > 0) {
    innerValue.value = innerValue.value.slice(0, -1);
    // 更新v-model
    emit("update:modelValue", innerValue.value);
  }
};

// 输入框获取焦点
const onInputFocus = () => {
  if (!props.disabled) {
    showKeyboard.value = true;
  }
};

// 输入框点击
const onInputClick = () => {
  if (!props.disabled) {
    showKeyboard.value = true;
  }
};

// 取消事件
const onCancel = () => {
  showKeyboard.value = false;
  emit("cancel");
};

// 确认事件
const onConfirm = () => {
  showKeyboard.value = false;
  emit("confirm", innerValue.value);
};
</script>

<style scoped>
.amount-keyboard {
  width: 100%;
}
</style>