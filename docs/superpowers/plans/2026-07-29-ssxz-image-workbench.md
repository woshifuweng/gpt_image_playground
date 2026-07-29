# SSXZ Image Workbench Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Turn the upstream image playground into an SSXZ-branded client that defaults to the production Images API and exposes only verified SSXZ image model IDs.

**Architecture:** Keep the existing OpenAI-compatible request path because SSXZ implements `POST /v1/images/generations`. Configure the deployment through Vite's existing default URL mechanism, restrict provider selection to the locked default, and expose the production model mappings through the existing settings form. Retain the upstream 1K/2K/4K size picker; both GPT size tiers use the verified `gpt-image-2` model ID.

**Tech Stack:** React 19, TypeScript, Vite, Zustand, Tailwind CSS, Vitest

---

### Task 1: Lock the SSXZ connection defaults

**Files:**
- Create: `.env.development`
- Create: `.env.production`
- Modify: `src/lib/apiProfiles.ts`
- Test: `src/lib/apiProfiles.test.ts`

- [x] **Step 1: Configure the default deployment**

Create the development and production environment files with the same public, non-secret settings:

```dotenv
VITE_DEFAULT_API_URL=https://api.ssxzapi.com/v1?apiMode=images&model=gpt-image-2&profileName=SSXZ%20GPT-Image
VITE_SHOW_DEFAULT_CONFIG_ONLY=true
```

- [x] **Step 2: Define the verified model catalog**

Export a fixed catalog containing only:

```ts
export const SSXZ_IMAGE_MODELS = [
  { label: 'GPT-Image (1K / 2K / 4K)', value: 'gpt-image-2' },
  { label: 'Grok Imagine Image (1K)', value: 'grok-imagine-image' },
] as const
```

- [x] **Step 3: Add a catalog regression test**

Assert that the default model is `gpt-image-2` and that the catalog contains exactly the two database-verified IDs.

- [x] **Step 4: Run the focused test**

Run:

```bash
npm test -- src/lib/apiProfiles.test.ts
```

Expected: all `apiProfiles` tests pass.

### Task 2: Apply SSXZ branding and remove unrelated provider choices

**Files:**
- Modify: `index.html`
- Modify: `public/manifest.webmanifest`
- Modify: `public/sw.js`
- Modify: `src/components/Header.tsx`
- Modify: `src/components/SettingsModal.tsx`

- [x] **Step 1: Replace visible product naming**

Use `SSXZ 图片工作台` for the document title, PWA name, and header. Link the header to `https://ssxzapi.com`.

- [x] **Step 2: Restrict the settings UI**

Show the locked `SSXZ Images API` provider only. Keep the existing request implementation intact for upstream compatibility, but do not expose fal.ai or custom-provider creation in the branded interface.

- [x] **Step 3: Replace the free-form model input**

Use the existing `Select` component with `SSXZ_IMAGE_MODELS`. Keep `gpt-image-2` selected by default and preserve the existing settings persistence behavior.

- [x] **Step 4: Update the API Key prompt**

Set the placeholder to:

```text
输入您的 SSXZ API Key
```

### Task 3: Validate the production build

**Files:**
- Verify generated output: `dist/`

- [x] **Step 1: Run the test suite**

Run:

```bash
npm test
```

Expected: all Vitest tests pass.

- [x] **Step 2: Build the application**

Run:

```bash
npm run build
```

Expected: TypeScript and Vite complete successfully and generate `dist/`.

- [x] **Step 3: Inspect the built artifact**

Serve `dist/` locally and verify the title, SSXZ endpoint, API Key prompt, model selector, and absence of unrelated provider choices.
