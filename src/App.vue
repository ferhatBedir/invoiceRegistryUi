<template>
  <div id="app" style="margin: 1% 2%">

    <div class="row justify-content-center">
      <accountantComp></accountantComp>
      <billComp></billComp>
    </div>

    <div>
      <billSearch></billSearch>
    </div>

  </div>
</template>

<script>
  import {eventBus} from './main'
  import Accountant from "./Accountant";
  import Bill from "./Bill";
  import BillSearch from "./BillSearch";

  export default {
    components: {
      accountantComp: Accountant,
      billComp: Bill,
      billSearch: BillSearch
    },
    data() {
      return {
        accountantGetUrl: 'http://localhost:9090/invoice/registry/accountant/find'
      }
    },
    mounted() {
      document.addEventListener('DOMContentLoaded', (event) => {
        this.$http.get(this.accountantGetUrl).then((response) => {
          eventBus.$emit("accountantEvent", response.body)
        });
      });
    }
  }
</script>

<style>

</style>
