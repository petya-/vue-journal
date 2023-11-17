<script setup lang="ts">
import TheHeader from '@/components/TheHeader.vue'
import EntryEditor from './components/EntryEditor.vue'
import EntryCard from '@/components/EntryCard.vue'
import { reactive, provide } from 'vue'
import type User from '@/types/User'
import type Entry from '@/types/Entry'
import { userInjectionKey } from '@/injectionKeys'

// data
const entries = reactive<Entry[]>([])
const user: User = reactive({
  id: 1,
  username: 'petya-',
  settings: [],
})

// methods
const handleCreateEntry = (entry: Entry) => {
  entries.unshift(entry)
}

provide(userInjectionKey, user)

</script>

<template>
  <main class="container m-auto p-10">
    <TheHeader />
    <EntryEditor @@create="handleCreateEntry" />
    <ul>
      <li v-for="entry in entries" :key="entry.id">
        <EntryCard :entry="entry" />
      </li>
    </ul>
  </main>
</template>
