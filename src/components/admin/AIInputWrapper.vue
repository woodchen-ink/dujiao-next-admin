<script setup lang="ts">
// 输入框右侧内嵌 AI 生成按钮的包装组件
// 通过 slot 包裹任意 Input/Textarea，并在其右内侧叠加 AI 图标
// multiline=true 时按钮定位到右上角（用于 Textarea）

const props = defineProps<{
  loading?: boolean
  multiline?: boolean
}>()

const emit = defineEmits<{
  generate: []
}>()
</script>

<template>
  <div class="relative">
    <slot />
    <button
      type="button"
      :disabled="props.loading"
      :class="[
        'absolute right-2 flex items-center justify-center w-5 h-5 rounded text-muted-foreground hover:text-primary transition-colors disabled:opacity-40 disabled:cursor-not-allowed',
        props.multiline ? 'top-2' : 'top-1/2 -translate-y-1/2'
      ]"
      title="AI 生成"
      @click.prevent="emit('generate')"
    >
      <svg v-if="props.loading" class="animate-spin w-4 h-4" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
        <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4" />
        <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8H4z" />
      </svg>
      <svg v-else class="w-4 h-4" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M12 2L13.5 8.5L20 10L13.5 11.5L12 18L10.5 11.5L4 10L10.5 8.5L12 2Z" fill="currentColor" opacity="0.9"/>
        <path d="M19 16L19.75 18.25L22 19L19.75 19.75L19 22L18.25 19.75L16 19L18.25 18.25L19 16Z" fill="currentColor" opacity="0.6"/>
      </svg>
    </button>
  </div>
</template>
