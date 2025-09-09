<template>
  <div class="quantity-input">
    <button class="quantity-btn" @click="$emit('decrease')">
      <img src="@/assets/icons/minus.svg" alt="Decrease" />
    </button>

    <input
      type="number"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.valueAsNumber)"
      min="1"
      class="quantity-input"
    />

    <button class="quantity-btn" @click="$emit('increase')">
      <img src="@/assets/icons/plus.svg" alt="Increase" />
    </button>
  </div>
</template>

<script setup>
defineProps({
  modelValue: {
    type: Number,
    required: true,
  },
});
defineEmits(["update:modelValue", "increase", "decrease"]);
</script>

<style scoped lang="scss">
.quantity-input {
  display: inline-flex;
  align-items: center;
  border: 1px solid #e5e5e5;
  border-radius: 4px;
  overflow: hidden;
  height: 48px;

  .quantity-btn {
    background: none;
    border: none;
    width: 48px;
    height: 48px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;

    &:hover {
      background: #f5f5f5;
    }

    img {
      width: 16px;
      height: 16px;
    }
  }

  .quantity-input {
    width: 48px;
    text-align: center;
    border: none;
    font-size: 16px;
    height: 100%;
    appearance: textfield;

    &::-webkit-outer-spin-button,
    &::-webkit-inner-spin-button {
      -webkit-appearance: none;
      margin: 0;
    }
  }

  @media (max-width: 480px) {
    width: 100%;

    .quantity-btn {
      flex: 1;
    }
    .quantity-input {
      flex: 2;
    }
  }
}
</style>
