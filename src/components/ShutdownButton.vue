<template>
  <q-page-sticky v-if="configurationStore.configuration.uisettings.show_shutdown_button" position="bottom-right" class="q-ma-lg">
    <q-btn round color="negative" icon="sym_o_power_settings_new" :aria-label="$t('BTN_LABEL_SHUTDOWN_HOST')" @click="confirmShutdown = true">
      <q-tooltip>{{ $t('BTN_LABEL_SHUTDOWN_HOST') }}</q-tooltip>
    </q-btn>

    <q-dialog v-model="confirmShutdown">
      <q-card class="q-pa-sm" style="min-width: 350px" flat>
        <q-card-section class="row items-center" style="flex-wrap: nowrap">
          <q-avatar icon="sym_o_power_settings_new" color="negative" text-color="white" />
          <span class="q-ml-sm">{{ $t('MSG_CONFIRM_SHUTDOWN') }}</span>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn no-caps v-close-popup flat :label="$t('BTN_LABEL_CANCEL')" />
          <q-btn no-caps v-close-popup :label="$t('BTN_LABEL_SHUTDOWN')" color="negative" @click="remoteProcedureCall('/api/system/host/shutdown')" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page-sticky>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useConfigurationStore } from '@/stores/configuration-store'
import { remoteProcedureCall } from '@/util/fetch_api'

const configurationStore = useConfigurationStore()
const confirmShutdown = ref(false)
</script>
