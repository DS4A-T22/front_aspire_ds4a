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
              <h4 class="card-title">+503</h4>
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
              <h4 class="card-title">52</h4>
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
              <h4 class="card-title">97%</h4>
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
        <div class="col-md-8">
          <chart-card
            :chart-data="barChart.data"
            :chart-options="barChart.options"
            :chart-responsive-options="barChart.responsiveOptions"
            chart-type="Bar">
            <template slot="header">
              <h4 class="card-title">Adherencia por medicamento</h4>
              <p class="card-category">Serie temporal </p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> Omalizumab
                <i class="fa fa-circle text-danger"></i> Dupilumab
                <i class="fa fa-circle text-warning"></i> Mepolizumab
              </div>
              <hr>
              <div class="stats">
                <i class="fa fa-check"></i> Datos verificada
              </div>
            </template>
          </chart-card>
        </div>

        <div class="col-md-4">
          <chart-card :chart-data="pieChart.data" chart-type="Pie">
            <template slot="header">
              <h4 class="card-title">Demográficos</h4>
              <p class="card-category">Distribución por género</p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> Masculino
                <i class="fa fa-circle text-danger"></i> Femenino
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
          <chart-card
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
                <i class="fa fa-check"></i> Datos verificada
              </div>
            </template>
          </chart-card>


        </div>
        <!-- Adherence progress per patient -->
         <div class="col-md-6">
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
        </div>

      </div>
      <div class = "row">
        <!-- TODO list -->
        <div class="col-md-6">
          <card>
            <template slot="header">
              <h5 class="title">Tareas pendientes</h5>
              <p class="category">Consideraciones</p>
            </template>
            <l-table :data="tableData.data"
                     :columns="tableData.columns">
              <template slot="columns"></template>

              <template slot-scope="{row}">
                <td>
                  <base-checkbox v-model="row.checked"></base-checkbox>
                </td>
                <td>{{row.title}}</td>
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

      </div>
    </div>
  </div>
</template>
<script>
  import ChartCard from 'src/components/Cards/ChartCard.vue'
  import StatsCard from 'src/components/Cards/StatsCard.vue'
  import LTable from 'src/components/Table.vue'
  export default {
    components: {
      LTable,
      ChartCard,
      StatsCard
    },
    data () {
      return {
        editTooltip: 'Edit Task',
        deleteTooltip: 'Remove',
        pieChart: {
          data: {
            labels: ['43%', '57%'],
            series: [43, 57]
          }
        },
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
            labels: ['Jan', 'Feb', 'Mar', 'Apr', 'Mai', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
            series: [
              [91, 93, 98, 95, 97, 89, 90, 94, 96, 99, 95, 96],
              [94, 96, 99, 95, 96, 97, 89, 90, 94, 96, 89, 93],
              [96, 97, 95, 94, 92, 96, 91, 97, 95, 95, 96, 93]
            ]
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
          data: [
            {title: 'Más de 100 pacientes han mejorado el grado de adherencia"', checked: false},
            {title: 'Se tiene un registro completo de todos los pacientes', checked: true},
            {
              title: 'Todos los pacientes realizan la prueba ACT periódicamente',
              checked: true
            },
            {title: 'Todos los pacientes reciben atención personalizada', checked: false},
            {title: 'Se logran determinar los factores que determinan la no adherencia en pacientes',
             checked: true},
            {title: 'Se ha realizado exitosamente la limpieza de las bases de datos presentadas', checked: false}
          ]
        },
        // Factor's table
        Factor_Table:{
        
             data:[
              {value: 45,
              max: 100}
             ]
          
        }
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
</style>
