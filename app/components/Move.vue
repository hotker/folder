<script setup lang="ts">
defineProps<{
  file: IFile;
  loading: boolean;
  error: string;
}>();
const parent = ref<IFile | null>(null);
const emit = defineEmits(["update"]);
const onSelect = (folder: IFile) => {
  parent.value = folder;
};
const onMove = () => {
  emit("submit", parent.value.id);
};
</script>
<template>
  <UModal
    :title="`Move ${file.name} ${file.type}`"
    :description="`Move ${file.type} to another folder`"
  >
    <template #body>
      <UAlert
        v-if="error"
        title="Error"
        :description="error"
        color="error"
        variant="soft"
        icon="lucide:message-circle-warning"
        class="mb-4"
      />

      <UFormField label="Select Destination Folder">
        <FolderPicker @select="onSelect" />
      </UFormField>
      <UAlert
        v-if="parent"
        class="mt-4"
        title="Move"
        color="neutral"
        variant="subtle"
      >
        <template #description>
          <div class="flex justify-start items-center gap-2">
            {{ file.path }}
            <Icon name="lucide:arrow-right" class="" />
            {{ parent.path + "/" + file.name }}
          </div>
        </template>
      </UAlert>
    </template>
    <template #footer>
      <UButton
        @click="onMove"
        :loading="loading"
        color="primary"
        variant="solid"
        >Move</UButton
      >
    </template>
  </UModal>
</template>
