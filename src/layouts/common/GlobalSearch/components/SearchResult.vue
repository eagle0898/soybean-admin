<template>
  <n-scrollbar>
    <div class="pb-12px">
      <template v-for="item in options" :key="item.path">
        <div
          class="bg-[#e5e7eb] dark:bg-dark h-56px mt-8px px-14px rounded-4px cursor-pointer flex-y-center justify-between"
          :style="{
            background: item.path === active ? theme.themeColor : '',
            color: item.path === active ? '#fff' : ''
          }"
          @click="handleTo"
          @mouseenter="handleMouse(item)"
        >
          <Icon :icon="item.meta?.icon ?? 'mdi:bookmark-minus-outline'" />
          <span class="flex-1 ml-5px">{{ item.meta?.title }}</span>
          <icon-ant-design:enter-outlined class="icon text-20px p-2px mr-3px" />
        </div>
      </template>
    </div>
  </n-scrollbar>
</template>

<script lang="ts" setup>
import { computed } from 'vue';
import type { RouteRecordRaw } from 'vue-router';
import { NScrollbar } from 'naive-ui';
import { Icon } from '@iconify/vue';
import { useThemeStore } from '@/store';

interface Props {
  value: string;
  options: RouteRecordRaw[];
}

interface Emits {
  (e: 'update:value', val: string): void;
  (e: 'enter'): void;
}

const props = withDefaults(defineProps<Props>(), {});
const emit = defineEmits<Emits>();

const active = computed({
  get() {
    return props.value;
  },
  set(val: string) {
    emit('update:value', val);
  }
});
const theme = useThemeStore();

/** 鼠标移入 */
async function handleMouse(item: RouteRecordRaw) {
  active.value = item.path;
}

function handleTo() {
  emit('enter');
}
</script>
<style lang="scss" scoped></style>
