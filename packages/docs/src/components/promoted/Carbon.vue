<template>
  <v-responsive class="align-center">
    <buysellads/>
    <promotion-card />
  </v-responsive>
  <br />
</template>

<script setup lang="ts">
  // Components
  import PromotionCard from '@/components/promotions/PromotionCard.vue'
  import PromotedBase from './Base.vue'
  import Buysellads from './Buysellads.vue'
  // Composables
  import { useTheme } from 'vuetify'
  // Utilities
  import { computed, onBeforeUnmount, onMounted, shallowRef } from 'vue'

  const theme = useTheme()

  const isDark = computed(() => theme.current.value.dark)
</script>