<template>
  <SfLink
    class="sf-header-navigation-item__link"
    :link="link"
  >
    {{ label }}
  </SfLink>
</template>
<script lang="ts">
import { SfLink} from '@storefront-ui/vue';
import { defineComponent } from '@nuxtjs/composition-api';

export default defineComponent({
  name: 'HeaderNavigationItem',
  components: {
    SfLink,

  },
  props: {
    label: {
      type: String,
      default: '',
    },
    link: {
      type: [String, Object],
      default: '',
    },
    hasChildren: {
      type: Boolean,
      default: false,
    },
  },
});
</script>
<style lang="scss">
.sf-header-navigation-item__link {
  display: flex;
  .sf-icon {
    display: inline-flex;
    margin: 2 var(--spacer-xs);
  }
}
.sf-header-navigation-item__item--desktop:focus{
  --header-navigation-item-border-color:gray !important
}
</style>
