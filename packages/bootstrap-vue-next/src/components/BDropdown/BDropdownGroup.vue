<template>
  <li role="presentation">
    <component
      :is="headerTag"
      :id="headerId"
      class="dropdown-header"
      :class="computedClasses"
      :role="headerRole"
    >
      <slot name="header">
        {{ header }}
      </slot>
    </component>
    <ul
      :id="id"
      role="group"
      class="list-unstyled"
      v-bind="$attrs"
      :aria-describedby="ariaDescribedby || headerId"
    >
      <slot />
    </ul>
  </li>
</template>

<script setup lang="ts">
import type {ClassValue, ColorVariant} from '../../types'
import {computed} from 'vue'

defineOptions({
  inheritAttrs: false,
})

interface BDropdownGroupProps {
  id?: string
  ariaDescribedby?: string
  header?: string
  headerClass?: ClassValue
  headerTag?: string
  headerVariant?: ColorVariant | null
}

const props = withDefaults(defineProps<BDropdownGroupProps>(), {
  headerTag: 'header',
  id: undefined,
  ariaDescribedby: undefined,
  header: undefined,
  headerClass: undefined,
  headerVariant: null,
})

const headerId = computed<string | undefined>(() =>
  props.id ? `${props.id}_group_dd_header` : undefined
)

const headerRole = computed<'heading' | undefined>(() =>
  props.headerTag === 'header' ? undefined : 'heading'
)

const computedClasses = computed(() => [
  props.headerClass,
  {
    [`text-${props.headerVariant}`]: props.headerVariant !== null,
  },
])
</script>
