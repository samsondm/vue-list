<template>
  <div class="crypto-list__currency">
    <div class="crypto-list__currency__general">
      <div class="crypto-list__cell crypto-list__cell_name">{{currency.name}}</div>
      <div class="crypto-list__cell crypto-list__cell_type">{{currency.type}}</div>
      <div class="crypto-list__cell crypto-list__cell_symbol">{{currency.symbol}}</div>
      <div class="crypto-list__cell crypto-list__cell_mark">{{currency.is_new ? 'Новинка!' : ''}}</div>
      <div class="crypto-list__cell crypto-list__cell_details">
        <button type="button" @click="handleClickDetails">{{shouldDetailsRender ? '-' : '+'}}</button>
      </div>
    </div>
    <Loading
      v-if="shouldDetailsRender"
      :isLoading="isDetailsLoading"
      :isLoadingError="isDetailsLoadingError"
      :className="`crypto-list__currency__details`"
    >
      <CryptoListCurrencyDetails :details="details"/>
    </Loading>
  </div>
</template>

<script>
import CryptoListCurrencyDetails from './CryptoListCurrencyDetails';
import Loading from './Loading';

export default {
  name: 'CryptoListRow',
  components: {
    CryptoListCurrencyDetails,
    Loading,
  },
  props: {
    currency: {
      id: String,
      name: String,
      symbol: String,
      is_new: Boolean,
    },
  },
  data() {
    return {
      isDetailsLoading: false,
      isDetailsLoadingError: false,
      shouldDetailsRender: false,
      details: null,
      cryptoAPIUrl: 'https://api.coinpaprika.com/v1/coins/',
    };
  },
  methods: {
    async handleClickDetails() {
      if (this.shouldDetailsRender) {
        this.shouldDetailsRender = false;
        return;
      }
      try {
        this.shouldDetailsRender = true;
        this.isDetailsLoading = true;
        const response = await fetch(this.cryptoAPIUrl + this.currency.id);
        if (!response.ok) throw new Error(response.statusText);
        const data = await response.json();
        if (!data) throw new Error('Data is empty');
        this.details = data;
      } catch (error) {
        this.isDetailsLoadingError = true;
      } finally {
        this.isDetailsLoading = false;
      }
    },
  },
};
</script>

<style lang="scss">
.crypto-list__currency {
  padding: 5px 0;
  &:not(:last-child) {
    border-bottom: 1px solid rgba(0, 0, 0, 0.12);
  }
  &__general {
    height: 30px;
    line-height: 30px;
    display: flex;
  }
  &__details {
    margin: 5px 10px;
  }
}
</style>

