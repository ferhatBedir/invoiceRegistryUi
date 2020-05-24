<template>
  <div class="container">

    <div class="row justify-content-center" style="margin: 3% 0">
      <div class="col-sm-3">
        <button type="button" class="btn btn-primary" @click="findAll()">Get All Bills</button>
      </div>
      <div class="col-sm-3">
        <button type="button" class="btn btn-primary" @click="findAllByState(true)">Get Active Bills</button>
      </div>
      <div class="col-sm-3">
        <button type="button" class="btn btn-primary" @click="findAllByState(false)">Get Passive Bills</button>
      </div>
      <div class="col-sm-3">
        <div class="form-group row">
          <div class="col-sm-12">
            <input type="email" class="form-control" id="firstName" v-model="firstName" required
                   placeholder="FirstName">
          </div>
        </div>

        <div class="form-group row">
          <div class="col-sm-12">
            <input type="email" class="form-control" id="lastName" v-model="lastName" required placeholder="LastName">
          </div>
        </div>

        <div class="form-group row">
          <div class="col-sm-12">
            <input type="email" class="form-control" id="email" v-model="email" required placeholder="Email">
          </div>
        </div>

        <button type="button" class="btn btn-primary" @click="findAllByOwnerInfo()">Get Bills By Owner</button>
      </div>
    </div>
    <div class="row justify-content-center">
      <table class="table">
        <thead>
        <tr>
          <th scope="col">First Name</th>
          <th scope="col">Last Name</th>
          <th scope="col">Email</th>
          <th scope="col">Bill No</th>
          <th scope="col">Amount</th>
          <th scope="col">Product Name</th>
          <th scope="col">Bill State</th>
          <th scope="col">Name</th>
          <th scope="col">Surname</th>
        </tr>
        </thead>
        <tbody v-for="bill in billList">
        <tr>
          <td>{{bill.firstName}}</td>
          <td>{{bill.lastName}}</td>
          <td>{{bill.email}}</td>
          <td>{{bill.billNo}}</td>
          <td>{{bill.amount}}</td>
          <td>{{bill.productName}}</td>
          <td>{{bill.billState}}</td>
          <td>{{bill.accountantName}}</td>
          <td>{{bill.accountantSurname}}</td>
        </tr>
        </tbody>
      </table>
    </div>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        billList: {},
        firstName: null,
        lastName: null,
        email: null,
        findAllGetUrl: 'http://localhost:9090/invoice/registry/bill/find/all',
        findAllByStateGetUrl: 'http://localhost:9090/invoice/registry/bill/find/state?value=',
        findAllByOwnerInfoGetUrl: 'http://localhost:9090/invoice/registry/bill//find/info?name=',
      }
    },
    methods: {
      findAll() {
        this.$http.get(this.findAllGetUrl).then((response) => {
          this.billList = response.body;
        })
      },
      findAllByState(state) {
        let url = this.findAllByStateGetUrl;
        url += state;
        this.$http.get(url).then((response) => {
          this.billList = response.body;
        })
      },
      findAllByOwnerInfo() {
        let url = this.findAllByOwnerInfoGetUrl + this.firstName + '&surname=' + this.lastName + '&email=' + this.email;
        this.$http.get(url).then((response) => {
          this.billList = response.body;
        })
      }
    }
  }
</script>

<style scoped>

</style>
