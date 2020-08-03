<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-xl-4 col-md-4">
          <stats-card>
            <div slot="header" class="icon-success">
              <i class="nc-icon nc-favourite-28 text-success"></i>
            </div>
            <div slot="content">  
              <p class="card-category">Pacientes Adherentes</p>
              <h4 class="card-title">{{widgets.count_adherent_patients}}</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-caret-up"></i>+2.1%
            </div>
          </stats-card>
        </div>

        <div class="col-xl-4 col-md-4">
          <stats-card>
            <div slot="header" class="icon-warning">
              <i class="nc-icon nc-fav-remove text-warning"></i>
            </div>
            <div slot="content">
              <p class="card-category">Pacientes NO Adherentes</p>
              <h4 class="card-title">{{widgets.count_non_adherent_patients}}</h4>
            </div>
            <div slot="footer">
              <!--<i class="fa fa-calendar-o"></i>Last day -->
              <i class="fa fa-caret-down"></i>-5.3%
            </div>
          </stats-card>
        </div>

        <div class="col-xl-4 col-md-4">
          <stats-card>
            <div slot="header" class="icon-info">
              <i class="nc-icon nc-chart-pie-36 text-primary"></i>
            </div>
            <div slot="content">
              <p class="card-category">Adherencia promedio</p>
              <h4 class="card-title">{{widgets.avg_ongoing_adherence}}%</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-caret-up"></i>+1%
            </div>
          </stats-card>
        </div>

        <!--<div class="col-xl-3 col-md-6">
          <stats-card>
            <div slot="header" class="icon-danger">
              <i class="nc-icon nc-chart-pie-36 text-danger"></i>
            </div>
            <div slot="content">
              <p class="card-category">Errors</p>
              <h4 class="card-title">23</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-clock-o"></i>Last day
            </div>
          </stats-card>
        </div>-->


      </div>
      <div class="row">
        <div class="col-md-8" v-if="!show_chart_1">
          <card >
            <template slot="header">
              <h4 class="card-title">Adherencia Promedio</h4>
              <p class="card-category">Fuente: Omnivida</p>
            </template>
            <div class="loader"></div>
          </card>
        </div>
        <div class="col-md-8" v-if="show_chart_1">
          <!-- <card>
            <template slot="header">
              <h4 class="card-title">Adherencia por medicamento</h4>
            </template>
            <ejs-chart id="container" :primaryXAxis='primaryXAxis'>
              <e-series-collection>
                  <e-series :dataSource='seriesData1' type='Column' xName='x' yName='y'> </e-series>
                  <e-series :dataSource='seriesData2' type='Column' xName='x' yName='y'> </e-series>
              </e-series-collection>
          </ejs-chart>
          </card> -->
          <chart-card
            :chart-data="barChart.data"
            :chart-options="barChart.options"
            :chart-responsive-options="barChart.responsiveOptions"
            chart-type="Bar">
            <template slot="header">
              <h4 class="card-title">Adherencia Promedio</h4>
              <p class="card-category">Fuente: Omnivida</p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> Adherencia
                <!-- <i class="fa fa-circle text-danger"></i> Dupilumab
                <i class="fa fa-circle text-warning"></i> Mepolizumab -->
              </div>
              <hr>
              <div class="stats">
                <i class="fa fa-check"></i> Datos verificados
              </div>
            </template>
          </chart-card>
        </div>
        <div class="col-md-4" v-if="!show_chart_2">
          <card >
            <template slot="header">
              <h4 class="card-title">Adherencia en pacientes</h4>
              <p class="card-category">Fuente: Omnivida</p>
            </template>
            <div class="loader"></div>
          </card>
        </div>

        <div class="col-md-4" v-if="show_chart_2">
          <chart-card :chart-data="pieChart.data" chart-type="Pie">
            <template slot="header">
              <h4 class="card-title">Adherencia en pacientes</h4>
              <p class="card-category">Fuente: Omnivida</p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> No Adherente
                <i class="fa fa-circle text-danger"></i> Adherente
                <!--<i class="fa fa-circle text-warning"></i> Unsubscribe-->
              </div>
              <hr>
              <div class="stats">
                <i class="fa fa-clock-o"></i> Actualizado hoy
              </div>
            </template>
          </chart-card>
        </div>
      </div>

      <div class="row">
       <!-- Adherence factors:  TODO - define  using https://jscharting.com/examples/chart-types/bar/-->
        <div class="col-md-6"> 
          <card>
             <template slot="header">
              <h4 class="card-title">Piramide de Adherencia</h4>
              <p class="card-category">Fuente: Omnivida</p>
            </template>
            <ejs-chart id="container" title="" :primaryXAxis="primaryXAxis" :palettes='palettes'>
              <e-series-collection>
                  <e-series :dataSource='seriesData' type='StackingBar' xName='x' yName='y' name='Apple'> </e-series>
                  <e-series :dataSource='seriesData' type='StackingBar' xName='x' yName='y1' name='Orange'> </e-series>
              </e-series-collection>
            </ejs-chart>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> Mujeres
                <i class="fa fa-circle text-danger"></i> Hombres
                <!--<i class="fa fa-circle text-warning"></i> Unsubscribe-->
              </div>
            </template>
          </card>
          <!-- <chart-card
            :chart-data="bar_factores.data"
            :chart-options="bar_factores.options"
            :chart-responsive-options="bar_factores.responsiveOptions"
            chart-type="Bar">
            <template slot="header">
              <h4 class="card-title">Factores Adherencia</h4>
              <p class="card-category">Categorías de adherencia de un paciente </p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> % Factor
              </div>
              <hr>
              <div class="stats">
                <i class="fa fa-check"></i> Datos verificados
              </div>
            </template>
          </chart-card> -->


        </div>
        <!-- Adherence progress per patient -->
         <!-- <div class="col-md-6">
          <chart-card :chart-data="lineChart.data"
                      :chart-options="lineChart.options"
                      :responsive-options="lineChart.responsiveOptions">
            <template slot="header">
              <h4 class="card-title">Adherencia en pacientes</h4>
              <p class="card-category">Fuente: Omnivida</p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> Paciente 1
                <i class="fa fa-circle text-danger"></i> Paciente 2
                <i class="fa fa-circle text-warning"></i> Paciente 3
                <i class="fa fa-circle text-muted"></i> Paciente 4
              </div>
              <hr>
              <div class="stats">
                <i class="fa fa-history"></i> Actualizado hace 5 segundos
              </div>
            </template>
          </chart-card>
        </div> -->
         <div class="col-md-6">
          <card>
            <template slot="header">
             <h4 class="card-title">Pacientes de riesgo</h4>
              <p class="card-category">Fuente: Omnivida</p>
            </template>
            <div class="loader"  v-if="!show_table_1"></div>
            <l-table :data="tableData.data"
                     :columns="tableData.columns"
                     v-if="show_table_1">
              <template slot="columns"></template>

              <template slot-scope="{row}">
                <td>{{row.id_patient}}</td>
                <td>{{row.first_name}} {{row.last_name}}</td>
                <td class="td-actions text-right">
                  <button type="button" class="btn-simple btn btn-xs btn-info" v-tooltip.top-center="editTooltip">
                    <i class="fa fa-edit"></i>
                  </button>
                  <button type="button" class="btn-simple btn btn-xs btn-danger" v-tooltip.top-center="deleteTooltip">
                    <i class="fa fa-times"></i>
                  </button>
                </td>
              </template>
            </l-table>
            <!-- <div class="footer">
              <hr>
              <div class="stats">
                <i class="fa fa-history"></i> Actualizado hace 2 minutos
              </div>
            </div> -->
          </card>
        </div>

      </div>
      <!-- <div class = "row">
        
        <div class="col-md-6">
          <card>
            <template slot="header">
             <h4 class="card-title">Pacientes de riesgo</h4>
              <p class="card-category">Fuente: Omnivida</p>
            </template>
            <l-table :data="tableData.data"
                     :columns="tableData.columns">
              <template slot="columns"></template>

              <template slot-scope="{row}">
                <td>{{row.id_patient}}</td>
                <td>{{row.first_name}} {{row.last_name}}</td>
                <td class="td-actions text-right">
                  <button type="button" class="btn-simple btn btn-xs btn-info" v-tooltip.top-center="editTooltip">
                    <i class="fa fa-edit"></i>
                  </button>
                  <button type="button" class="btn-simple btn btn-xs btn-danger" v-tooltip.top-center="deleteTooltip">
                    <i class="fa fa-times"></i>
                  </button>
                </td>
              </template>
            </l-table>
            <div class="footer">
              <hr>
              <div class="stats">
                <i class="fa fa-history"></i> Actualizado hace 2 minutos
              </div>
            </div>
          </card>
        </div>
      </div> -->
    </div>
  </div>
</template>
<script>
  import ChartCard from 'src/components/Cards/ChartCard.vue'
  import StatsCard from 'src/components/Cards/StatsCard.vue'
  import LTable from 'src/components/Table.vue'
  import axios from 'axios';
  import https from 'https';
  import {ColumnSeries, StackingBarSeries, Category } from "@syncfusion/ej2-vue-charts";
  export default {
    components: {
      LTable,
      ChartCard,
      StatsCard
    },
    mounted(){
      this.getInfoWidget();
      this.getAdherenceYear();
      this.getAdherenceGender();
      this.patientHighRisk();
    },
    data () {
      return {
        show_chart_1: false,
        show_chart_2: false,
        show_table_1: false,
        widgets:{
          avg_ongoing_adherence: "",
          count_adherent_patients: "",
          count_non_adherent_patients: ""
        },
        editTooltip: 'Edit Task',
        deleteTooltip: 'Remove',
        pieChart: {
          data: {
            labels: [],
            series: []
          }
        },
        seriesData:[],
        primaryXAxis: {
            valueType: 'Category',
            title: 'Rango de edad'
        },
        palettes:["#1DC7EA", "#FF4A55"],
      // Information regarding the historic background data of the patient
        lineChart: {
          data: {
            labels: ['2015', '2016', '2017', '2018', '2019', '2020', '2021', '2022'],
            series: [
              [80, 100, 49, 80, 110, 86, 98, 110, 120],
              [53, 60, 43, 60, 87, 76, 85, 105, 110],
              [10, 50, 35, 48, 46, 69, 65, 98, 104],              
              [1, 3, 25, 38, 40, 39, 60, 80, 90]
            ]
          },
          options: {
            low: 0,
            high: 125,
            showArea: false,
            height: '245px',
            axisX: {
              showGrid: false
            },
            lineSmooth: true,
            showLine: true,
            showPoint: true,
            fullWidth: true,
            chartPadding: {
              right: 50
            }
          },
          responsiveOptions: [
            ['screen and (max-width: 640px)', {
              axisX: {
                labelInterpolationFnc (value) {
                  return value[0]
                }
              }
            }]
          ]
        },
        barChart: {
          data: {
            labels: [],
            series: []
          },
          options: {
            seriesBarDistance: 10,
            axisX: {
              showGrid: false
            },
            height: '245px'
          },
          responsiveOptions: [
            ['screen and (max-width: 640px)', {
              seriesBarDistance: 5,
              axisX: {
                labelInterpolationFnc (value) {
                  return value[0]
                }
              }
            }]
          ]
        },
        bar_factores: {
          data: {
            labels: ['Factor A', 'Factor B', 'Factor C', 'Factor D'],
            series: [
              [91, 93, 58, 95],
            ]
          },
          options: {
            seriesBarDistance: 50,
          
            axisX: {
              showGrid: false
            },
            height: '275px',
          },
          responsiveOptions: [
            ['screen and (max-width: 940px)', {
              seriesBarDistance: 10,
              axisX: {
                labelInterpolationFnc (value) {
                  return value[0]
                }
              }
            }]
          ]
        },
        tableData: {
          data: []
        },
        // Factor's table
        Factor_Table:{
        
             data:[
              {value: 45,
              max: 100}
             ]
          
        }
      }
    },
    provide: { 
      chart: [StackingBarSeries, ColumnSeries, Category]
    },
     methods: {
      patientHighRisk() {
        axios.get(process.env.VUE_APP_BACKEND_SERVER+'/patients/high-risk')
        .then(response => {
          var keysTable = Object.keys(response.data);
          var valuesTable = Object.values(response.data);
          this.show_table_1 = true;
          for (var key of keysTable) {
            this.tableData.data.push(valuesTable[key])
            //console.log("VALUE", keysTable['0'])
          }
        })
        .catch(err => {
          console.log(err)
        })

      },
      getInfoWidget() {
        axios.get(process.env.VUE_APP_BACKEND_SERVER+'/patients/stats/adherence/counts')
          .then(response => {
            this.widgets = response.data;
            var total = response.data.count_adherent_patients + response.data.count_non_adherent_patients;
            var avg_no_adherece = (response.data.count_non_adherent_patients/total)*100;
            var avg_adherece = (response.data.count_adherent_patients/total)*100;
            this.pieChart = {
              data: {
                labels: [`${parseFloat(avg_no_adherece.toFixed(2))}%`, `${parseFloat(avg_adherece.toFixed(2))}%`],
                series: [avg_no_adherece, avg_adherece]
              }
            }
            this.show_chart_2 = true;          
          })
          .catch(err => {
            console.log(err)
          })
      },
      getAdherenceYear() {
        axios.get(process.env.VUE_APP_BACKEND_SERVER+'/patients/stats/adherence/last-year')
          .then(response => {
            this.barChart.data.labels = Object.keys(response.data.ongoing_adherence_percentage);
            this.barChart.data.series.push(Object.values(response.data.ongoing_adherence_percentage));
            this.show_chart_1 = true;
            //this.widgets = response.data;
          })
          .catch(err => {
            console.log(err)
          })
      },
      getAdherenceGender() {
        axios.get(process.env.VUE_APP_BACKEND_SERVER+'/patients/stats/adherence/gender')
          .then(response => {
            console.log("ADHETRENCIA GENRO", response.data)

            for (var index in response.data.age_groups) {
              var value = {
                x: response.data.age_groups[index],
                y: response.data.female_adherence[index],
                y1: - response.data.male_adherence[index]
              }
              this.seriesData.push(value);
            }

            console.log("this.seriesData", this.seriesData)


           /* seriesData:[
             { x: 'Jan', y: 6, y1: 6, y2: -1 }, { x: 'Feb', y: 8 , y1: 8, y2: -1.5 },
              { x: 'Mar', y: 12, y1: 11, y2: -2 }, { x: 'Apr', y: 15, y1: 16, y2: -2.5 },
              { x: 'May', y: 20, y1: 21, y2: -3 }, { x: 'Jun', y: 24, y1: 25, y2: -3.5 },
              { x: 'Jul', y: 28, y1: 27, y2: -4 }, { x: 'Aug', y: 32, y1: 31, y2: -4.5 },
              { x: 'Sep', y: 33, y1: 34, y2: -5 }, { x: 'Oct', y: 35, y1: 34, y2: -5.5 },
              { x: 'Nov', y: 40, y1: 41, y2: -6 }, { x: 'Dec', y: 42, y1: 42, y2: -6.5 }
        ]*/
            //this.widgets = response.data;
          })
          .catch(err => {
            console.log(err)
          })
      }
    }
  }
</script>
<style>
body{
  font-family: sans-serif;
}
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.box{
    width: 800px;
    padding: 40px;
    margin: 50px auto;
    background: #f3f3f3;
    box-shadow: 3px 3px 30px 3px rgba(0, 0, 0, 0.1);
}

.loader {
  border: 16px solid #f3f3f3; /* Light grey */
  border-top: 16px solid #3498db; /* Blue */
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
