# SSXZ 图片工作台 v0.7.8 合并执行计划

> 设计依据：`docs/superpowers/specs/2026-08-30-upstream-v078-merge-design.md`

**目标：** 以官方 v0.7.8 为新基底，迁移并验证 SSXZ 定制，推送独立分支，不发布正式环境。

**技术栈：** React 19、TypeScript、Vite、Zustand、Tailwind CSS、Vitest

---

## 任务 1：创建隔离工作树并验证上游基线

**操作：**

1. 从 `v0.7.8` 创建分支 `codex/ssxz-image-workbench-v078`。
2. 在 `F:\CodexTemp\gpt-image-v078-ssxz-20260830` 创建隔离工作树。
3. 运行：

```powershell
npm ci
npm test
npm run build
```

**完成标准：** 上游原始 v0.7.8 测试和构建通过，当前 SSXZ 工作树保持不动。

## 任务 2：迁移 SSXZ 非冲突定制

**涉及文件：**

- `.env.development`
- `.env.production`
- `index.html`
- `public/manifest.webmanifest`
- `public/pwa-icon.svg`
- `src/components/Header.tsx`
- `src/components/SizePickerModal.tsx`
- `src/components/TaskGrid.tsx`
- `src/hooks/useVersionCheck.ts`
- `docs/superpowers/specs/2026-08-30-upstream-v078-merge-design.md`

**操作：**

1. 逐个迁移 SSXZ 品牌、PWA、主站入口和已确认的界面简化。
2. 保留 v0.7.8 新增依赖、部署文件、Service Worker 行为和数据结构。
3. 不复制 `dist/`、`node_modules/` 或任何密钥。

**完成标准：** 非冲突定制迁移完成，代码差异只包含明确的 SSXZ 产品改造。

## 任务 3：逐段解决六个重叠文件

**涉及文件：**

- `src/components/DetailModal.tsx`
- `src/components/InputBar.tsx`
- `src/components/SettingsModal.tsx`
- `src/components/input/inputParamsPanel.tsx`
- `src/lib/apiProfiles.ts`
- `src/lib/apiProfiles.test.ts`

**操作：**

1. 以 v0.7.8 实现为主体，保留其预置配置、异步供应商和透明背景逻辑。
2. 在显示层恢复 SSXZ 品牌、模型白名单、API Key 提示和服务商入口限制。
3. 在配置层保留同源地址迁移和旧浏览器数据兼容。
4. 扩充测试，覆盖默认同源接口、已验证模型、预置配置更新不覆盖 API Key。

**完成标准：** 不存在冲突标记，旧 SSXZ 定制与 v0.7.8 新功能同时可用。

## 任务 4：代码级回归验证

**操作：**

```powershell
npm test -- src/lib/apiProfiles.test.ts src/lib/presetConfig.test.ts src/lib/defaultApiUrl.test.ts
npm test
npm run build
git diff --check
```

**检查：**

- 测试数量不低于上游 v0.7.8 基线。
- 构建产物不含 API Key、Token 或真实上游密钥。
- 不出现 `<<<<<<<`、`=======`、`>>>>>>>` 冲突标记。

## 任务 5：真实页面验收

**页面：** 本地 Vite 预览站点

**桌面端检查：**

1. 页面标题、Logo、PWA 名称和主站入口均为 SSXZ。
2. 设置页只展示允许的 SSXZ 配置，不出现 fal.ai、自定义供应商或赞助商入口。
3. 默认接口为同源地址，模型列表只包含生产已验证模型。
4. API Key 可由用户填写，但不会被预置配置覆盖或写进构建产物。
5. 1K、2K、4K 尺寸提示和任务卡片交互正常。
6. 透明背景、中文输入法、配置切换和历史记录入口无报错。

**移动端检查：**

- 390px 宽度下无横向溢出。
- 设置弹窗、输入栏、尺寸选择和任务卡片可正常操作。

## 任务 6：提交与推送

**操作：**

1. 将上游基底迁移、SSXZ 适配和验证修复整理为含义清晰的提交。
2. 推送到 fork 的 `codex/ssxz-image-workbench-v078` 分支。
3. 记录目标上游提交、最终提交、测试结果和未发布状态。

**完成标准：** 本地工作区干净、远端分支可追溯；正式环境没有变化。
