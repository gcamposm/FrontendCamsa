<template>
  <v-container>
      <v-layout justify-center>
      <v-flex xs12 md12 lg12>
        <v-simple-table>
          <thead>
            <tr>
              <th class="text-left">Chofer</th>
              <th class="text-left">Destino</th>
              <th class="text-left">Monto</th>
              <th class="text-left">Recibido</th>
              <th class="text-left">Vuelto</th>
              <th class="text-left">Fecha</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="report in reports" :key="report.id">
              <td>{{ report.nameDriver }}</td>
              <td>{{ report.destiny }}</td>
              <td>{{ report.amount }}</td>
              <td><v-switch
                v-model=report.received
                @change="updateReport(report)"
              ></v-switch></td>
              <td><v-switch
                v-model=report.exchange
                @change="updateReport(report)"
              ></v-switch></td>
              <td>{{ report.date }}</td>
            </tr>
          </tbody>
        </v-simple-table>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios';
import { mapState } from 'vuex';

export default {
  name: 'all-reports',
  data () {
    return {
      reports: []
    }
  },
  created() {
    this.getAllReports()
  },
  methods: {
    getAllReports(){
      axios
      .get(this.serverURL + '/reports/')
      .then(response => {
        this.reports = response.data
      })
    },
    updateReport(report){
      axios
      .put(this.serverURL + '/reports/edit/' + report.id, report)
      .then(response => {
        this.getAllReports()
      })
      .catch(e=>{
        console.log(e.response)
      })
    }
  },
  computed: {
    ...mapState([
      'serverURL'
    ])
  }
}
</script>