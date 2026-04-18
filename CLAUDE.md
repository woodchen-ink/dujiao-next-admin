# dujiao-next-admin 管理后台

Vue 3 + TypeScript + Vite 管理端前端。

## 技术栈

- Vue 3 / TypeScript / Vite
- Reka-UI + shadcn/ui 组件库
- TanStack Vue Table 表格
- TipTap 富文本编辑器
- Vue-i18n 国际化（zh-CN / zh-TW / en-US）
- Fetch API 自定义封装（`src/api/client.ts`）

## 目录结构

```
src/
  api/
    admin.ts        # 所有管理端 API 方法
    client.ts       # HTTP 客户端封装
    types.ts        # API 类型定义
  components/
    admin/
      AIInputWrapper.vue  # 输入框内嵌 AI 生成按钮组件（新增）
      MediaPicker.vue
  views/admin/
    Categories.vue         # 分类管理（含 AI 功能）
    components/
      ProductEditModal.vue # 商品编辑弹窗（含 AI 功能）
```

## AI 辅助生成功能

管理端通过 `adminAPI.aiGenerate(action, data)` 调用后端 `/api/v1/admin/ai/generate`。

### AIInputWrapper 组件

包装任意 Input/Textarea，在输入框右侧内嵌 ✦ AI 图标按钮：

```vue
<AIInputWrapper :loading="aiLoading['action_key']" @generate="handler">
  <Input v-model="value" class="pr-8" />
</AIInputWrapper>
```

- `multiline` prop：Textarea 场景时按钮定位到右上角（而非垂直居中）
- 需要在 Input/Textarea 上加 `class="pr-8"` 防止文字被遮挡

### 分类管理 AI 功能

- 简体中文名称旁：点击 ✦ 自动翻译填充繁体和英文
- Slug 字段旁：点击 ✦ 根据分类名称生成 slug

### 商品编辑 AI 功能

| 字段 | 简体中文 | 其他语言 |
|------|---------|---------|
| 商品名称 | 规整为「[分类] 名称」格式 | 翻译填充 |
| Slug | 生成 slug | - |
| SEO 关键词 | 生成关键词 | 翻译填充 |
| SEO 描述 | 生成描述 | 翻译填充 |
| 商品简介 | 根据分类/名称/详情生成 | 翻译填充 |
| 商品详情 | AI 优化按钮（在 label 旁） | AI 翻译按钮 |

## 编码约定

- 所有 API 方法统一在 `src/api/admin.ts` 的 `adminAPI` 对象中声明
- 多语言字段结构：`{ 'zh-CN': '', 'zh-TW': '', 'en-US': '' }`
- 通知工具：`notifyError` / `notifySuccess`（`src/utils/notify.ts`）
- 环境变量：`VITE_API_BASE_URL`（API 基址）
