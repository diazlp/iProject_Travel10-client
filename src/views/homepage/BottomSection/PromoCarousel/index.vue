<script>
import { mapWritableState, mapActions } from "pinia";
import { usePackageStore } from "../../../../stores/package";
import Flicking from "@egjs/vue3-flicking";
import PromoCard from "./PromoCard/index.vue";
import { useToast } from "vue-toastification";

export default {
  setup() {
    const toast = useToast();
    return { toast };
  },

  components: {
    Flicking: Flicking,
    PromoCard,
  },

  computed: {
    ...mapWritableState(usePackageStore, ["promoPackageList"]),
  },

  methods: {
    ...mapActions(usePackageStore, ["renderPromoPackage"]),

    async renderPromoPackageOnCreate() {
      try {
        const response = await this.renderPromoPackage();

        this.promoPackageList = response.data;
      } catch (err) {
        this.toast.error(err.response?.data?.message);
      }
    },
  },

  async created() {
    await this.renderPromoPackageOnCreate();
  },
};
</script>

<template>
  <div>
    <h3 class="text-heading-3 text-white font-label font-bold pt-14 mb-4">
      Promo Liburan
    </h3>
    <Flicking
      v-bind:options="{
        moveType: 'freeScroll',
        bound: true,
        align: 'center',
      }"
      class="py-5 flicking-panel"
    >
      <PromoCard
        v-for="promoPackage in promoPackageList"
        v-bind:key="promoPackage?.id"
        v-bind:promoPackage="promoPackage"
        class="mx-5"
      />
    </Flicking>
  </div>
</template>
