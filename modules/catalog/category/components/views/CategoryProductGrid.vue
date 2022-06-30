<template>
  <transition-group
    appear
    class="grid-layout"
    name="slide"
    tag="div"
  >
    <template v-if="loading">
      <div
        v-for="n in 4*3"
        :key="n"
        class="sf-product-card card"
        data-testid="skeleton"
      >
        <SkeletonLoader :height="`${imageSize.height}px`" />
        <SkeletonLoader />
        <SkeletonLoader />
      </div>
    </template>
    <template v-else>
      <SfProductCard
        v-for="product in listProductsInStock"
        :key="product.uid"
        v-bind="product.commonProps"
        class="card"
        data-testid="product-card"
        :image-height="imageSize.height"
        :image-width="imageSize.width"
        show-add-to-cart-button
        @click:wishlist="$emit('click:wishlist', product)"
        @click:add-to-cart="$emit('click:add-to-cart', { product, quantity: 1 })"
      >
        <template #price>
          <CategoryProductPrice :product="product" />
        </template>
      </SfProductCard>
      

      <SfProductCard
        v-for="product in listProductsOutOfStock"
        :key="product.uid"
        v-bind="product.commonProps"
        class="card out"
        data-testid="product-card"
        :image-height="imageSize.height"
        :image-width="imageSize.width"
        show-add-to-cart-button
        :showAddToCartButton="false"
        @click:wishlist="$emit('click:wishlist', product)"
        @click:add-to-cart="$emit('click:add-to-cart', { product, quantity: 1 })"
      >
        <template #price>
          <CategoryProductPrice :product="product" />
        </template>
      </SfProductCard>
    </template>
  </transition-group>


</template>


<script lang="ts">
import { defineComponent, PropType, toRefs } from '@nuxtjs/composition-api';
import { SfProductCard } from '@storefront-ui/vue';
import { useImage } from '~/composables';
import type { Product } from '~/modules/catalog/product/types';

import SkeletonLoader from '~/components/SkeletonLoader/index.vue';
import { useProductsWithCommonProductCardProps } from './useProductsWithCommonCardProps';
import CategoryProductPrice from '~/modules/catalog/category/components/views/CategoryProductPrice.vue';

export default defineComponent({
  components: {
    CategoryProductPrice,
    SfProductCard,
    SkeletonLoader,
  },
  props: {
    products: {
      type: Array as PropType<Product[]>,
      required: true,
    },
    pricesLoaded: Boolean,
    loading: Boolean,
  },
  emits: ['click:wishlist', 'click:add-to-cart'],
  setup(props) {
    const { imageSizes: { productCard: imageSize } } = useImage();
    const { products } = toRefs(props);
    const { productsWithCommonProductCardProps } = useProductsWithCommonProductCardProps(products);
    return {
      imageSize,
      productsWithCommonProductCardProps,
    };
  },
  computed:{
    listProductsInStock(){
      return this.productsWithCommonProductCardProps.filter(el => el.stock_status=='IN_STOCK')
    },
    listProductsOutOfStock(){
      return this.productsWithCommonProductCardProps.filter(el => el.stock_status=='OUT_OF_STOCK')
    }
  }
});
</script>

<style lang="scss" scoped>
@import "./transition.scss";
.grid-layout {
  display: flex;
  flex-wrap: wrap;
  align-content: flex-start;
  justify-content: center;

  @include for-desktop {
    justify-content: flex-start;
    margin: var(--spacer-sm) 0 0 var(--spacer-sm);
  }
}
.card {
  --product-card-title-margin: var(--spacer-base) 0 0 0;
  --product-card-title-font-weight: var(--font-weight--medium);
  --product-card-title-margin: var(--spacer-xs) 0 0 0;
  flex: 1 1 50%;

  @include for-desktop {
    flex: 1 1 23%;
    --product-card-title-font-weight: var(--font-weight--normal);
    --product-card-add-button-bottom: var(--spacer-base);
    --product-card-title-margin: var(--spacer-sm) 0 0 0;
    max-width: 25%;
  }
}

.sf-circle-icon{
  --button-background: black !important;
  --button-box-shadow: 0 0 0 0.3125rem gray !important;
}

.out{
  opacity: 0.3;
}
</style>
