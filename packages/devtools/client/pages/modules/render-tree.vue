<script setup lang="ts">
import { definePageMeta } from '#imports'
import { Components as VueComponents } from '@vue/devtools-applet'
import { useDevToolsState as useVueDevToolsState } from '@vue/devtools-core'
import { useClient } from '~/composables/client'
import { useOpenInEditor } from '~/composables/editor'

const { connected } = useVueDevToolsState()
const client = useClient()
const openInEditor = useOpenInEditor()

definePageMeta({
  icon: 'i-carbon-category',
  title: 'Render Tree',
  layout: 'full',
  show: () => {
    const client = useClient()
    return () => !!client.value
  },
  order: 1,
  category: 'vue-devtools',
})

function togglePanel(status: boolean) {
  if (status)
    client.value.devtools.open()
  else
    client.value.devtools.close()
}
</script>

<template>
  <div class="h-full w-full">
    <VueComponents
      v-if="connected"
      @on-inspect-component-start="togglePanel(false)"
      @on-inspect-component-end="togglePanel(true)"
      @open-in-editor="openInEditor"
    />
    <NLoading v-else>
      Connecting....
    </NLoading>
  </div>
</template>
