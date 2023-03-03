<template>
  <div class="pt-75 mx-auto">
    <div class="layout-row align-items-center justify-content-center">
      <input
        v-model="dateSelected"
        type="date"
        class="large outlined"
        data-testid="app-input"
        placeholder="Filter Date"
      />
      <button
        class=""
        :disabled="!dateSelected"
        data-testid="submit-button"
        @click="handleSelectedDate"
      >
        Filter
      </button>
    </div>
    <div class="card mx-auto table-card mt-50 pb-10">
      <table>
        <thead>
          <tr>
            <th>Date2</th>
            <th>Description</th>
            <th>Type</th>
            <th data-testid="amount" class="sortable" @click="sortByAmount">
              Amount ($)
            </th>
            <th>Available Balance</th>
          </tr>
        </thead>

        <tbody data-testid="records-body">
          <tr v-for="(txn, index) in transactionList" :key="index">
            <!--Use this section to render the transactions-->
            <td>{{ txn.date }}</td>
            <td>{{ txn.description }}</td>
            <td>{{ txn.type === 1 ? "Debit" : "Credit" }}</td>
            <td>{{ txn.amount }}</td>
            <td>{{ txn.balance }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "RecordsTable",
  props: {
    transactions: Array
  },
  data() {
    return {
      transactionList: [],
      dateSelected: null,
      isSorted: false,
      isFiltered: false
    };
  },
  created() {
    this.transactionList = JSON.parse(JSON.stringify(this.transactions));
  },
  watch: {
    dateSelected(newValue) {
      if (!newValue) {
        this.isFiltered = false;
        this.transactionList = JSON.parse(JSON.stringify(this.transactions));
      }
    }
  },
  methods: {
    handleSelectedDate() {
      this.transactionList = this.transactions.filter(
        item => item.date === this.dateSelected
      );
      this.isFiltered = true;
      if (this.transactionList.length === 0) {
        this.transactionList = JSON.parse(JSON.stringify(this.transactions));
        this.dateSelected = null;
        this.isFiltered = false;
      }
    },
    sortByAmount() {
      if (!this.isSorted) {
        this.transactionList.sort((a, b) =>
          a.amount < b.amount ? -1 : a.amount > b.amount ? 1 : 0
        );
      } else {
        if (!this.isFiltered) {
          this.transactionList = JSON.parse(JSON.stringify(this.transactions));
        } else {
          this.handleSelectedDate();
        }
      }
      this.isSorted = !this.isSorted;
    }
  }
};
</script>

<style scoped>
.table-card {
  padding: 12px;
}
</style>
