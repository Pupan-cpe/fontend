<template>
  <div>
    {{ this.temp }}
    {{ "exam" }}

    <v-col cols="12" md="6">
      <v-card class="mx-1 mb-1">
        <v-card-title class="pa-5 pb-3">
          <p>Apex Line Chart</p>
          <v-spacer></v-spacer>
          <v-menu>
            <template v-slot:activator="{ on, attrs }">
              <v-btn icon v-bind="attrs" v-on="on">
                <v-icon color="textColor">mdi-dots-vertical</v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item
                v-for="(item, i) in users"
                :key="i"
                @click="() => {}"
              >
                <v-list-item-title>{{ item }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-card-title>
        <v-card-text class="pa-5 pt-0">
          <v-row no-gutters>
            <v-col cols="12">
              <ApexChart
                type="area"
                height="350"
                :options="apexArea.options"
                :series="apexArea.series"
              ></ApexChart>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-col>
  </div>
</template>

<script>
import ApexChart from "vue-apexcharts";
import config from "../../config";
import axios from "axios";

export default {
  name: "Charts",
  components: {
    ApexChart,
  },
  data() {
    return {
      users: [],
      loading: false,
      setIntervalId: null,
      menu: ["Edit", "Copy", "Delete", "Print"],
      apexArea: {
        options: {
          chart: {
            toolbar: {
              show: false,
            },
          },
          colors: [config.light.primary, config.light.success],
          dataLabels: {
            enabled: false,
          },
          xaxis: {
            type: "datetime",
            categories: [
              "2020-09-18T00:00:00",
              "2020-09-19T01:30:00",
              "2020-09-20T02:30:00",
              "2020-09-21T03:30:00",
              "2020-09-22T04:30:00",
              "2020-09-23T05:30:00",
              "2020-09-24T06:30:00",
            ],
          },
          tooltip: {
            x: {
              format: "dd/MM/yy HH:mm",
            },
          },
          legend: {
            show: false,
          },
          fill: {
            type: "solid",
            opacity: 0.2,
            colors: [config.light.primary, config.light.success],
          },
          stroke: {
            width: 4,
            curve: "smooth",
          },
        },

        series: [
          {
            name: "Temp",
            data: [
             10,4,1,3,4,5,3
            ],
            
          },
           {
            name: "series2",
            data: [11, 32, 45, 32, 34, 52, 41],
          },
          
        ],
      },
    };
  },

  created() {
    this.getTemp();
    this.setIntervalId = setInterval(() => {
      this.getTemp();
    }, 3000);
  },
  beforeDestroy() {
    clearInterval(this.setIntervalId);
  },

  methods: {
    async getTemp() {
      // this.loading = true;
      try {
        const users = await axios.get("http://192.168.1.136/getscale");
        this.temp = users.data.temp;
        this.hum = users.data.hum;

        this.loading = false;
        console.log("temp:");
        console.log(this.temp);
        console.log("Hum");
        console.log(this.hum);
        console.log(typeof(this.temp));
      } catch (err) {
        console.log(err); // handle errors here...
      }
    },
    generateData(count, yrange) {
      let i = 0;
      let series = [];
      while (i < count) {
        let x = "w" + (i + 1).toString();
        let y =
          Math.floor(Math.random() * (yrange.max - yrange.min + 1)) +
          yrange.min;
        series.push({
          x: x,
          y: y,
        });
        i++;
      }
      return series;
    },
    generatePieSeries() {
      let series = [];

      for (let i = 0; i < 5; i++) {
        let y = Math.floor(Math.random() * (500 - 100 + 100)) + 100;
        series.push(y);
      }
      return series;
    },
  },
};
</script>
