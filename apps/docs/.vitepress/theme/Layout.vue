<template>
  <b-container fluid class="p-0">
    <b-row>
      <b-col>
        <b-navbar variant="primary">
          <b-navbar-nav>
            <b-navbar-brand :to="withBase('/')">Home</b-navbar-brand>
            <b-nav>
              <b-nav-item :to="withBase('/getting-started')">Getting Started</b-nav-item>
              <b-nav-item :to="withBase('/reference/components')">Components</b-nav-item>
              <b-nav-item :to="withBase('/reference/composables')">Composables</b-nav-item>
              <b-nav-item :to="withBase('/reference/directives')">Directives</b-nav-item>
              <b-nav-item :to="withBase('/reference/icons')">Icons</b-nav-item>
              <b-nav-item :to="withBase('/reference/types')">Types</b-nav-item>
              <b-nav-item :to="withBase('/migration-guide')">Migrate</b-nav-item>
            </b-nav>
          </b-navbar-nav>
          <b-nav>
            <b-button
              :variant="null"
              :href="globalData.githubUrl"
              aria-label="Open Github"
              target="_blank"
              rel="noopener"
            >
              <github-icon aria-hidden />
            </b-button>
            <b-button
              :variant="null"
              :href="globalData.opencollectiveUrl"
              aria-label="Open Github"
              target="_blank"
              rel="noopener"
            >
              <opencollective-icon />
            </b-button>
            <b-button
              :variant="null"
              :href="globalData.discordUrl"
              aria-label="Open Discord Server"
              target="_blank"
              rel="noopener"
            >
              <discord-icon aria-hidden />
            </b-button>
            <client-only>
              <b-dropdown :variant="null">
                <!-- TODO there's no way to adjust these options, say if you wanted to remove the padding -->
                <template #button-content>
                  <component :is="currentIcon" :aria-label="`Toggle theme (${dark})`" />
                </template>
                <b-dropdown-item
                  v-for="el in options"
                  :key="el"
                  :active="dark === el"
                  @click="set(el)"
                >
                  <component :is="map[el]" /> {{ el }}
                </b-dropdown-item>
              </b-dropdown>
            </client-only>
          </b-nav>
        </b-navbar>
      </b-col>
    </b-row>
    <b-row v-if="page.isNotFound">
      <b-col>
        <b-container class="text-center my-auto p-5">
          <b-row>
            <b-col>
              <h1>Oh No!</h1>
            </b-col>
          </b-row>
          <b-row>
            <b-col>
              <h2>File Not Found</h2>
            </b-col>
          </b-row>
        </b-container>
      </b-col>
    </b-row>
    <b-row v-else>
      <b-col>
        <b-container>
          <b-row>
            <b-col>
              <Content />
            </b-col>
          </b-row>
        </b-container>
      </b-col>
    </b-row>
  </b-container>
</template>

<script setup lang="ts">
import {
  BButton,
  BCol,
  BContainer,
  BDropdown,
  BDropdownItem,
  BNav,
  BNavbar,
  BNavbarBrand,
  BNavbarNav,
  BNavItem,
  BRow,
  useColorMode,
} from 'bootstrap-vue-next'
import {computed, inject, toRef, readonly} from 'vue'
import GithubIcon from '~icons/bi/github'
import OpencollectiveIcon from '~icons/simple-icons/opencollective'
import DiscordIcon from '~icons/bi/discord'
import MoonStarsFill from '~icons/bi/moon-stars-fill'
import SunFill from '~icons/bi/sun-fill'
import CircleHalf from '~icons/bi/circle-half'
import {useData, withBase} from 'vitepress'
import {appInfoKey} from './keys'

// https://vitepress.dev/reference/runtime-api#usedata
const {page} = useData()

const dark = useColorMode({
  persist: true,
})

const map = {
  dark: MoonStarsFill,
  light: SunFill,
  auto: CircleHalf,
}

const options = Object.keys(map) as (keyof typeof map)[]

const currentIcon = computed(() => map[dark.value])

const set = (newValue: keyof typeof map) => (dark.value = newValue)

const globalData = inject(appInfoKey, {
  discordUrl: '',
  githubUrl: '',
  opencollectiveUrl: '',
})
</script>
