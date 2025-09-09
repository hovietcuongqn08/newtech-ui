<template>
  <table class="config-table">
    <tbody>
      <tr
        v-for="option in optionTypes"
        :key="option.uid || option.displayName"
        class="config-row"
        @click="$emit('open-modal', option)"
      >
        <td class="config-label">
          {{ option.displayName }}
        </td>

        <td class="config-value">
          {{ selectedOptions[option.displayName]?.displayName || "-" }}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
const props = defineProps({
  optionTypes: {
    type: Array,
    required: true,
  },
  selectedOptions: {
    type: Object,
    required: true,
  },
});

defineEmits(["open-modal"]);
</script>

<style scoped lang="scss">
.config-table {
  width: 100%;
  border-collapse: collapse;

  .config-row {
    background-color: #fff;
    cursor: pointer;
    transition: border-color 0.2s ease;

    td {
      padding: 16px;
      border: 1px solid #e5e5e5;
      min-height: 56px;
    }

    td:first-child {
      border-right: none;
      width: 180px;
      font-weight: 600;
      color: #231f20;
    }

    td:last-child {
      border-left: none;
      color: #231f20;
    }

    &:hover {
      background-color: #fafafa;
    }
  }
}
</style>
