<template>
  <SwPluginSlot
    name="product-card"
    :slot-context="product"
    style="display: contents"
  >
    <SfProductCard
      :title="getName"
      :image="getImageUrl"
      :special-price="formatPrice(getSpecialPrice)"
      :regular-price="formatPrice(getRegularPrice)"
      :max-rating="5"
      :score-rating="getProductRating"
      :image-width="700"
      :image-height="1000"
      :link="getRouterLink"
      class="sw-product-card"
      :show-add-to-cart-button="true"
      :is-added-to-cart="isInCart"
      :is-on-wishlist="isInWishlist"
      @click:add-to-cart="addToCart"
      @click:wishlist="toggleWishlistItem"
    >
    </SfProductCard>
  </SwPluginSlot>
</template>

<script>
import { SfProductCard } from "@storefront-ui/vue"
import { useAddToCart, useWishlist } from "@shopware-pwa/composables"
import {
  getProductThumbnailUrl,
  getProductTierPrices,
  getProductUrl,
  getProductName,
  getProductCalculatedPrice,
  getProductCalculatedListingPrice,
  getProductPriceDiscount,
} from "@shopware-pwa/helpers"
import getResizedImage from "@/helpers/images/getResizedImage.js"
import { toRefs } from "@vue/composition-api"
import { usePriceFilter } from "@/logic/usePriceFilter.js"
import SwPluginSlot from "sw-plugins/SwPluginSlot.vue"

export default {
  components: {
    SfProductCard,
    SwPluginSlot,
  },
  setup(props) {
    const { product } = toRefs(props)
    const { addToCart, quantity, getStock, isInCart } = useAddToCart({
      product,
    })
    const { addToWishlist, removeFromWishlist, isInWishlist } = useWishlist({
      product,
    })
    return {
      quantity,
      addToCart,
      getStock,
      isInCart,
      toggleWishlistItem: () =>
        isInWishlist.value
          ? removeFromWishlist(product.value.id)
          : addToWishlist(),
      isInWishlist,
      formatPrice: usePriceFilter(),
    }
  },
  props: {
    product: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {}
  },
  computed: {
    getName() {
      return getProductName({ product: this.product })
    },
    getProductRating() {
      return this.product && this.product.ratingAverage
    },
    // should be replaced with prettyUrl attribute when pretty urls are included in product entity
    getRouterLink() {
      return this.$routing.getUrl(getProductUrl(this.product))
    },
    getRegularPrice() {
      return (
        (this.tierPrices.length &&
          this.tierPrices[0] &&
          this.tierPrices[0].unitPrice) ||
        getProductCalculatedListingPrice(this.product)
      )
    },
    getSpecialPrice() {
      return this.tierPrices.length
        ? undefined
        : getProductPriceDiscount(this.product) &&
            getProductCalculatedPrice(this.product)
    },
    tierPrices() {
      return getProductTierPrices(this.product)
    },
    getImageUrl() {
      return getResizedImage(getProductThumbnailUrl(this.product), {
        width: 280,
        height: 400,
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.sw-product-card {
  width: 100%;
  padding: 0.3rem;

  ::v-deep .sf-product-card__image-wrapper {
    min-height: 50px;
  }
}
</style>
