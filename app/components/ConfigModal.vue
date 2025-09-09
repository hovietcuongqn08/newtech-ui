<template>
  <div class="modal-overlay" @click="$emit('close')">
    <div class="modal-content" @click.stop>
      <div class="modal-header">
        <h3>{{ config.displayName.toUpperCase() }}</h3>
        <button class="close-btn" @click="$emit('close')">×</button>
      </div>
      <div class="options-list">
        <label
          v-for="option in config.options"
          :key="option.uid"
          class="option-item"
        >
          <span class="option-name">{{ option.displayName }}</span>

          <input
            type="radio"
            name="modal-options"
            :value="option.uid"
            :checked="isSelected(option)"
            @change="$emit('select', option)"
          />
          <span class="custom-radio"></span>
        </label>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  config: {
    type: Object,
    required: true,
  },
  selected: {
    type: Object,
    required: true,
  },
});

function isSelected(option) {
  return props.selected?.uid === option.uid;
}
</script>

<style scoped lang="scss">
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.3);
  z-index: 1000;
  display: flex;
  justify-content: flex-end;
}

.modal-content {
  background: #fff;
  max-width: 512px;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  box-shadow: -2px 0 8px rgba(0, 0, 0, 0.1);
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0);
  }
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 24px;
  border-bottom: 1px solid #eee;

  h3 {
    margin: 0;
    font-size: 16px;
    font-weight: 400;
    letter-spacing: 0.05em;
    text-transform: uppercase;
  }
}

.close-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #231f20;
}

.options-list {
  flex: 1;
  overflow-y: auto;
  padding: 0;
}

.option-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
  cursor: pointer;
  border-bottom: 1px solid #f0f0f0;

  &:hover {
    background: #fafafa;
  }

  input[type="radio"] {
    display: none;
  }

  .custom-radio {
    width: 20px;
    height: 20px;
    border: 2px solid #ccc;
    border-radius: 50%;
    transition: all 0.2s ease;
    background-size: 12px 12px;
    background-position: center;
    background-repeat: no-repeat;
  }

  input[type="radio"]:checked + .custom-radio {
    border: none;
    background-color: #231f20;
    background-image: url("@/assets/icons/check.svg");
  }
}

.option-name {
  font-size: 14px;
  color: #231f20;
  font-weight: 600;
  line-height: 1.5;
}
</style>
