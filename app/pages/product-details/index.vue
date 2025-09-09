<template>
  <div class="product-details-container">
    <div class="product-main">
      <ImageCarousel
        v-model:currentIndex="currentImageIndex"
        :images="[
          '/images/image-1.png',
          '/images/image-2.png',
          '/images/image-3.png',
          '/images/image-4.png',
        ]"
        :alt="product.name"
      />
    </div>

    <div class="product-info">
      <h1 class="product-name">{{ product.name }}</h1>
      <p class="product-description">{{ product.description }}</p>

      <div class="options-section">
        <h2>CHOOSE OPTIONS</h2>

        <ConfigList
          :optionTypes="optionTypes"
          :selectedOptions="selectedOptions"
          @open-modal="openConfigModal"
        />

        <div class="price-section">
          <span class="total-price">{{ formatPrice(totalPrice) }}</span>
        </div>

        <div class="actions-section">
          <span class="quantity-label">Quantity</span>

          <div class="actions-inner">
            <QuantityInput
              v-model="quantity"
              @increase="increaseQuantity"
              @decrease="decreaseQuantity"
            />

            <button class="add-to-cart-btn" @click="addToCart">
              <img
                src="@/assets/icons/basket-add.svg"
                alt="Cart"
                class="cart-icon"
              />
              ADD TO CART
            </button>
          </div>
        </div>
      </div>

      <ConfigModal
        v-if="activeConfig"
        :config="activeConfig"
        :selected="selectedOptions[activeConfig?.displayName]"
        @select="selectOption"
        @close="closeConfigModal"
      />

      <Toast
        v-if="showToast"
        message="The product has been added to your cart"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import productData from "@/mocks/product-details-mockup.json";
import ConfigList from "@/components/ConfigList.vue";
import ConfigModal from "@/components/ConfigModal.vue";
import Toast from "@/components/Toast.vue";
import QuantityInput from "@/components/QuantityInput.vue";
import ImageCarousel from "@/components/ImageCarousel.vue";

const product = ref(productData.product);
const optionTypes = ref(productData.optionTypes);
const currentImageIndex = ref(0);
const selectedOptions = ref({});
const quantity = ref(1);
const activeConfig = ref(null);
const showToast = ref(false);

onMounted(() => {
  optionTypes.value.forEach((config) => {
    selectedOptions.value[config.displayName] = config.options[0];
  });
});

const totalPrice = computed(() => {
  const optionsPrice = Object.values(selectedOptions.value).reduce(
    (sum, option) => sum + option.price,
    0
  );
  return optionsPrice * quantity.value;
});

function formatPrice(cents) {
  return (cents / 100).toLocaleString("en-US", {
    style: "currency",
    currency: "USD",
  });
}

function openConfigModal(config) {
  activeConfig.value = config;
}

function closeConfigModal() {
  activeConfig.value = null;
}

function selectOption(option) {
  selectedOptions.value[activeConfig.value.displayName] = option;
  closeConfigModal();
}

function decreaseQuantity() {
  if (quantity.value > 1) {
    quantity.value--;
  }
}

function increaseQuantity() {
  quantity.value++;
}

function addToCart() {
  showToast.value = true;
  setTimeout(() => {
    showToast.value = false;
  }, 3000);
}
</script>

<style scoped lang="scss">
.product-details-container {
  display: flex;
  gap: 102px;
  max-width: 1200px;
  margin: 120px auto;

  .product-main {
    flex: 1;
    min-width: 0;
    max-width: 588px;
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 24px;
  }

  .product-info {
    max-width: 510px;
    width: 100%;
  }

  .product-name {
    font-size: 3rem;
    font-weight: 400;
    line-height: 1.2;
    margin: 0 0 16px;
    color: #231f20;

    @media (max-width: 768px) {
      font-size: 24px;
    }

    @media (max-width: 480px) {
      font-size: 20px;
    }
  }

  .product-description {
    font: 400 16px/1.5 sans-serif;
    margin: 24px 0 0;
    color: #231f20;

    @media (max-width: 480px) {
      font-size: 14px;
    }
  }

  .options-section {
    margin-top: 48px;
    display: flex;
    flex-direction: column;
    gap: 24px;

    h2 {
      margin: 0;
      font: 400 20px/1.2 sans-serif;
      letter-spacing: 0.08em;
      color: #231f20;
      text-transform: uppercase;
      display: flex;
      align-items: center;

      &::after {
        content: "";
        flex: 1;
        height: 1px;
        background: #e5e5e5;
        margin-left: 24px;
      }

      @media (max-width: 480px) {
        font-size: 16px;
      }
    }

    @media (max-width: 768px) {
      margin-top: 32px;
      gap: 16px;
    }
  }

  .divider {
    border: none;
    border-top: 1px solid #eee;
    margin: 24px 0;
  }

  .total-price {
    font-weight: 600;
    font-size: 28px;
    line-height: 1.25;
    margin: 0;
    color: #231f20;

    @media (max-width: 480px) {
      font-size: 24px;
    }
  }

  .actions-inner {
    display: flex;
    gap: 12px;
    align-items: center;
    margin-top: 6px;

    @media (max-width: 768px) {
      flex-direction: column;
      gap: 16px;
    }
  }

  .add-to-cart-btn {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    background: #231f20;
    color: #fff;
    border: none;
    height: 48px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.2s;
    font-size: 14px;
    letter-spacing: 0.08em;

    &:hover {
      background: #231f20;
    }

    .cart-icon {
      width: 16px;
      height: 16px;
    }

    @media (max-width: 768px) {
      width: 100%;
    }
  }

  @media (max-width: 1280px) {
    gap: 48px;
    padding: 0 24px;
  }

  @media (max-width: 1024px) {
    gap: 32px;

    .product-main {
      max-width: 50%;
    }
  }

  @media (max-width: 768px) {
    flex-direction: column;
    gap: 32px;
    padding: 0 16px;

    .product-main {
      max-width: 100%;
    }

    .product-info {
      max-width: none;
    }
  }

  @media (max-width: 480px) {
    gap: 24px;
    padding: 0 12px;
  }
}
</style>
