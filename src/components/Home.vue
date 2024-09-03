<template>
  <main class="main-content">
    <div class="welcome-section">
      <h1>Welcome to Dashboard</h1>
      <p>This is the dashboard for <a href="https://aaronetwork.xyz/" target="_blank">Aaron Network</a>. The dashboard provides historical data of the network.</p>
      <div class="stats-cards">
        <div class="card">
          <h2>Total accounts</h2>
          <p style="font-size: 18px">{{ formattedAccount }}</p>
        </div>
        <div class="card">
          <h2>Current inflation</h2>
          <p style="font-size: 18px">{{ inflationRate }}%</p>
        </div>
        <div class="card">
          <h2>Current supply</h2>
          <p style="font-size: 18px">{{ formattedSupply }}</p>
        </div>
        <div class="card">
          <h2>Bonded/Unbonded</h2>
          <p style="font-size: 18px">{{ formattedBond }}/{{ formattedUnbond }}</p>
        </div>
        <div class="card">
          <h2>Total transactions</h2>
          <p style="font-size: 18px">{{ formattedTransaction }}</p>
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
        <supply-chart v-if="supplyDataLoaded" :supplyData="supplyData"></supply-chart>
      </div>
      <div class="stats">
        <h3>Inflation's</h3>
        <inflation-chart v-if="inflationDataLoaded" :inflationData="inflationData"></inflation-chart>
      </div>
      <div class="stats">
        <h3>Staking pool</h3>
        <staking-chart v-if="stakingPoolDataLoaded" :stakingPoolData="stakingPoolData"></staking-chart>
      </div>
      <div class="stats">
        <h3>Transaction</h3>
        <transaction-chart v-if="transactionDataLoaded" :transactionData="transactionData"></transaction-chart>
      </div>
    </div>
  </main>
</template>

<script>
import axios from 'axios';
import AccountChart from './AccountChart.vue';
import SupplyChart from './SupplyChart.vue';
import InflationChart from './InflationChart.vue';
import StakingChart from './StakingChart.vue';
import TransactionChart from './TransactionChart.vue';

export default {
  components: {
    AccountChart,
    SupplyChart,
    InflationChart,
    StakingChart,
    TransactionChart
  },
  data() {
    return {
      apiChainUrl: import.meta.env.VITE_CHAIN_API_URL,
      apiUrl: import.meta.env.VITE_API_URL,
      denom: import.meta.env.VITE_DENOM,
      minimalDenom: import.meta.env.VITE_MINIMAL_DENOM,
      accountDataLoaded: false,
      supplyDataLoaded: false,
      inflationDataLoaded: false,
      stakingPoolDataLoaded: false,
      transactionDataLoaded: false,
      accountData: [],
      inflationData: [],
      stakingPoolData: [],
      supplyData: [],
      transactionData: [],
      transaction: null,
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
    formattedTransaction() {
      return this.transaction !== undefined && this.transaction !== null ? Number(this.transaction).toLocaleString() : 'Loading...';
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
    async fetchSupplyData() {
      try {
        const response = await axios.get(this.apiUrl + 'supply');
        this.supplyData = response.data

        this.supplyDataLoaded = true
      } catch (error) {
        console.error('Error fetching supplies:', error);
      }
    },
    async fetchInflationData() {
      try {
        const response = await axios.get(this.apiUrl + 'inflation');
        this.inflationData = response.data

        this.inflationDataLoaded = true
      } catch (error) {
        console.error('Error fetching inflation:', error);
      }
    },
    async fetchStakingPoolData() {
      try {
        const response = await axios.get(this.apiUrl + 'staking');
        this.stakingPoolData = response.data

        this.stakingPoolDataLoaded = true
      } catch (error) {
        console.error('Error fetching staking pool:', error);
      }
    },
    async fetchTransactionData() {
      try {
        const response = await axios.get(this.apiUrl + 'transaction');
        this.transactionData = response.data

        this.transactionDataLoaded = true
      } catch (error) {
        console.error('Error fetching transaction:', error);
      }
    },
    async fetchTransaction() {
      try {
        const response = await axios.get(this.apiUrl + 'transaction/total');
        this.transaction = response.data.total
      } catch (error) {
        console.error('Error fetching transaction total:', error);
      }
    },
  },
  created() {
    this.fetchPool();
    this.fetchAccounts();
    this.fetchSupply();
    this.fetchInflation();
    this.fetchAccountData();
    this.fetchSupplyData();
    this.fetchInflationData();
    this.fetchStakingPoolData();
    this.fetchTransactionData();
    this.fetchTransaction();
  },
};
</script>

<style scoped>

</style>
