<template>
  <main class="flex flex-col justify-center h-full mx-auto max-w-600px">
    <section class="flex flex-col items-center leading-loose text-center gap-4">
      <div class="text-3xl">
        <span class="i-twemoji-christmas-tree"></span>
        {{ $t('happyHolidays') }}
        <span class="i-twemoji-world-map"></span>
      </div>
      <!-- Dates - Check out locales/en.json for the key -->
      <div class="text-3xl">
        <i18n-t keypath="christmasIsComing" :plural="day">
          <template #date>
            {{ d(christmasDate, 'long') }}
          </template>
          <template #time>
            <span class="text-green-700">{{ t('day', { count: day }) }}</span>
          </template>
        </i18n-t>
      </div>
      <!-- Controls - I give you an .icon-button class if you want to use it -->
      <div class="flex justify-between gap-44 text-3xl">
        <button class="icon-button" @click="locale = nextLocale">
          <span class="i-carbon-language"></span>
        </button>
        <div>
          <span class="mr-2" :class="flagIcons[locale]"></span>
          <span>{{ $t('language') }}</span>
        </div>
      </div>
      <!-- Flags - the current locale -->
    </section>
  </main>
</template>

<script setup>
import { computed } from 'vue';
import { useI18n } from 'vue-i18n'

const {
  locale,
  availableLocales,
  t,
  d,
} = useI18n();
const flagIcons = {
  'en': 'i-twemoji-flag-united-states',
  'de': 'i-twemoji-flag-germany',
  'ja-JP': 'i-twemoji-flag-japan',
  'km-KH': 'i-twemoji-flag-cambodia',
}
const nextLocale = computed(() => {
  const index = availableLocales.findIndex((i) => i === locale.value);
  return index === availableLocales.length - 1 ? availableLocales[0] : availableLocales[index + 1];
})

// See the README about tricky timezone issues!
// I figured since this is i18n-friendly, we'd wanna
// make sure the timezones were right :-)
const christmasDate = new Date('2022/12/25')
const today = new Date();
const day = christmasDate.getDate() - today.getDate();
</script>

<style scoped>
.icon-button {
  @apply text-xl
    w-32px
    h-32px
    rounded-full
    border-1
    border-transparent
    bg-transparent
    cursor-pointer
    duration-300
    hover:ring-2
    hover:border-green-500
    hover:ring-green-500
    hover:ring-opacity-40
    hover:text-green-600;
}
</style>
