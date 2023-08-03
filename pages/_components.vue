<script setup>
// Prob need some clean up in this file.
import { ref, defineAsyncComponent } from "vue";
definePageMeta({ layout: "blank" });

const currentComponent = ref("");
const resolvedComponent = ref(null);
const expandElement = ref(false);
const menuItems = ref([]);
const addMargin = ref(true);
const showSidebar = ref(false);
const addPadding = ref(false);
const showToolbar = ref(true);
const maximize = ref(false);
const showOutline = ref(false);
const showBorder = ref(false);
const showBoundaries = ref(false);
const responsiveSelected = ref("max-w-none");
const responsiveOptions = ref([
  "max-w-none",
  "max-w-xs",
  "max-w-sm",
  "max-w-md",
  "max-w-lg",
  "max-w-xl",
  "max-w-2xl",
  "max-w-3xl",
  "max-w-4xl",
  "max-w-5xl",
  "max-w-6xl",
  "max-w-7xl",
  "max-w-full",
  "max-w-min",
  "max-w-max",
  "max-w-fit",
  "max-w-prose",
  "max-w-screen-sm",
  "max-w-screen-md",
  "max-w-screen-lg",
  "max-w-screen-xl",
  "max-w-screen-2xl",
]);
const bgColorSelected = ref("bg-inherit");
const bgColors = ref([
  "bg-inherit",
  "bg-current",
  "bg-transparent",
  "bg-black",
  "bg-white",
  "bg-slate-50",
  "bg-slate-100",
  "bg-slate-200",
  "bg-slate-300",
  "bg-slate-400",
  "bg-slate-500",
  "bg-slate-600",
  "bg-slate-700",
  "bg-slate-800",
  "bg-slate-900",
  "bg-slate-950",
  "bg-gray-50",
  "bg-gray-100",
  "bg-gray-200",
  "bg-gray-300",
  "bg-gray-400",
  "bg-gray-500",
  "bg-gray-600",
  "bg-gray-700",
  "bg-gray-800",
  "bg-gray-900",
  "bg-gray-950",
  "bg-zinc-50",
  "bg-zinc-100",
  "bg-zinc-200",
  "bg-zinc-300",
  "bg-zinc-400",
  "bg-zinc-500",
  "bg-zinc-600",
  "bg-zinc-700",
  "bg-zinc-800",
  "bg-zinc-900",
  "bg-zinc-950",
  "bg-neutral-50",
  "bg-neutral-100",
  "bg-neutral-200",
  "bg-neutral-300",
  "bg-neutral-400",
  "bg-neutral-500",
  "bg-neutral-600",
  "bg-neutral-700",
  "bg-neutral-800",
  "bg-neutral-900",
  "bg-neutral-950",
]);

// Generate nested menu
const componentTree = computed(() => {
  for (const fullPath in import.meta.glob("../components/**/*.vue")) {
    menuItems.value.push(fullPath);
  }
  const groupedItems = {};
  for (const item of menuItems.value) {
    const directory = getDirectory(item);
    if (Object.prototype.hasOwnProperty.call(groupedItems, directory)) {
      groupedItems[directory].push({
        label: getFilename(item),
        componentName: getComponentName(item),
        path: item,
      });
    } else {
      groupedItems[directory] = [
        {
          label: getFilename(item),
          componentName: getComponentName(item),
          path: item,
        },
      ];
    }
  }
  return Object.entries(groupedItems).map(([directory, items]) => ({
    directory,
    items,
  }));
});

// Utility functions
const getDirectory = (item) => {
  const parts = item.split("/");
  parts.pop();
  return parts.join("/");
};
const getComponentName = (item) => {
  return item.replace(".vue", "").replace("../components/", "");
};
const getComponentPath = (item) => {
  return item.replace("../components/", "").replace(".vue", "");
};
const getFilename = (item) => {
  return item.split("/").pop().replace(".vue", "");
};
const getCategoryDepth = (item) => {
  return item.split("/").splice(2).length;
};
const getCategoryName = (item) => {
  return item.split("/").splice(1).pop();
};

// Component loading functions
const importComponent = async (componentName) => {
  const combinedString = `../components/${componentName}.vue`;
  const componentModule = await import(combinedString /* @vite-ignore */);
  return componentModule.default || componentModule;
};
const changeComponent = (componentName) => {
  currentComponent.value = componentName;
  const component = defineAsyncComponent(() => importComponent(componentName));
  resolvedComponent.value = component;
};
</script>
<template>
  <div
    class="h-screen w-full overflow-hidden bg-gray-100 text-gray-700 dark:bg-gray-950 dark:text-gray-200"
    :class="{
      overflow: maximize,
    }"
  >
    <main>
      <div class="flex flex-row">
        <!-- Sidebar -->
        <aside
          class="relative flex max-h-full bg-white dark:bg-slate-900"
          :class="{
            hidden: maximize,
          }"
        >
          <div class="flex">
            <!-- Sidebar icons -->
            <div
              class="flex min-h-screen flex-col border-r border-gray-200 pb-7 dark:border-gray-800 dark:bg-slate-950"
            >
              <div class="">
                <button
                  class="m-1 flex items-center justify-center rounded-lg p-3 align-middle hover:bg-gray-300 dark:hover:bg-gray-700"
                  @click="showSidebar = !showSidebar"
                >
                  <svg
                    v-if="!showSidebar"
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    aria-hidden="true"
                    role="img"
                    class="icon"
                    style=""
                    width="22px"
                    height="22px"
                    viewBox="0 0 16 16"
                    data-v-b3bfdaf0=""
                  >
                    <path
                      fill="currentColor"
                      fill-rule="evenodd"
                      d="M0 12.25a.75.75 0 0 0 1.5 0v-8.5a.75.75 0 0 0-1.5 0v8.5Zm7.841-8.03a.75.75 0 0 1 0 1.06l-1.97 1.97h9.379a.75.75 0 0 1 0 1.5H5.871l1.97 1.97a.75.75 0 1 1-1.06 1.06L3.53 8.53L3 8l.53-.53l3.25-3.25a.75.75 0 0 1 1.061 0Z"
                      clip-rule="evenodd"
                    ></path>
                  </svg>

                  <svg
                    v-else
                    data-v-b3bfdaf0=""
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    aria-hidden="true"
                    role="img"
                    class="icon"
                    width="22px"
                    height="22px"
                    viewBox="0 0 16 16"
                  >
                    <path
                      fill="currentColor"
                      fill-rule="evenodd"
                      d="M14.5 3.75a.75.75 0 0 1 1.5 0v8.5a.75.75 0 0 1-1.5 0v-8.5Zm-6.341.47a.75.75 0 0 0 0 1.06l1.97 1.97H.75a.75.75 0 0 0 0 1.5h9.379l-1.97 1.97a.75.75 0 1 0 1.06 1.06l3.25-3.25L13 8l-.53-.53l-3.25-3.25a.75.75 0 0 0-1.061 0Z"
                      clip-rule="evenodd"
                    ></path>
                  </svg>
                </button>
              </div>

              <div class="">
                <a
                  class="m-1 flex items-center justify-center rounded-lg p-3 align-middle hover:bg-gray-300 dark:hover:bg-gray-700"
                  href="https://github.com/renegadevi/nuxt-component-viewer"
                  target="_blank"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    aria-hidden="true"
                    role="img"
                    class="icon dark:invert"
                    style=""
                    width="22px"
                    height="22px"
                    viewBox="0 0 256 250"
                    data-v-b3bfdaf0=""
                  >
                    <path
                      fill="#161614"
                      d="M128.001 0C57.317 0 0 57.307 0 128.001c0 56.554 36.676 104.535 87.535 121.46c6.397 1.185 8.746-2.777 8.746-6.158c0-3.052-.12-13.135-.174-23.83c-35.61 7.742-43.124-15.103-43.124-15.103c-5.823-14.795-14.213-18.73-14.213-18.73c-11.613-7.944.876-7.78.876-7.78c12.853.902 19.621 13.19 19.621 13.19c11.417 19.568 29.945 13.911 37.249 10.64c1.149-8.272 4.466-13.92 8.127-17.116c-28.431-3.236-58.318-14.212-58.318-63.258c0-13.975 5-25.394 13.188-34.358c-1.329-3.224-5.71-16.242 1.24-33.874c0 0 10.749-3.44 35.21 13.121c10.21-2.836 21.16-4.258 32.038-4.307c10.878.049 21.837 1.47 32.066 4.307c24.431-16.56 35.165-13.12 35.165-13.12c6.967 17.63 2.584 30.65 1.255 33.873c8.207 8.964 13.173 20.383 13.173 34.358c0 49.163-29.944 59.988-58.447 63.157c4.591 3.972 8.682 11.762 8.682 23.704c0 17.126-.148 30.91-.148 35.126c0 3.407 2.304 7.398 8.792 6.14C219.37 232.5 256 184.537 256 128.002C256 57.307 198.691 0 128.001 0Zm-80.06 182.34c-.282.636-1.283.827-2.194.39c-.929-.417-1.45-1.284-1.15-1.922c.276-.655 1.279-.838 2.205-.399c.93.418 1.46 1.293 1.139 1.931Zm6.296 5.618c-.61.566-1.804.303-2.614-.591c-.837-.892-.994-2.086-.375-2.66c.63-.566 1.787-.301 2.626.591c.838.903 1 2.088.363 2.66Zm4.32 7.188c-.785.545-2.067.034-2.86-1.104c-.784-1.138-.784-2.503.017-3.05c.795-.547 2.058-.055 2.861 1.075c.782 1.157.782 2.522-.019 3.08Zm7.304 8.325c-.701.774-2.196.566-3.29-.49c-1.119-1.032-1.43-2.496-.726-3.27c.71-.776 2.213-.558 3.315.49c1.11 1.03 1.45 2.505.701 3.27Zm9.442 2.81c-.31 1.003-1.75 1.459-3.199 1.033c-1.448-.439-2.395-1.613-2.103-2.626c.301-1.01 1.747-1.484 3.207-1.028c1.446.436 2.396 1.602 2.095 2.622Zm10.744 1.193c.036 1.055-1.193 1.93-2.715 1.95c-1.53.034-2.769-.82-2.786-1.86c0-1.065 1.202-1.932 2.733-1.958c1.522-.03 2.768.818 2.768 1.868Zm10.555-.405c.182 1.03-.875 2.088-2.387 2.37c-1.485.271-2.861-.365-3.05-1.386c-.184-1.056.893-2.114 2.376-2.387c1.514-.263 2.868.356 3.061 1.403Z"
                    ></path>
                  </svg>
                </a>
              </div>

              <div class="">
                <a
                  href="/_tailwind/"
                  target="_blank"
                  class="m-1 flex items-center justify-center rounded-lg p-3 align-middle hover:bg-gray-300 dark:hover:bg-gray-700"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    aria-hidden="true"
                    role="img"
                    class="icon"
                    style=""
                    width="22px"
                    height="22px"
                    viewBox="0 0 256 154"
                    data-v-b3bfdaf0=""
                  >
                    <defs>
                      <linearGradient
                        id="iconifyVue0"
                        x1="-2.778%"
                        x2="100%"
                        y1="32%"
                        y2="67.556%"
                      >
                        <stop offset="0%" stop-color="#2298BD"></stop>
                        <stop offset="100%" stop-color="#0ED7B5"></stop>
                      </linearGradient>
                    </defs>
                    <path
                      fill="url(#iconifyVue0)"
                      d="M128 0C93.867 0 72.533 17.067 64 51.2C76.8 34.133 91.733 27.733 108.8 32c9.737 2.434 16.697 9.499 24.401 17.318C145.751 62.057 160.275 76.8 192 76.8c34.133 0 55.467-17.067 64-51.2c-12.8 17.067-27.733 23.467-44.8 19.2c-9.737-2.434-16.697-9.499-24.401-17.318C174.249 14.743 159.725 0 128 0ZM64 76.8C29.867 76.8 8.533 93.867 0 128c12.8-17.067 27.733-23.467 44.8-19.2c9.737 2.434 16.697 9.499 24.401 17.318C81.751 138.857 96.275 153.6 128 153.6c34.133 0 55.467-17.067 64-51.2c-12.8 17.067-27.733 23.467-44.8 19.2c-9.737-2.434-16.697-9.499-24.401-17.318C110.249 91.543 95.725 76.8 64 76.8Z"
                    ></path>
                  </svg>
                </a>
              </div>
            </div>
            <!-- Sidebar tree -->
            <div class="border-r border-gray-200 dark:border-gray-800">
              <!-- header -->
              <div
                class="mb-0 flex border-b border-gray-200 p-4 px-6 pr-2 align-middle text-xl font-bold dark:border-gray-700"
                :class="{ hidden: showSidebar }"
              >
                <button
                  class="mr-8 flex align-middle"
                  @click="currentComponent = ''"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    aria-hidden="true"
                    role="img"
                    class="icon mr-1"
                    style=""
                    width="24px"
                    height="24px"
                    viewBox="0 0 36 36"
                    data-v-b3bfdaf0=""
                  >
                    <path
                      fill="currentColor"
                      d="m33.53 18.76l-6.93-3.19V6.43a1 1 0 0 0-.6-.9l-7.5-3.45a1 1 0 0 0-.84 0l-7.5 3.45a1 1 0 0 0-.58.91v9.14l-6.9 3.18a1 1 0 0 0-.58.91v9.78a1 1 0 0 0 .58.91l7.5 3.45a1 1 0 0 0 .84 0l7.08-3.26l7.08 3.26a1 1 0 0 0 .84 0l7.5-3.45a1 1 0 0 0 .58-.91v-9.78a1 1 0 0 0-.57-.91ZM25.61 22l-5.11-2.33l5.11-2.35l5.11 2.35Zm-1-6.44l-6.44 3v-7.69a1 1 0 0 0 .35-.08L24.6 8v7.58ZM18.1 4.08l5.11 2.35l-5.11 2.35L13 6.43Zm-7.5 13.23l5.11 2.35L10.6 22l-5.11-2.33Zm6.5 11.49l-6.5 3v-7.69A1 1 0 0 0 11 24l6.08-2.8Zm15 0l-6.46 3v-7.69A1 1 0 0 0 26 24l6.08-2.8Z"
                      class="clr-i-solid clr-i-solid-path-1"
                    ></path>
                    <path fill="none" d="M0 0h36v36H0z"></path>
                  </svg>
                  Component Viewer
                </button>
              </div>

              <!-- tree -->
              <ul
                class="h-screen overflow-scroll p-4 pb-20 pr-6 pt-4"
                :class="{ hidden: showSidebar }"
              >
                <li
                  v-for="(menuItem, menuItemIndex) in componentTree"
                  :key="menuItem.directory"
                  :style="`margin-left: calc(${getCategoryDepth(
                    menuItem.directory
                  )}em*1.5)`"
                >
                  <span class="z-20 flex font-bold capitalize">
                    <span
                      class="inline-flex rounded-lg bg-gray-100 p-[.5rem] dark:bg-gray-800"
                      ><svg
                        xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink"
                        aria-hidden="true"
                        role="img"
                        class="icon"
                        style=""
                        width="16px"
                        height="16px"
                        viewBox="0 0 36 36"
                        data-v-b3bfdaf0=""
                      >
                        <path
                          fill="currentColor"
                          d="m31.42 9.09l-13-6a1 1 0 0 0-.84 0l-13 6A1 1 0 0 0 4 10v17a1 1 0 0 0 .58.91l13 6a1 1 0 0 0 .84 0l13-6A1 1 0 0 0 32 27V10a1 1 0 0 0-.58-.91ZM18 14.9L7.39 10L18 5.1L28.61 10Zm12 11.46l-11 5.08v-14.8l11-5.08Z"
                          class="clr-i-solid clr-i-solid-path-1"
                        ></path>
                        <path fill="none" d="M0 0h36v36H0z"></path></svg
                    ></span>
                    <span class="ml-2 mt-1">{{
                      getCategoryName(menuItem.directory)
                    }}</span>
                  </span>

                  <div
                    v-if="menuItemIndex > 3 || menuItemIndex == 1"
                    class="bottom-[2rem] z-10 mb-4 ml-[-.5rem] mt-2 w-2 border-b-2 border-gray-200 dark:border-gray-700"
                    style="margin-top: -1.1rem"
                  ></div>
                  <ul
                    class="mb-[-1rem] ml-4 border-l-2 border-gray-200 pb-7 dark:border-gray-700"
                    :class="{
                      '!mb-[-8rem] border-l-2 pb-0':
                        menuItemIndex == componentTree.length - 1,
                      '!pb-0': componentTree.length < 1,
                    }"
                  >
                    <li
                      v-for="item in menuItem.items"
                      :key="item.label"
                      class="ml-[-.8rem] first:pt-2 last:pb-0"
                    >
                      <button
                        class="ml-3 flex w-full space-x-1"
                        :class="{
                          'last:mb-0 ':
                            menuItemIndex == componentTree.length - 1 &&
                            componentTree.length > 2,
                        }"
                        @click="changeComponent(getComponentPath(item.path))"
                      >
                        <div
                          class="relative mr-[-.3rem] mt-5 w-4 border-b-2 border-gray-200 dark:border-gray-700 dark:hover:text-gray-400"
                        ></div>
                        <span
                          class="my-1 rounded px-3 py-1 hover:cursor-pointer hover:bg-gray-100 hover:text-blue-600 dark:hover:bg-gray-600 dark:hover:text-gray-200"
                          :class="{
                            'bg-gray-200 dark:bg-gray-700':
                              currentComponent == item.componentName,
                          }"
                          >{{ item.label }}
                        </span>
                      </button>
                    </li>
                  </ul>

                  <div
                    v-if="menuItemIndex == componentTree.length - 1"
                    class="h-10"
                  ></div>
                  <!-- last-->
                </li>
              </ul>
            </div>
          </div>
        </aside>

        <!-- Main content -->
        <div class="flex-1">
          <!-- window -->
          <div>
            <!-- maximize button -->
            <div class="absolute right-1 top-1 z-50">
              <button
                class="flex justify-center rounded p-2 hover:bg-gray-300"
                :class="{
                  'text-gray-500': maximize,
                }"
                @click="maximize = !maximize"
              >
                <svg
                  v-if="maximize"
                  data-v-b3bfdaf0=""
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                  aria-hidden="true"
                  role="img"
                  class="icon"
                  width="20px"
                  height="20px"
                  viewBox="0 0 24 24"
                >
                  <path
                    fill="none"
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M6 14h4m0 0v4m0-4l-6 6m14-10h-4m0 0V6m0 4l6-6"
                  ></path>
                </svg>
                <svg
                  v-else
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                  aria-hidden="true"
                  role="img"
                  class="icon"
                  style=""
                  width="20px"
                  height="20px"
                  viewBox="0 0 24 24"
                  data-v-b3bfdaf0=""
                >
                  <path
                    fill="none"
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M8 20H4m0 0v-4m0 4l6-6m6-10h4m0 0v4m0-4l-6 6"
                  ></path>
                </svg>
              </button>
            </div>
            <!-- Main area -->
            <div
              class="flex h-screen grow flex-col items-stretch overflow-hidden"
            >
              <!-- empty state / component area -->
              <div v-if="currentComponent == ''">
                <div
                  class="flex justify-center p-20 text-center align-middle font-bold"
                >
                  <span class="text-xl">Select a component in the sidebar</span>
                </div>
              </div>
              <div
                v-else
                class="z-10 flex grow flex-col overflow-auto"
                :class="{
                  'm-4': addMargin,
                  [responsiveSelected]: true,
                }"
              >
                <div
                  :class="`${bgColorSelected} dark:${bgColorSelected}`"
                  class="overflow-auto"
                >
                  <div
                    :class="{
                      flex: !expandElement,
                      'border border-gray-300': showBoundaries,
                      'p-4': addPadding,
                    }"
                    class="overflow-auto"
                  >
                    <!-- Component -->
                    <component
                      :is="resolvedComponent"
                      v-if="resolvedComponent"
                      :class="{
                        showOutline: showOutline,
                        border: showBorder,
                        'w-full': expandElement,
                      }"
                      class="overflow-auto"
                    ></component>
                  </div>
                </div>
              </div>
              <!-- Toolbar-->
              <div
                v-if="currentComponent != ''"
                class="w-fit bg-white dark:bg-slate-800"
                :class="{
                  hidden: maximize,
                }"
                style="width: -webkit-fill-available"
              >
                <button
                  class="flex w-full justify-center border-t border-gray-200 p-2 hover:bg-gray-100 dark:border-gray-700 dark:hover:bg-gray-900"
                  @click="showToolbar = !showToolbar"
                >
                  <svg
                    v-if="showToolbar"
                    data-v-b3bfdaf0=""
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    aria-hidden="true"
                    role="img"
                    class="icon"
                    width="14px"
                    height="14px"
                    viewBox="0 0 24 24"
                  >
                    <path
                      fill="currentColor"
                      d="M19.92 12.08L12 20l-7.92-7.92l1.42-1.41l5.5 5.5V2h2v14.17l5.5-5.51l1.42 1.42M12 20H2v2h20v-2H12Z"
                    ></path>
                  </svg>
                  <svg
                    v-else
                    data-v-b3bfdaf0=""
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    aria-hidden="true"
                    role="img"
                    class="icon"
                    width="16px"
                    height="16px"
                    viewBox="0 0 24 24"
                  >
                    <path
                      fill="currentColor"
                      d="M4.08 11.92L12 4l7.92 7.92l-1.42 1.41l-5.5-5.5V22h-2V7.83l-5.5 5.5l-1.42-1.41M12 4h10V2H2v2h10Z"
                    ></path>
                  </svg>
                </button>

                <div
                  class="grid grid-cols-2 gap-4 border-t p-6 dark:border-gray-700 dark:bg-gray-800 lg:grid-cols-10"
                  :class="{ hidden: !showToolbar }"
                >
                  <div class="font-bold">Global</div>
                  <div class="col-span-2 grid grid-cols-2 gap-4 md:col-span-9">
                    <div class="col-span-2 grid grid-cols-5">
                      <div>
                        <form v-if="$i18n">
                          <svg
                            data-v-b3bfdaf0=""
                            xmlns="http://www.w3.org/2000/svg"
                            xmlns:xlink="http://www.w3.org/1999/xlink"
                            aria-hidden="true"
                            role="img"
                            class="icon mr-2"
                            width="28px"
                            height="28px"
                            viewBox="0 0 24 24"
                          >
                            <path
                              fill="currentColor"
                              d="m20.58 19.37l-2.99-8.36c-.21-.55-.68-.89-1.22-.89s-1 .34-1.23.91l-2.98 8.34a.75.75 0 1 0 1.41.51l.62-1.73h4.35l.62 1.73c.11.31.4.5.71.5c.08 0 .17-.01.25-.04a.75.75 0 0 0 .45-.96Zm-5.84-2.73l1.64-4.59l1.64 4.59h-3.28Zm-2.55-8.79c-2.26 3.57-4.3 5.73-6.78 7.17a.746.746 0 0 1-1.02-.27a.738.738 0 0 1 .27-1.02c2.1-1.22 3.82-2.97 5.75-5.87H4.12c-.41 0-.75-.34-.75-.75s.34-.75.75-.75h3.75V4.38c0-.41.34-.75.75-.75s.75.34.75.75v1.98h3.75c.41 0 .75.34.75.75s-.34.75-.75.75h-.94Zm.04 7.27c-.13 0-.26-.03-.38-.1c-.65-.38-1.28-.8-1.87-1.24a.75.75 0 0 1 .9-1.2c.54.41 1.13.79 1.73 1.14a.752.752 0 0 1-.38 1.4Z"
                            ></path>
                          </svg>
                          <select
                            v-model="$i18n.locale"
                            class="w-32 rounded-md bg-gray-100 p-2 dark:bg-gray-600"
                          >
                            <option
                              v-for="language in $i18n.availableLocales"
                              :key="`${language}`"
                              :value="language"
                            >
                              {{ $t(language) }}
                            </option>
                          </select>
                        </form>
                        <span v-else>Missing "@nuxtjs/i18n"</span>
                      </div>
                      <div>
                        <form v-if="$colorMode">
                          <svg
                            data-v-b3bfdaf0=""
                            xmlns="http://www.w3.org/2000/svg"
                            xmlns:xlink="http://www.w3.org/1999/xlink"
                            aria-hidden="true"
                            role="img"
                            class="icon mr-2"
                            width="28px"
                            height="28px"
                            viewBox="0 0 24 24"
                          >
                            <g fill="currentColor">
                              <path d="M12 16a4 4 0 0 0 0-8v8Z"></path>
                              <path
                                fill-rule="evenodd"
                                d="M12 2C6.477 2 2 6.477 2 12s4.477 10 10 10s10-4.477 10-10S17.523 2 12 2Zm0 2v4a4 4 0 1 0 0 8v4a8 8 0 1 0 0-16Z"
                                clip-rule="evenodd"
                              ></path>
                            </g>
                          </svg>
                          <select
                            v-model="$colorMode.preference"
                            class="w-32 rounded-md bg-gray-100 p-2 dark:bg-gray-600"
                          >
                            <option value="system">System</option>
                            <option value="light">Light</option>
                            <option value="dark">Dark</option>
                          </select>
                        </form>
                        <span v-else>Missing "@nuxtjs/color-mode"</span>
                      </div>
                    </div>
                  </div>
                  <div class="font-bold">Viewport</div>
                  <div class="col-span-2 grid grid-cols-2 gap-4 md:col-span-9">
                    <div class="col-span-2 grid grid-cols-3 md:grid-cols-5">
                      <div>
                        <label
                          class="group relative flex cursor-pointer items-center justify-start p-1"
                        >
                          <input
                            v-model="addMargin"
                            type="checkbox"
                            class="peer absolute left-1/2 h-full w-full -translate-x-1/2 cursor-pointer appearance-none rounded-md"
                          />
                          <span
                            class="mr-4 flex h-5 w-10 flex-shrink-0 cursor-pointer items-center rounded-full bg-gray-300 p-1 duration-300 ease-in-out after:h-4 after:w-4 after:rounded-full after:bg-white after:shadow-md after:duration-300 peer-checked:bg-green-400 peer-checked:after:translate-x-4 dark:bg-gray-500 dark:after:bg-gray-700 dark:peer-checked:bg-green-600"
                          ></span>
                          Add margin
                        </label>
                      </div>
                      <div>
                        <label
                          class="group relative flex cursor-pointer items-center justify-start p-1"
                        >
                          <input
                            v-model="addPadding"
                            type="checkbox"
                            class="peer absolute left-1/2 h-full w-full -translate-x-1/2 cursor-pointer appearance-none rounded-md"
                          />
                          <span
                            class="mr-4 flex h-5 w-10 flex-shrink-0 cursor-pointer items-center rounded-full bg-gray-300 p-1 duration-300 ease-in-out after:h-4 after:w-4 after:rounded-full after:bg-white after:shadow-md after:duration-300 peer-checked:bg-green-400 peer-checked:after:translate-x-4 dark:bg-gray-500 dark:after:bg-gray-700 dark:peer-checked:bg-green-600"
                          ></span>
                          Add padding
                        </label>
                      </div>
                      <div>
                        <label
                          class="group relative flex cursor-pointer items-center justify-start p-1"
                        >
                          <input
                            v-model="showBoundaries"
                            type="checkbox"
                            class="peer absolute left-1/2 h-full w-full -translate-x-1/2 cursor-pointer appearance-none rounded-md"
                          />
                          <span
                            class="mr-4 flex h-5 w-10 flex-shrink-0 cursor-pointer items-center rounded-full bg-gray-300 p-1 duration-300 ease-in-out after:h-4 after:w-4 after:rounded-full after:bg-white after:shadow-md after:duration-300 peer-checked:bg-green-400 peer-checked:after:translate-x-4 dark:bg-gray-500 dark:after:bg-gray-700 dark:peer-checked:bg-green-600"
                          ></span>
                          Show boundaries
                        </label>
                      </div>
                      <div>
                        <select
                          v-model="responsiveSelected"
                          class="rounded-md bg-gray-100 p-2 dark:bg-gray-600"
                        >
                          <option
                            v-for="size in responsiveOptions"
                            :key="size"
                            :value="size"
                          >
                            {{ size }}
                          </option>
                        </select>
                      </div>
                      <div class="flex">
                        <div
                          class="mr-1 h-full w-10 rounded-md border border-gray-300 dark:border-gray-700"
                          :class="bgColorSelected"
                        ></div>
                        <select
                          v-model="bgColorSelected"
                          class="rounded-md bg-gray-100 p-2 dark:bg-gray-600"
                        >
                          <option
                            v-for="color in bgColors"
                            :key="color"
                            :value="color"
                          >
                            {{ color }}
                          </option>
                        </select>
                      </div>
                    </div>
                  </div>
                  <div class="font-bold">Component</div>
                  <div class="col-span-9 grid grid-cols-2 gap-4">
                    <div class="col-span-2 grid grid-cols-5">
                      <div>
                        <label
                          class="group relative flex cursor-pointer items-center justify-start p-1"
                        >
                          <input
                            v-model="showOutline"
                            type="checkbox"
                            class="peer absolute left-1/2 h-full w-full -translate-x-1/2 cursor-pointer appearance-none rounded-md"
                          />
                          <span
                            class="mr-4 flex h-5 w-10 flex-shrink-0 cursor-pointer items-center rounded-full bg-gray-300 p-1 duration-300 ease-in-out after:h-4 after:w-4 after:rounded-full after:bg-white after:shadow-md after:duration-300 peer-checked:bg-green-400 peer-checked:after:translate-x-4 dark:bg-gray-500 dark:after:bg-gray-700 dark:peer-checked:bg-green-600"
                          ></span>
                          Show outline
                        </label>
                      </div>
                      <div>
                        <label
                          class="group relative flex cursor-pointer items-center justify-start p-1"
                        >
                          <input
                            v-model="showBorder"
                            type="checkbox"
                            class="peer absolute left-1/2 h-full w-full -translate-x-1/2 cursor-pointer appearance-none rounded-md"
                          />
                          <span
                            class="mr-4 flex h-5 w-10 flex-shrink-0 cursor-pointer items-center rounded-full bg-gray-300 p-1 duration-300 ease-in-out after:h-4 after:w-4 after:rounded-full after:bg-white after:shadow-md after:duration-300 peer-checked:bg-green-400 peer-checked:after:translate-x-4 dark:bg-gray-500 dark:after:bg-gray-700 dark:peer-checked:bg-green-600"
                          ></span>
                          Show border
                        </label>
                      </div>
                      <div>
                        <label
                          class="group relative flex cursor-pointer items-center justify-start p-1"
                        >
                          <input
                            v-model="expandElement"
                            type="checkbox"
                            class="peer absolute left-1/2 h-full w-full -translate-x-1/2 cursor-pointer appearance-none rounded-md"
                          />
                          <span
                            class="mr-4 flex h-5 w-10 flex-shrink-0 cursor-pointer items-center rounded-full bg-gray-300 p-1 duration-300 ease-in-out after:h-4 after:w-4 after:rounded-full after:bg-white after:shadow-md after:duration-300 peer-checked:bg-green-400 peer-checked:after:translate-x-4 dark:bg-gray-500 dark:after:bg-gray-700 dark:peer-checked:bg-green-600"
                          ></span>
                          Expand
                        </label>
                      </div>
                      <div></div>
                      <div></div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style lang="postcss">
.showOutline,
.showOutline * {
  @apply border border-red-500 !important;
}
::-webkit-scrollbar {
  -webkit-appearance: none;
  width: 7px;
}
::-webkit-scrollbar-thumb {
  border-radius: 4px;
  background-color: rgba(0, 0, 0, 0.5);
  box-shadow: 0 0 1px rgba(255, 255, 255, 0.5);
}
</style>
