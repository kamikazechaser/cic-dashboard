<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" lg="7">
        <v-card elevation="0">
          <v-card-title>Pin Status</v-card-title>
          <v-data-table
            :headers="headers"
            :items="pinStatus"
            class="elevation-0"
          >
            <template #no-data> No results </template>
          </v-data-table>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'LockedPinsPage',
  middleware: 'auth',
  async fecth() {
    const pinStatus = await this.$axios.get(`/admin/pin-status`)

    this.pinStatus = pinStatus.data
  },
  data() {
    return {
      pinStatus: [],
      headers: [
        {
          text: 'Phone Number',
          value: 'phone_number',
        },
        {
          text: 'Failed Pin Attempts',
          sortable: true,
          value: 'failed_pin_attempts',
        },
        {
          text: 'Account Status',
          sortable: true,
          value: 'account_status',
        },
      ],
    }
  },
}
</script>
