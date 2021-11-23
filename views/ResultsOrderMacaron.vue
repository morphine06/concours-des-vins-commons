<template>
  <div>
    <div class="row">
      <div class="col-md-5 offset-md-7">
        <div>
          Nombre de macaron or : {{ row_or.or_or | formatChiffreSimple }}
        </div>
        <div>
          Nombre de macaron Argent :
          {{ row_or.or_argent | formatChiffreSimple }}
        </div>
        <div>
          Nombre de macaron Excellence :
          {{ row_or.or_excellence | formatChiffreSimple }}
        </div>
      </div>
    </div>
    <div class="row mt-3">
      <div class="col-md-7">
        <div>
          Sous-total HT :
          {{ (row_or.or_or + row_or.or_argent + row_or.or_excellence) / 1000 }}
          lot(s) de 1000 x
          {{
            row_or.or_price_macaron
              ? $options.filters.formatPriceDivisePar100(
                  row_or.or_price_macaron
                )
              : $options.filters.formatPriceDivisePar100(
                  row_or.yp_macarons_price
                )
          }}
          = {{ row_or.or_price_ht | formatPriceDivisePar100 }} HT
        </div>
        <div>
          Frais de port :
          <span
            @click="showFraisDePort = true"
            v-if="!showFraisDePort"
            class="color-red pointer"
            >Voir les frais de port à titre indicatif</span
          >
          <span
            @click="showFraisDePort = false"
            v-if="showFraisDePort"
            class="color-red pointer"
            >{{ row_or.or_fraisport }} €</span
          >
        </div>
      </div>
      <div class="col-md-5">
        <div class="frame bg-gray rounded-0">
          <div class="d-flex align-items-center">
            <div>HT :</div>
            <h5 class="color-red ms-2">
              {{ row_or.or_price_ht | formatPriceDivisePar100 }} 
            </h5>
          </div>
          <div class="d-flex align-items-center">
            <div>TVA :</div>
            <h5 class="color-red ms-2">
              {{ row_or.or_price_tva | formatPriceDivisePar100 }} 
            </h5>
          </div>
          <div class="d-flex align-items-center">
            <div>Total TTC :</div>
            <h2 class="color-red ms-2">
              {{ row_or.or_price_ttc | formatPriceDivisePar100 }} TTC
            </h2>
          </div>
          <div class="color-red">
            (Frais de port en sus indiqués sur la facture à venir)
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "resultsOrderMacaron",
  components: {},
  props: {
    row_or: Object,
    from: String
  },
  data() {
    return {
      showFraisDePort: false
    };
  },
  watch: {
    row_or() {
      this.calculTotal();
    }
  },
  async mounted() {},
  methods: {
    calculTotal() {
      let total = 0;
      let tt =
        this.row_or.or_or + this.row_or.or_argent + this.row_or.or_excellence;
      if (this.row_or.or_price_macaron)
        total = (tt / 1000) * this.row_or.or_price_macaron;
      else total = (tt / 1000) * this.row_or.yp_macarons_price;

      this.$emit("changetotalOrder", total);
    }
  }
};
</script>

<style lang="scss" scoped></style>
