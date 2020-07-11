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
              <h4 class="card-title">1.7K</h4>
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
        <div class="col-md-6">
          <chart-card :chart-data="lineChart.data"
                      :chart-options="lineChart.options"
                      :responsive-options="lineChart.responsiveOptions">
            <template slot="header">
              <h4 class="card-title">Users Behavior</h4>
              <p class="card-category">24 Hours performance</p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> Open
                <i class="fa fa-circle text-danger"></i> Click
                <i class="fa fa-circle text-warning"></i> Click Second Time
              </div>
              <hr>
              <div class="stats">
                <i class="fa fa-history"></i> Updated 3 minutes ago
              </div>
            </template>
          </chart-card>
        </div>

        <div class="col-md-6">
          <card>
            <template slot="header">
              <h5 class="title">Tasks</h5>
              <p class="category">Backend development</p>
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
                <i class="fa fa-history"></i> Updated 3 minutes ago
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
        lineChart: {
          data: {
            labels: ['9:00AM', '12:00AM', '3:00PM', '6:00PM', '9:00PM', '12:00PM', '3:00AM', '6:00AM'],
            series: [
              [287, 385, 490, 492, 554, 586, 698, 695],
              [67, 152, 143, 240, 287, 335, 435, 437],
              [23, 113, 67, 108, 190, 239, 307, 308]
            ]
          },
          options: {
            low: 0,
            high: 800,
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
        tableData: {
          data: [
            {title: 'Sign contract for "What are conference organizers afraid of?"', checked: false},
            {title: 'Lines From Great Russian Literature? Or E-mails From My Boss?', checked: true},
            {
              title: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
              checked: true
            },
            {title: 'Create 4 Invisible User Experiences you Never Knew About', checked: false},
            {title: 'Read "Following makes Medium better"', checked: false},
            {title: 'Unfollow 5 enemies from twitter', checked: false}
          ]
        }
      }
    }
  }
</script>
<style>

</style>
