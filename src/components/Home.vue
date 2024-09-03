<template>
  <main class="main-content">
    <div class="welcome-section">
      <h1>Welcome to Dashboard</h1>
      <p>This is an open source project. <a href="https://github.com/aaronetwork/" target="_blank">More details.</a></p>
      <div class="stats-cards">
        <div class="card">
          <h2>Total accounts</h2>
          <p>{{ formattedAccount }}</p>
        </div>
        <div class="card">
          <h2>Current inflation</h2>
          <p>{{ inflationRate }}%</p>
        </div>
        <div class="card">
          <h2>Current supply</h2>
          <p>{{ formattedSupply }}</p>
        </div>
        <div class="card">
          <h2>Staking pool (bonded/unbonded)</h2>
          <p>{{ formattedBond }}/{{ formattedUnbond }}</p>
        </div>
      </div>
    </div>
    <div class="main-section">
      <div class="stats">
        <h3>Accounts</h3>
        <account-chart v-if="accountDataLoaded" :accountData="accountData"></account-chart>
      </div>
      <div class="stats">
        <h3>Supplies</h3>
      </div>
      <div class="stats">
        <h3>Inflation's</h3>
      </div>
      <div class="stats">
        <h3>Transactions</h3>
      </div>
    </div>
  </main>
</template>

<script>
import axios from 'axios';
import AccountChart from './AccountChart.vue';

export default {
  components: {
    AccountChart,
  },
  data() {
    return {
      apiChainUrl: import.meta.env.VITE_CHAIN_API_URL,
      apiUrl: import.meta.env.VITE_API_URL,
      denom: import.meta.env.VITE_DENOM,
      minimalDenom: import.meta.env.VITE_MINIMAL_DENOM,
      accountData: [],
      accountDataLoaded: false,
      inflationData: [],
      stakingPoolData: [],
      supplyData: [],
      account: null,
      inflation: null,
      stakingPool: null,
      supply: null,
    };
  },
  computed: {
    formattedAccount() {
      return this.account !== undefined && this.account !== null ? Number(this.account).toLocaleString() : 'Loading...';
    },
    formattedSupply() {
      return this.supply ? (this.supply / 1000000).toLocaleString() : 'Loading...';
    },
    inflationRate() {
      return this.inflation ? (parseFloat(this.inflation) * 100).toFixed(2) : 'Loading...';
    },
    formattedBond() {
      return this.stakingPool ? (this.stakingPool.bonded_tokens / 1000000).toLocaleString() : 'Loading...';
    },
    formattedUnbond() {
      return this.stakingPool ? (this.stakingPool.not_bonded_tokens / 1000000).toLocaleString() : 'Loading...';
    },
  },
  methods: {
    async fetchPool() {
      try {
        const response = await axios.get(this.apiChainUrl + 'cosmos/staking/v1beta1/pool');
        this.stakingPool = response.data.pool;
      } catch (error) {
        console.error('Error fetching staking pool:', error);
      }
    },
    async fetchAccounts() {
      try {
        const response = await axios.get(this.apiChainUrl + 'cosmos/auth/v1beta1/accounts?pagination.limit=10&pagination.count_total=true');
        this.account = response.data.pagination.total;
      } catch (error) {
        console.error('Error fetching accounts:', error);
      }
    },
    async fetchSupply() {
      try {
        const response = await axios.get(this.apiChainUrl + 'cosmos/bank/v1beta1/supply');
        const supplyData = response.data.supply.find(s => s.denom === this.minimalDenom);
        this.supply = supplyData ? supplyData.amount : null;
      } catch (error) {
        console.error('Error fetching supply:', error);
      }
    },
    async fetchInflation() {
      try {
        const response = await axios.get(this.apiChainUrl + 'cosmos/mint/v1beta1/inflation');
        this.inflation = response.data.inflation;
      } catch (error) {
        console.error('Error fetching inflation:', error);
      }
    },
    async fetchAccountData() {
      try {
        const response = await axios.get(this.apiUrl + 'account');
        this.accountData = response.data

        this.accountDataLoaded = true
      } catch (error) {
        console.error('Error fetching accounts:', error);
      }
    },
  },
  created() {
    this.fetchPool();
    this.fetchAccounts();
    this.fetchSupply();
    this.fetchInflation();
    this.fetchAccountData();
  },
};
</script>

<style scoped>

</style>
