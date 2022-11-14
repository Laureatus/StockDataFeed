<template>
    <div>
        <Navbar/>
        <ArrivalTable/>
    </div>
</template>
  
  <script>
import Navbar from './Navbar.vue';
import ArrivalTable from './ArrivalTable.vue'

  export default {
    name: "Landingpage",
    components: { Navbar, ArrivalTable}
}
  </script>