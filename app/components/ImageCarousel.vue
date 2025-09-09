<template>
  <div class="carousel">
    <div class="main-image-container">
      <div class="main-image-inner">
        <img
          :src="images[currentIndex]"
          :alt="'Product image ' + (currentIndex + 1)"
          class="main-image"
        />
      </div>

      <div class="nav-section">
        <button class="nav-btn prev" @click="previousImage">
          <img src="/assets/icons/arrow-left.svg" alt="Previous" />
        </button>

        <button class="nav-btn next" @click="nextImage">
          <img src="/assets/icons/arrow-right.svg" alt="Next" />
        </button>
      </div>
    </div>
    <div class="thumbnail-list">
      <div
        v-for="(image, index) in images"
        :key="index"
        class="thumbnail-item"
        :class="{ active: index === currentIndex }"
        @click="selectImage(index)"
      >
        <div class="thumbnail-item-inner">
          <img :src="image" :alt="'Thumbnail ' + (index + 1)" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  images: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["update:modelValue"]);
const currentIndex = ref(0);

function nextImage() {
  currentIndex.value = (currentIndex.value + 1) % props.images.length;
}

function previousImage() {
  currentIndex.value =
    currentIndex.value === 0 ? props.images.length - 1 : currentIndex.value - 1;
}

function selectImage(index) {
  currentIndex.value = index;
}

watch(currentIndex, (newValue) => {
  emit("update:modelValue", props.images[newValue]);
});
</script>

<style scoped lang="scss">
.carousel {
  display: flex;
  flex-direction: column;
  gap: 24px;
  width: 100%;

  .main-image-container {
    position: relative;
    width: 100%;
    background: #fff;
    overflow: hidden;
    border: 1px solid #e5e5e5;
    padding: 54px;

    .main-image-inner {
      position: relative;
      width: 100%;
      padding-top: 100%;

      .main-image {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: contain;
      }
    }

    .nav-section {
      position: absolute;
      bottom: 12px;
      right: 12px;
      display: flex;
      gap: 4px;

      .nav-btn {
        width: 40px;
        height: 40px;
        background: #f5f7fa;
        border: none;
        border-radius: 50%;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        z-index: 1;
        transition: all 0.2s;

        &:hover {
          background: #fff;
        }

        &.prev {
          left: 16px;
        }

        &.next {
          right: 16px;
        }

        img {
          width: 24px;
          height: 24px;
        }
      }
    }
  }

  .thumbnail-list {
    display: flex;
    gap: 12px;
    flex-wrap: wrap;
    overflow-x: auto;

    .thumbnail-item {
      position: relative;
      overflow: hidden;
      cursor: pointer;
      background: #fff;
      transition: all 0.2s ease;
      max-width: 138px;
      width: 100%;

      &-inner {
        position: relative;
        width: 100%;
        padding-top: 100%;
      }

      &::after {
        content: "";
        position: absolute;
        inset: 0;
        border: 2px solid transparent;
        border-radius: 4px;
        transition: border-color 0.2s ease, box-shadow 0.2s ease;
      }

      &.active::after {
        border-color: #231f20;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      }

      img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        transition: transform 0.2s ease;

        &:hover {
          opacity: 0.8;
          transform: scale(1.05);
        }
      }
    }

    img {
      object-fit: none;
      cursor: pointer;
      border: 2px solid transparent;

      &.active {
        border-color: #231f20;
      }
    }
  }
}
</style>
