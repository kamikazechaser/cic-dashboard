<template>
  <v-container>
    <v-card elevation="0" class="mt-4">
      <v-row justify="center">
        <v-col cols="12">
          <v-card-title>Latest 25 Transactions</v-card-title>
          <v-data-table
            :loading="loading"
            :headers="txHeaders"
            :items="txData"
            class="elevation-0"
          >
            <template #[`item.tx_success`]="{ item }">
              <v-chip small :color="item.tx_success ? 'green' : 'red'" dark>
                {{ item.tx_success ? 'Yes' : 'No' }}
              </v-chip>
            </template>
            <template #[`item.time`]="{ item }">
              <span>{{ new Date(item.time).toLocaleString() }}</span>
            </template>
            <template #[`item.tx_value`]="{ item }">
              <span>{{ item.tx_value / 1000000 }}</span>
            </template>
            <template #[`item.tx_hash`]="{ item }">
              <span class="truncate">{{ item.tx_hash }}</span>
            </template>
            <template #no-data>No transactions found</template>
          </v-data-table>
        </v-col>
      </v-row>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'UsersTransactionsPage',
  data() {
    return {
      loading: false,
      txData: [],
      voucherTxCount: 0,
      txHeaders: [
        {
          text: 'Successful',
          value: 'tx_success',
        },
        {
          text: 'Time',
          value: 'tx_date',
        },
        {
          text: 'Value',
          value: 'tx_value',
        },
        {
          text: 'Voucher',
          value: 'voucher',
        },

        {
          text: 'Tx Hash',
          value: 'tx_hash',
        },
        {
          text: 'From',
          value: 'sender_identifier',
        },
        {
          text: 'To',
          value: 'recipient_identifier',
        },
      ],
    }
  },
  async fetch() {
    this.loading = true
    const txData = await this.$axios.get(
      `/admin/latest-transactions/${this.$route.params.phone}?per_page=25`
    )
    this.txData = txData.data
    this.loading = false
  },
}
</script>

<style>
.truncate {
  display: inline-block;
  width: 180px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
