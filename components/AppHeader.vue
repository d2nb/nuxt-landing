<script setup>
import { clsx } from 'clsx'

const route = useRoute()

const isSidenavOpen = ref(false)

const showSidenav = () => {
  isSidenavOpen.value = true
}

const hideSidenav = () => {
  isSidenavOpen.value = false
}

const isSubnavOpen = ref(false)

const hideSubnav = () => {
  isSubnavOpen.value = false
}

const navs = [
  {
    name: '首页',
    path: '/'
  },
  {
    name: '营销服务',
    path: '/platforms',
    children: [
      {
        name: 'TikTok',
        path: '/platforms/tiktok'
      },
      {
        name: 'Google',
        path: '/platforms/google'
      },
      {
        name: 'Bing',
        path: '/platforms/bing'
      }
    ]
  },
  {
    name: '关于我们',
    path: '/about'
  }
]

const desktopNavClasses = (nav) => {
  return clsx('mr-6 last:mr-0 font-bold', {
    'text-primary': route.path === nav.path
  })
}

const desktopSubnavClasses = (nav) => {
  return clsx('block min-w-[120px] p-2 rounded hover:bg-gray-100', {
    'text-primary': route.path === nav.path
  })
}

const subtitleMatched = (nav) => {
  return route.matched.some((item) => item.path === nav.path)
}

const mobileNavClasses = (nav) => {
  return clsx('block px-2 py-1 rounded active:bg-gray-100', {
    'text-primary': route.path === nav.path
  })
}

const mobileSubnavClasses = (nav) => {
  return clsx('block p-2 text-sm rounded active:bg-gray-100', {
    'text-primary': route.path === nav.path
  })
}

const formatChildren = (navs) => {
  return navs.map((nav) => {
    return {
      label: nav.name
    }
  })
}
</script>

<template>
  <header class="h-16 border-b">
    <UContainer class="flex justify-between items-center h-full">
      <div class="text-2xl font-bold text-primary">Fancy</div>
      <div class="flex md:hidden items-center">
        <UButton
          icon="i-heroicons-bars-3-solid"
          size="xl"
          color="gray"
          variant="link"
          :padded="false"
          @click="showSidenav"
        />
      </div>

      <div class="hidden md:flex items-center">
        <ul class="flex items-center text-gray-500">
          <li v-for="nav in navs" :class="desktopNavClasses(nav)">
            <template v-if="nav.children">
              <UPopover mode="hover" v-model:open="isSubnavOpen">
                <span :class="{ 'text-primary': subtitleMatched(nav) }">
                  {{ nav.name }}
                </span>
                <template #panel>
                  <ul class="p-2 text-sm font-normal">
                    <li v-for="subnav in nav.children">
                      <NuxtLink
                        :class="desktopSubnavClasses(subnav)"
                        :to="subnav.path"
                        @click="hideSubnav"
                      >
                        {{ subnav.name }}
                      </NuxtLink>
                    </li>
                  </ul>
                </template>
              </UPopover>
            </template>
            <template v-else>
              <NuxtLink :to="nav.path">{{ nav.name }}</NuxtLink>
            </template>
          </li>
        </ul>

        <UButton class="ml-6" size="md">开户试投</UButton>
      </div>

      <USlideover v-model="isSidenavOpen" :ui="{ base: 'flex-none !w-[300px]' }">
        <div class="flex justify-end items-center h-16 px-6 border-b">
          <UButton
            icon="i-heroicons-x-mark"
            size="xl"
            color="gray"
            variant="link"
            :padded="false"
            @click="hideSidenav"
          />
        </div>
        <ul class="p-2 text-gray-500">
          <li v-for="nav in navs" :key="nav.path">
            <template v-if="nav.children">
              <div :class="mobileNavClasses(nav)">{{ nav.name }}</div>
              <ul class="pl-4">
                <li v-for="subnav in nav.children">
                  <NuxtLink
                    :class="mobileSubnavClasses(subnav)"
                    :to="subnav.path"
                    @click="hideSidenav"
                  >
                    {{ subnav.name }}
                  </NuxtLink>
                </li>
              </ul>
            </template>
            <template v-else>
              <NuxtLink :class="mobileNavClasses(nav)" :to="nav.path" @click="hideSidenav">
                {{ nav.name }}
              </NuxtLink>
            </template>
          </li>
        </ul>
      </USlideover>
    </UContainer>
  </header>
</template>
