<template>
  <component :is="tag" :id="id" :class="computedClasses">
    <slot>
      {{ text }}
    </slot>
  </component>
</template>

<script setup lang="ts">
import {computed, toRef} from 'vue'
import type {Booleanish, TextColorVariant} from '../../types'
import {useBooleanish} from '../../composables'

interface BFormTextProps {
  id?: string
  inline?: Booleanish
  tag?: string
  text?: string
  textVariant?: TextColorVariant | null
}

const props = withDefaults(defineProps<BFormTextProps>(), {
  inline: false,
  id: undefined,
  text: undefined,
  tag: 'small',
  textVariant: 'muted',
})

const inlineBoolean = useBooleanish(toRef(props, 'inline'))

const computedClasses = computed(() => ({
  [`text-${props.textVariant}`]: props.textVariant !== null,
  'form-text': !inlineBoolean.value,
}))
</script>
