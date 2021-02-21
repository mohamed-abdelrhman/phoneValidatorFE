<template>
  <div class="container">
    <div>
      <h2>Phone Validator</h2>
      <select v-model="countryCode" @change="filter">
        <option value="">
          Countries
        </option>
        <option value="(237)">
          Cameroon
        </option>
        <option value="(251)">
          Ethiopia
        </option>
        <option value="(212)">
          Morocco
        </option>
        <option value="(258)">
          Mozambique
        </option>
        <option value="(256)">
          Uganda
        </option>
      </select>
      <select v-model="status" @change="filter">
        <option value="">
          All Customers
        </option>
        <option value="valid">
          Valid
        </option>
        <option value="invalid">
          Invalid
        </option>
      </select>
      <table>
        <thead>
          <tr>
            <th>Id</th>
            <th>Name</th>
            <th>Phone</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(customer, index) in customers" :key="`${index}`">
            <td>{{ customer.id }}</td>
            <td>{{ customer.name }}</td>
            <td>{{ customer.phone }}</td>
            <td v-if="customer.status===true" style="background-color: darkseagreen">
              true
            </td>
            <td v-else style="background-color: darkred">
              false
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      customers: [],
      countryCode: '',
      status: ''
    }
  },
  async created () {
    await this.getCustomers(false, false)
  },
  methods: {
    async getCustomers (countryCode, status) {
      let params = ''
      if (countryCode && status) {
        params = '?country_code=' + countryCode + '&status=' + status
      } else if (countryCode) {
        params = '?country_code=' + countryCode
      } else if (status) {
        params = '?status=' + status
      }

      const res = await this.$axios.get('/customers' + params)
      this.customers = res.data
    },
    filter () {
      this.getCustomers(this.countryCode, this.status)
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

th {
  background-color: #dddddd;
}

input[type=text], select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  margin-top: 25px;
}

</style>
