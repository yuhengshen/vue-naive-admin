<template>
  <n-layout has-sider wh-full>
    <n-layout-sider
      v-if="isLargeScreen"
      bordered
      collapse-mode="width"
      :collapsed-width="64"
      :width="220"
      :native-scrollbar="false"
      :collapsed="appStore.collapsed"
    >
      <SideBar />
    </n-layout-sider>
    <n-drawer
      v-else
      v-model:show="showDrawer"
      display-directive="show"
      :width="220"
      :native-scrollbar="false"
      placement="left"
    >
      <SideBar @menu-select="showDrawer = false" />
    </n-drawer>

    <article flex-1 flex-col overflow-hidden>
      <header bg-white px-15 border-b bc-eee flex items-center :style="`height: ${header.height}px`">
        <AppHeader />
      </header>
      <section v-if="tags.visible" border-b bc-eee>
        <AppTags :style="{ height: `${tags.height}px` }" />
      </section>
      <section flex-1 overflow-hidden>
        <AppMain />
      </section>
    </article>
  </n-layout>
</template>

<script setup>
import AppHeader from './components/header/index.vue'
import SideBar from './components/sidebar/index.vue'
import AppMain from './components/AppMain.vue'
import AppTags from './components/tags/index.vue'
import { useAppStore } from '@/store'
import { header, tags } from '~/settings'
import { useMediaQuery } from '@vueuse/core'

const appStore = useAppStore()
const isLargeScreen = useMediaQuery('(min-width: 960px)')

if (!isLargeScreen.value) {
  appStore.setCollapsed(true)
}

const showDrawer = computed({
  get() {
    return !appStore.collapsed
  },
  set(val) {
    appStore.setCollapsed(!val)
  },
})
</script>
