<template>
  <b-card-head-foot class="card-header" v-bind="props">
    <slot />
  </b-card-head-foot>
</template>

<script setup lang="ts">
import type {ColorVariant, TextColorVariant} from '../../types'
import BCardHeadFoot from './BCardHeadFoot.vue'

interface BCardHeaderProps {
  text?: string
  bgVariant?: ColorVariant | null
  borderVariant?: ColorVariant | null
  html?: string
  tag?: string
  textVariant?: TextColorVariant | null
}

const props = withDefaults(defineProps<BCardHeaderProps>(), {
  tag: 'div',
  text: undefined,
  bgVariant: undefined,
  borderVariant: undefined,
  html: undefined,
  textVariant: undefined,
})
</script>
