<template>
  <a-modal v-bind="$attrs" v-model:open="_open" @ok="handleOK">
    <slot></slot>
  </a-modal>
</template>

<script setup lang="ts">
import { computed } from 'vue';
interface Props {
  open: boolean;
  formRef?: any; //校验表单Ref
}

const props = withDefaults(defineProps<Props>(), {
  open: false,
  okText: '确定',
  cancelText: '取消',
});
const emits = defineEmits(['update:open', 'callBackOk', 'cancel']);

const _open = computed({
  get: () => {
    return props.open;
  },
  set: val => {
    if (!val) {
      close(val);
    }
  },
});

function close(val: any) {
  if (props.formRef) {
    props.formRef.resetFields();
  }
  emits('update:open', val);
  emits('cancel');
}

async function handleOK() {
  if (props.formRef) {
    await props.formRef.validate();
    emits('callBackOk');
    props.formRef.clearValidate();
    close(false);
    return;
  }
  emits('callBackOk');
  close(false);
}
</script>

<style lang="scss" scoped></style>
