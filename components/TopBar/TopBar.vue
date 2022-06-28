<template>

</template>

<script lang="ts">
import { defineComponent } from '@nuxtjs/composition-api';


export default defineComponent({
  components: {
    CurrencySelector: () => import('../CurrencySelector.vue'),
    StoreSwitcher: () => import('../StoreSwitcher.vue'),

  },
});

</script>
<style lang="scss" scoped>
.topbar {
  position: relative;
  z-index: 2;
  &__button {
    margin: 0 0 0 var(--spacer-xs);
  }
}

::v-deep {
  .sf-top-bar__container {
    justify-content: space-between;
    & > * {
      width: calc(100% / 3);
      justify-content: center;
    }
    & > :first-child {
      justify-content: flex-start;
    }
    & > :last-child {
      justify-content: flex-end;
    }
  }
}
</style>
