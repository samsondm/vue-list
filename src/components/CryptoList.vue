<template>
  <Loading
    v-if="shouldListRender"
    :isLoading="isListLoading"
    :isLoadingError="isListLoadingError"
    :className="`crypto-list`"
  >
    <div class="crypto-list__title">Список криптовалют</div>
    <div class="crypto-list__head">
      <div class="crypto-list__cell crypto-list__cell_name">Имя</div>
      <div class="crypto-list__cell crypto-list__cell_type">Тип</div>
      <div class="crypto-list__cell crypto-list__cell_symbol">Знак</div>
      <div class="crypto-list__cell crypto-list__cell_mark">Пометки</div>
      <div class="crypto-list__cell crypto-list__cell_details">Детали</div>
    </div>
    <div class="crypto-list__body">
      <CryptoListCurrency
        v-for="currency of cryptoListActive"
        :key="currency.id"
        :currency="currency"
      />
    </div>
  </Loading>
</template>

<script>
import Loading from './Loading';
import CryptoListCurrency from './CryptoListCurrency';

export default {
  name: 'CryptoList',
  components: {
    Loading,
    CryptoListCurrency,
  },
  data() {
    return {
      isListLoading: false,
      isListLoadingError: false,
      shouldListRender: true,
      cryptoList: [],
      cryptoAPIUrl: 'https://api.coinpaprika.com/v1/coins',
    };
  },
  async mounted() {
    try {
      this.isListLoading = true;
      const response = await fetch(this.cryptoAPIUrl);
      if (!response.ok) throw new Error(response.statusText);
      const data = await response.json();
      if (!data || !data.length) throw new Error('Data is empty');
      this.cryptoList = data;
    } catch (error) {
      this.isListLoadingError = true;
    } finally {
      this.isListLoading = false;
    }
  },
  computed: {
    cryptoListActive() {
      return this.cryptoList.filter(crypto => crypto.is_active);
    },
  },
};
</script>

<style lang="scss">
.crypto-list {
  display: flex;
  flex-direction: column;
  max-width: 520px;
  min-width: 320px;
  margin: 10px auto 0;
  box-shadow: 0px 2px 1px -1px rgba(0, 0, 0, 0.2),
    0px 1px 1px 0px rgba(0, 0, 0, 0.14), 0px 1px 3px 0px rgba(0, 0, 0, 0.12);
  &__title {
    margin: 10px 0;
    text-align: center;
    font-size: 1.5em;
    font-weight: bold;
  }
  &__head {
    display: flex;
    position: sticky;
    top: 0;
    padding: 10px 0;
    background-color: rgba(210, 210, 210, 1);
    border-top: 1px solid rgba(0, 0, 0, 0.12);
    border-bottom: 1px solid rgba(0, 0, 0, 0.12);
  }
  &__body {
    display: flex;
    flex-direction: column;
  }
  &__cell {
    flex-grow: 1;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    &_name {
      flex-basis: 120px;
      padding-left: 5px;
    }
    &_type {
      flex-basis: 40px;
    }
    &_symbol {
      flex-basis: 50px;
    }
    &_mark {
      flex-basis: 60px;
    }
    &_details {
      flex-basis: 50px;
      text-align: center;
      > button {
        width: 24px;
        height: 24px;
      }
    }
  }
}
</style>

