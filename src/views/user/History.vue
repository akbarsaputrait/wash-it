<template>
  <div class="home">
    <HeaderShayna/>

    <table class="table">
      <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">Customer Name</th>
        <th scope="col">Order Date</th>
        <th scope="col">Verify Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(transaction,index) in userTransactions" :key ="transaction.id">
        <th scope="row">{{index + 1}}</th>
        <td>{{ transaction.laundry_shop.name }}</td>
        <td>{{ transaction.createdAt }}</td>
        <td>
          <b-button @click="changeStatusTransaction">{{transaction.transaction_status}}</b-button>
        </td>
      </tr>

      </tbody>
    </table>

    <Footer/>
  </div>
</template>

<script>
import HeaderShayna from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'
import axios from 'axios'

export default {
  name: 'History',
  components: {
    HeaderShayna,
    Footer
  },
  data () {
    return {
      isLoading: true,
      userTransactions: [],
      user: {},
    }
  },
  created () {
    this.fetchLaundryShop()
  },
  methods: {
    async fetchLaundryShop () {
      if (localStorage.getItem('user')) {
        try {
          this.user = JSON.parse(localStorage.getItem('user'))
        } catch (e) {
          localStorage.removeItem('carts')
        }
      }
      this.isLoading = false
      try {
        const res = await axios.get('transaction/')

        if (res && res.hasOwnProperty('data')) {
          this.userTransactions = res.data.filter(data => {
            return data.userId === this.user.id
          })
        }
      } catch (error) {
        this.$notify('danger', 'Something Bad Happened', error, { duration: 5000 })
      }
    }
  },

}
</script>
