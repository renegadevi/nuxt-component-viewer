# Nuxt Component Viewer

Simple and fully dynamic single file alternative to Storyboard for Nuxt, accessable via `/_components`.

***Demo***: https://stackblitz.com/edit/nuxt-starter-uotxqv

![screenshot-1](https://github.com/renegadevi/nuxt-component-viewer/blob//.github/screenshot-1.png)

![screenshot-2](https://github.com/renegadevi/nuxt-component-viewer/blob//.github/screenshot-2.png)


## Features
- Component viewer
- Localization (i18n)
- 100% Tailwind
- Fully dynamic
- Quick toggles for component/viewport
- Light/Dark mode
- Single page

## Setup

### Prerequisites:
- [pnpm](https://pnpm.io/)
- [Nuxt](https://nuxt.com/§)
- [TailwindCSS](https://nuxt.com/modules/tailwindcss)
- [i18n (optional)](https://nuxt.com/modules/i18n/)
- [Color mode (optional)](https://nuxt.com/modules/color-mode)


### Minimal install guide

1. Create new nuxt project
```sh
pnpm dlx nuxi@latest init nuxt-project
cd nuxt-project
```

2. Install packages
```sh
pnpm install
pnpm add --save-dev @nuxtjs/tailwindcss
```

4. add tailwind as module to `nuxt.config.ts`
```ts
export default defineNuxtConfig({
  devtools: { enabled: true },
  modules: [
    "@nuxtjs/tailwindcss",
  ],
})
```

5. update `.app` to enable pages
```tsx
<template>
  <div>
    <NuxtLayout>
      <NuxtPage  />
    </NuxtLayout>
  </div>
</template>
```
6. create `/pages` folder

7. create `/pages/index.vue` file with a link
```tsx
<template>
    <div>
        <NuxtLink to="/_components/">Go to ComponentViewer</NuxtLink>
    </div>
</template>
```

8. copy the `_components.vue` file from this repo to `/pages/_components.vue`

9. create folder `/layouts`

10. create files `/layouts/default.vue`and `/layouts/blank.vue` with this template
```tsx
<template>
    <div>
      <slot />
    </div>
  </template>
```

11. Create `/components` folder

12. Create component file `/components/ButtonComponent.vue`
```tsx
<template>
    <button class="bg-blue-400 p-4 py-2 text-white rounded-xl">A nice button</button>
</template>
```

12. Start server
```sh
pnpm run dev
```

13. Navigate to http://localhost:3000/_components

