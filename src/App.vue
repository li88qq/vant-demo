<template>
  <div>
    <van-form>
      <van-cell-group inset>
        <van-field v-model="formRt.text" name="text" label="单行文本"/>
        <van-field v-model="formRt.textarea" type="textarea" name="textarea" label="多行文本" :rows="1" autosize/>
        <van-field v-model="formRt.static" name="static" readonly class="text-static"/>
        <van-field name="radio" label="单选" is-link readonly @click="showPicker('radio')"></van-field>
      </van-cell-group>
      <div style="margin: 16px;">
        <van-button round block type="primary">
          提交
        </van-button>
      </div>

    </van-form>

    <van-popup v-model:show="popupRt.show" round position="bottom">
      <van-picker
          :columns="popupRt.columns"
          @cancel="popupRt.onCancel"
          @confirm="popupRt.onConfirm"
      />
    </van-popup>
  </div>
</template>

<script setup>
import {ref, reactive, watch} from 'vue'

const formRt = reactive({
  text: '',
  textarea: '',
  static: '说明,需要描述内容',
  radio: false,
})

const picker_radio = {
  columns: [
    {text: '保密', value: 0},
    {text: '男', value: 1},
    {text: '女', value: 2},
  ]
}

const pickerMap = {
  radio: picker_radio,
}

const showPicker = (field) => {
  popupRt.field = field;
  popupRt.show = true;
}

const popupRt = reactive({
  field: '',
  show: false,
  columns: [],
  onCancel: () => {
    popupRt.show = false;
  },
  onConfirm: () => {
    popupRt.show = false;
  },
})

watch(() => popupRt.field, (value) => {
  const picker = pickerMap[value];
  if (picker) {
    popupRt.columns = picker.columns;
    if (picker.onCancel) {
      popupRt.onCancel = picker.onCancel;
    }
    if (picker.onConfirm) {
      popupRt.onConfirm = picker.onConfirm;
    }
  }
})


</script>
<style scoped>
.text-static {
  background-color: #eee;
}
</style>