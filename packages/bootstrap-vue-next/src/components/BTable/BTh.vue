<template>
  <th
    :scope="scope"
    :class="computedClasses"
    :colspan="colspan"
    :rowspan="rowspan"
    :data-label="stackedHeading"
  >
    <div v-if="stackedHeading !== undefined">
      <slot />
    </div>
    <slot v-else />
  </th>
</template>

<script setup lang="ts">
import type {Booleanish, ColorVariant} from '../../types'
import {useBooleanish} from '../../composables'
import {computed, toRef} from 'vue'

interface BThProps {
  colspan?: string | number
  rowspan?: string | number
  stackedHeading?: string
  stickyColumn?: Booleanish
  variant?: ColorVariant | null
}

const props = withDefaults(defineProps<BThProps>(), {
  colspan: undefined,
  rowspan: undefined,
  variant: null,
  stackedHeading: undefined,
  stickyColumn: false,
})

const stickyColumnBoolean = useBooleanish(toRef(props, 'stickyColumn'))

const computedClasses = computed(() => ({
  [`table-${props.variant}`]: props.variant !== null,
  'b-table-sticky-column': stickyColumnBoolean.value,
  'table-b-table-default': stickyColumnBoolean.value && props.variant === null,
}))

const scope = computed(() => (props.colspan ? 'colspan' : props.rowspan ? 'rowspan' : 'col'))
</script>
