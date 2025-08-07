<script setup lang="ts">
  import { ref, watch } from 'vue';
  import { useStore } from 'src/stores/store'
  import { useSettingsStore } from 'src/stores/settingsStore'
  import type { DialogInfo } from 'src/interfaces/interfaces'
  import { copyToClipboard } from 'src/utils/utils';
  const store = useStore()
  const settingsStore = useSettingsStore()
  const emit = defineEmits(['closeDialog']);

  const showDialog = ref(true);

  defineProps<{
    alertInfo: DialogInfo,
  }>()

  watch(showDialog, () => {
    if(!showDialog.value) emit('closeDialog')
  })
</script>

<template>
  <q-dialog v-model="showDialog">
    <q-card
      style="width: 500px"
      class="alertDialog"
    >
      <q-card-section class="row items-center q-pb-none">
        <div class="text-h6">
          Transaction Sent!
        </div>
        <q-space />
        <q-btn
          v-close-popup
          icon="close"
          :color="settingsStore.darkMode? 'white':'black'"
          flat
          round
          dense
        />
      </q-card-section>

      <q-card-section class="q-pt-none">
        {{ alertInfo.message }} <br><br>
        <span
          style="cursor: pointer;"
          @click="copyToClipboard(alertInfo.txid)"
        >
          Transaction ID:
          <span style="color: var(--color-grey)">{{ alertInfo.txid.slice(0, 20) + "..." + alertInfo.txid.slice(-10) }}</span>
          <img
            class="copyIcon icon"
            src="images/copyGrey.svg"
          >
        </span>
        <br><br>
        <a
          :href="store.explorerUrl + `/${alertInfo.txid}`"
          target="_blank"
        >Link blockexplorer</a>
        <span
          style="cursor: pointer;"
          @click="copyToClipboard(store.explorerUrl + `/${alertInfo.txid}`)"
        >
          <img
            class="copyIcon icon"
            :src="settingsStore.darkMode? 'images/copyGrey.svg':'images/copy.svg'"
          >
        </span>
      </q-card-section>
      <br>
    </q-card>
  </q-dialog>
</template>

<style scoped>
  body.dark .alertDialog {
    background-color: #050a14;
  }
</style>