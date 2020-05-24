<template>

  <div class="col-md-6">
    <div class="panel panel-info">
      <div class="panel-heading">
        <h4>Add Accountant</h4>
      </div>
      <div class="panel-body">

        <div class="form-group row">
          <label for="name" class="col-sm-3 col-form-label">Name</label>
          <div class="col-sm-9">
            <input type="email" class="form-control" id="name" v-model="accountant.name">
          </div>
        </div>

        <div class="form-group row">
          <label for="surname" class="col-sm-3 col-form-label">Surname</label>
          <div class="col-sm-9">
            <input type="email" class="form-control" id="surname" v-model="accountant.surname">
          </div>
        </div>

        <div>
          <button @click.prevent="addAccountant()" class="btn btn-primary">Submit</button>
        </div>

        <div v-if="alertDiv" :class="alertClass" style="margin: 3%">
          <strong>{{alertMessage}}</strong>
        </div>


      </div>
    </div>
  </div>

</template>

<script>
  export default {
    data() {
      return {
        accountant: {
          name: null,
          surname: null
        },
        accountantPostUrl: 'http://localhost:9090/invoice/registry/accountant/add',
        accountantGetUrl: 'http://localhost:9090/invoice/registry/accountant/find',
        alertDiv: false,
        alertClass: null,
        alertMessage: null

      }
    },
    methods: {
      addAccountant() {
        this.$http.post(this.accountantPostUrl, this.accountant)
          .then((response) => {
            let result = response.body;
            if (result.result === 1) {
              let myInterval = setInterval(this.showAlert('alert alert-success', 'ACCOUNTANT_ADDED..'), 1000);
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
  }
</script>

<style scoped>

</style>
