<template>
  <div style="width: 100%">
    <div class="InputFormFieldWapper">
      <el-input
        style="margin-left: 10px"
        @keydown="handleKeyDown"
        v-model="inputTextInner"
        autosize
        @input="updateInputText"
        ref="inputRefInner"
        type="textarea"
        :placeholder="
          aiLoading ? '思考中..' : '输入你想问的...'
        "
        :disabled="aiLoading"
      >
      </el-input>
      <div class="animation-dot" v-if="aiLoading">
        <div class="dot_0"></div>
        <div class="dot_1"></div>
        <div class="dot_2"></div>
        <div class="dot_3"></div>
      </div>
      <div @click="onSubmit" class="sendIcon" v-else>
        <el-icon :size="20">
          <Promotion />
        </el-icon>
      </div>
    </div>
  </div>
</template>

<script>
import { Promotion } from "@element-plus/icons-vue";
import { ref, defineComponent, watch } from "vue";

export default defineComponent({
  name: "InputFormField",
  components: {
    Promotion,
  },
  props: {
    // 选择的模型
    inputText: {
      type: String,
      default: null,
    },
    // 加载状态
    aiLoading: {
      type: Boolean,
      default: false,
    },
  },
  setup(props, { emit }) {
    // 通过ref监听组件值
    let inputTextInner = ref(null);
    const inputRefInner = ref(null);

    // 使用watch监听content变量的变化
    // eslint-disable-next-line no-unused-vars
    watch(inputTextInner, (newVal, oldValue) => {
      // console.log("watch输入内容改变了", newVal);
      emit("update:inputText", newVal);
    });

    // 重置值
    function resetInputValue() {
      if (inputRefInner.value) {
        inputRefInner.value.clear();
      }
    }

    //提交内容的快捷键监听
    function handleKeyDown(e) {
      // 判断是否按下了 alt 键和 enter 键
      if (e.ctrlKey && e.keyCode === 13) {
        // 执行你的操作
        // console.log("Alt + Enter 被按下");

        emit("onSubmit");
      }
    }

    //提交内容的快捷键监听
    function onSubmit() {
      // console.log("点击了提交onSubmit()");
      emit("onSubmit");
    }
    // 更新输入文本，
    function updateInputText(value) {
      // console.log("更新值", value);
      inputTextInner.value = value;
    }

    return {
      updateInputText,
      onSubmit,
      handleKeyDown,
      resetInputValue,
      inputTextInner,
      inputRefInner,
    };
  },
});
</script>

<style lang="scss" scoped>
.InputFormFieldWapper {
  display: flex;
  width: 100%;
  align-items: flex-start;

  .sendIcon {
    flex-shrink: 0;
    width: 36px;
    height: 36px;
    color: white;
    cursor: pointer;
    background: #7365FF;
    border-radius: 50%;
    justify-content: center;
    align-items: center;
    display: flex;
    margin: 22px 20px 0 0;
  }
}

:deep(.selectWrapper) {
  width: 150px;
  .el-input {
    .el-input__wrapper {
      padding-top: 25px;
      padding-left: 25px;
      box-shadow: none !important;
      background: none !important;
      .el-input__inner {
        text-indent: 10px;
      }

      &:hover {
        box-shadow: none;
        background: none !important;
      }
    }
  }

  &.el-select--disabled {
    background: white;
    .el-input__wrapper {
      background: white;
    }
  }
}



:deep(.InputFormFieldWapper) {
  .el-textarea__inner {
    background: white;
    box-shadow: none;
    max-height: 400px;
    padding: 20px;
    margin: 10px 0px;
    width: 100%;
    resize: none;

    color: black;
    &:hover {
      box-shadow: none;
      background: white;
    }

    &.el-select--disabled {
      background: white;
    }
  }
}

.animation-dot {
  display: flex;
  padding-right: 5px;
  margin: 35px 12px 0 0;
}

:deep(.el-textarea.is-disabled .el-textarea__inner){
  background-color: #ffffff;
}


</style>
