<template>

  <div class="col-md-6">
    <div class="panel panel-info">
      <div class="panel-heading">
        <h4>Add Bill</h4>
      </div>
      <div class="panel-body">

        <div class="form-group row">
          <label for="firstName" class="col-sm-3 col-form-label">First Name</label>
          <div class="col-sm-9">
            <input type="email" class="form-control" id="firstName" v-model="bill.firstName" required>
          </div>
        </div>

        <div class="form-group row">
          <label for="lastName" class="col-sm-3 col-form-label">Last Name</label>
          <div class="col-sm-9">
            <input type="email" class="form-control" id="lastName" v-model="bill.lastName" required>
          </div>
        </div>

        <div class="form-group row">
          <label for="email" class="col-sm-3 col-form-label">Email</label>
          <div class="col-sm-9">
            <input type="email" class="form-control" id="email" v-model="bill.email" required>
          </div>
        </div>

        <div class="form-group row">
          <label for="amount" class="col-sm-3 col-form-label">Amount</label>
          <div class="col-sm-9">
            <input type="email" class="form-control" id="amount" v-model="bill.amount" required>
          </div>
        </div>

        <div class="form-group row">
          <label for="productName" class="col-sm-3 col-form-label">Product Name</label>
          <div class="col-sm-9">
            <input type="email" class="form-control" id="productName" v-model="bill.productName" required>
          </div>
        </div>

        <div class="form-group row">
          <label for="billNo" class="col-sm-3 col-form-label">Bill No</label>
          <div class="col-sm-9">
            <input type="email" class="form-control" id="billNo" v-model="bill.billNo" required>
          </div>
        </div>

        <div class="form-group row">
          <label for="accountant" class="col-sm-3 col-form-label">Accountant</label>
          <div class="col-sm-9">
            <select class="form-control" id="accountant" v-model="bill.accountant" required>
              <option v-for="accountant in accountantList">{{accountant.id}} {{accountant.name}} {{accountant.surname}}
              </option>
            </select>
          </div>
        </div>

        <div>
          <button @click.prevent="addBill()" class="btn btn-primary">Submit</button>
        </div>

        <div v-if="alertDiv" :class="alertClass" style="margin: 3%">
          <strong>{{alertMessage}}</strong>
        </div>

      </div>
    </div>

  </div>

</template>

<script>
  import {eventBus} from './main'

  export default {
    data() {
      return {
        bill: {
          firstName: null,
          lastName: null,
          email: null,
          amount: null,
          productName: null,
          billNo: null,
          accountant: null,
          accountantId: null
        },
        accountantList: [],
        billPostUrl: 'http://localhost:9090/invoice/registry/bill/add',
        alertDiv: false,
        alertClass: null,
        alertMessage: null
      }
    },
    methods: {
      addBill() {
        console.log(this.bill);
        if (this.bill.accountant !== null) {
          this.bill.accountantId = this.bill.accountant.split(" ")[0];
        }
        this.$http.post(this.billPostUrl, this.bill).then((response) => {
          let result = response.body;
          if (result.result === 1) {
            let myInterval = setInterval(this.showAlert('alert alert-success', 'BILL_ADDED..'), 1000);
            this.closeAlert(myInterval);
          } else {
            let myInterval = setInterval(this.showAlert('alert alert-danger', 'SOME_PARAMETERS_INVALID..'), 1000);
            this.closeAlert(myInterval);
          }
        })
      },
      showAlert(bootstrapClass, alertMessage) {
        this.alertClass = bootstrapClass;
        this.alertDiv = true;
        this.alertMessage = alertMessage;
      },
      closeAlert(interval) {
        clearInterval(interval);
        location.reload();
      }
    }
    ,
    created() {
      eventBus.$on("accountantEvent", (event) => {
        this.accountantList = event;
      })
    }
  }
</script>

<style scoped>

</style>
