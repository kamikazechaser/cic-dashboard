<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" lg="7">
        <v-card elevation="0">
          <v-card-title>Vouchers</v-card-title>
          <v-data-table
            :loading="loading"
            :headers="headers"
            :items="voucherData"
            class="elevation-0"
          >
            <template #[`item.token_name`]="{ item }">
              <b
                ><n-link :to="`vouchers/${item.token_addres}`">
                  {{ item.token_name }}
                </n-link></b
              >
            </template>
            <template #no-data> No results </template>
          </v-data-table>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'VouchersIndexPage',
  data() {
    return {
      voucherData: [],
      currentPage: 1,
      loading: false,
    }
  },
  async fetch() {
    const vouchersReq = await this.$axios.get(`/public/tokens`)

    this.voucherData = vouchersReq.data
  },
  fetchOnServer: false,
  computed: {
    headers() {
      const headers = [
        {
          text: 'Voucher Name',
          sortable: true,
          align: 'start',
          value: 'token_name',
        },
        {
          text: 'Voucher Symbol',
          sortable: true,
          value: 'token_symbol',
        },
        {
          text: 'Voucher Address',
          sortable: false,
          value: 'token_addres',
        },
      ]

      if (this.$vuetify.breakpoint.smAndDown) {
        headers.pop()
      }

      return headers
    },
  },
}
</script>
