<template>
  <div>
    <v-col cols="12">
      <!----------------------- -->
      <v-card class="mx-1 mb-1">
        <v-card-title class="pa-4 pb-0">
          <v-row no-gutters>
            <v-col cols="7" sm="4" md="4" lg="5" class="d-flex align-center">
              <p>Daily Line Chart</p>
            </v-col>
            <v-col sm="6" md="6" lg="5" class="d-none d-sm-flex align-center">
              <v-icon size="18" color="warning">mdi-circle-medium</v-icon
              ><span
                class="card-dark-grey font-weight-regular mr-3"
                style="font-size: 18px"
                >Tablet</span
              >
              <v-icon size="18" color="primary">mdi-circle-medium</v-icon
              ><span
                class="card-dark-grey font-weight-regular mr-3"
                style="font-size: 18px"
                >Mobile</span
              >
              <v-icon size="18" color="#B1BCFF">mdi-circle-medium</v-icon
              ><span
                class="card-dark-grey font-weight-regular"
                style="font-size: 18px"
                >Desktop</span
              >
            </v-col>
            <v-col cols="5" sm="2" md="2" lg="1" offset-lg="1">
              <v-menu>
                <template v-slot:activator="{ on, attrs }">
                  <v-select
                    class="main-chart-select"
                    v-model="mainApexAreaSelect"
                    v-bind="attrs"
                    v-on="on"
                    dense
                    flat
                    single-line
                    hide-details
                    :items="mock.select"
                    outlined
                  ></v-select>
                </template>
              </v-menu>
            </v-col>
          </v-row>
        </v-card-title>
        <v-card-text class="pa-4">
          <v-row>
            <v-col>
              <ApexChart
                v-if="apexLoading"
                height="350"
                type="area"
                :options="mock.mainApexArea.options"
                :series="
                  mainApexAreaSelect === 'Daily'
                    ? mock.mainApexArea.series
                    : mainApexAreaSelect === 'Weekly'
                    ? mock.mainApexArea.series2
                    : mock.mainApexArea.series3
                "
              ></ApexChart>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-col>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped></style>
