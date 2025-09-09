<template>
  <Transition name="toast">
    <div class="toast" v-if="show">
      <img src="@/assets/icons/check.svg" alt="Success" class="check-icon" />
      <span>{{ message }}</span>
    </div>
  </Transition>
</template>

<script setup>
defineProps({
  message: {
    type: String,
    required: true,
  },
  show: {
    type: Boolean,
    default: true,
  },
});
</script>

<style scoped>
.toast {
  position: fixed;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  background: #231f20;
  color: #fff;
  padding: 16px 24px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  z-index: 1000;
}

.check-icon {
  width: 20px;
  height: 20px;
}

.toast-enter-active,
.toast-leave-active {
  transition: all 0.3s ease;
}

.toast-enter-from,
.toast-leave-to {
  opacity: 0;
  transform: translate(-50%, 20px);
}
</style>
