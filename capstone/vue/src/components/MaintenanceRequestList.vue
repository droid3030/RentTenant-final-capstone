<template>
  <div>
    <table id="request-table">
      <tr>
        <th>Property ID</th>
        <th>Request Date</th>
        <th>Title</th>
        <th>Description</th>
        <th>Status</th>
        <th>Mark Complete</th>
      </tr>

      
      <tr
        class="data-row"
        v-for="request in currentRequests"
        :key="request.requestID"
      >
      <th>{{ request.propertyID }}</th>
      <th>{{ request.date }}</th>
        <th>
          <!-- <router-link :to="{name:'request-detail', params:{ID: request.requestID}}">  -->
            {{ request.title }}
            <!-- </router-link> -->
            </th>
        
        <th>{{request.description}}</th>
        <th>{{ request.status }}</th>
        <th>
          <button class="button" v-if="request.status != 'COMPLETE' " @click="markComplete(request)"/>
        </th>
      </tr>
      
    </table>
  </div>
</template>

<script>
import MaintenanceService from "../services/MaintenanceService";
export default {
  data() {
    return {
      currentRequests: [],
    };
  },
  methods: {
    getMaintenanceRequests() {
      return MaintenanceService.getRequestsByPropertyPrincipal().then(
        (response) => {
          if (response.status === 200) {
            this.currentRequests = response.data;
          }
        }
      );
    },
    markComplete(request){
      MaintenanceService.markComplete(request).then((response) => {
        if (response.status === 200) {
            request.status="COMPLETE";
          }
      })
      
    }
  },
  created() {
    this.getMaintenanceRequests();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Orbitron:wght@900&display=swap");
#request-table {
  border: 1px;
}

table,
th,
tr {
  border: 1px solid black;
}
th {
  padding: 5px;
}
.data-row {
  font: 18px "Orbitron";
  font-size: 14px;
  font-weight: 400;
}
.button {
  width: 18px;
  height: 18px;
  border-radius: 9px;
  background-color: orange;
}
</style>