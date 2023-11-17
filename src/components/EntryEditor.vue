<script lang="ts" setup>
import EmojiField from '@/components/EmojiField.vue'
import ArrowCircleRight from '@/assets/icons/arrow-circle-right.svg'
import { ref, computed, onMounted, inject } from 'vue'
import type Emoji from '@/types/Emoji'
import type Entry from '@/types/Entry'
import { userInjectionKey } from "@/injectionKeys";

// data
const body = ref('')
const emoji = ref<Emoji | null>(null)
const charCount = computed(() => body.value.length)
const MAX_CHARS = 280
const injectedUser = inject(userInjectionKey);

// template ref
const textarea = ref<HTMLTextAreaElement | null>(null)
onMounted(() => textarea.value?.focus())

// events
const emit = defineEmits<{ (e: '@create', entry: Entry): void }>()
// const emit = defineEmits<{ '@create': [entry: Entry] }>()


//metods
const handleTextInput = (e: KeyboardEvent) => {
  const textarea = e.target as HTMLTextAreaElement
  if (textarea.value.length <= MAX_CHARS) {
    body.value = textarea.value
    return
  }
  body.value = textarea.value = textarea.value.substring(0, MAX_CHARS)
}

const handleSubmit = () => {
  emit('@create', { body: body.value, emoji: emoji.value, createdAt: new Date(), userId: 1, id: Math.random() })

  // reset form
  body.value = ''
  emoji.value = null
}
</script>
<template>
  <form class="entry-form" @submit.prevent="handleSubmit">
    <textarea ref="textarea" :value="body" @keyup="handleTextInput"
      :placeholder="`New Journal Entry for ${injectedUser?.username}`"></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ MAX_CHARS }}</span>
      <button>Remember
        <ArrowCircleRight width="20" />
      </button>
    </div>
  </form>
</template>
