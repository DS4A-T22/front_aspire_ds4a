<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <card class="strpied-tabled-with-hover"
                body-classes="table-full-width table-responsive"
          >
            <template slot="header">
              <h4 class="card-title">Pacientes</h4>
              <p class="card-category">Lista de pacientes OMNIVIDA</p>
            </template>
            <l-table class="table table-hover" :columns="tableColumns" :data="tableData">
              <template slot="columns" style="width: 100%;!important">
                  <th></th>
                  <th>Nombres</th>
                  <th></th>
                  <th style="width: 100%;!important">Adherencia</th>
              </template>
              <template slot-scope="{row}">
                <td><img style="width: 50px;" v-bind:src="row.image" /></td>
                <td><p style="margin-bottom: 0;!important"><b>{{row.first_name}} {{row.last_name}}</b></p > {{row.age}} años</td>
                <td>
                  <button class="btn btn-icon btn-info" @click="handleEdit(row)"><i class="fa fa-edit"></i></button>
                  <button class="btn btn-icon btn-danger" @click="handleDelete(row)"><i class="fa fa-trash"></i></button>
                </td>
                <td>78%</td>
              </template>
          </l-table>
          </card>
          <nav aria-label="Page navigation example">
            <ul class="pagination">
              <li v-for="(item, index) in 22" v-bind:key="index" class="page-item">
                <a class="page-link" href="#"> {{ index + 1}}</a>
              </li>
              <!-- <li class="page-item"><a class="page-link" href="#">Previous</a></li>
              <li class="page-item"><a class="page-link" href="#">1</a></li>
              <li class="page-item"><a class="page-link" href="#">2</a></li>
              <li class="page-item"><a class="page-link" href="#">3</a></li>
              <li class="page-item"><a class="page-link" href="#">Next</a></li> -->
            </ul>
          </nav>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import LTable from 'src/components/Table.vue'
  import Card from 'src/components/Cards/Card.vue'
  import axios from 'axios';
  const tableColumns = ['Id', 'Name', 'Salary', 'Country', 'City']
  export default {
    components: {
      LTable,
      Card
    },
    mounted(){
      this.getPatients();
    },
    data() {
      return {
        patients:[],
        tableColumns: ['Id', 'Name', 'Salary', 'Operations'],
        tableData: []
      }
    },
    methods: {
      assignImage(gender, age) {
        if (gender === 'M' && age<18){
          return 'img/users/children.png';
        } 
        else if (gender === 'M' && age>18 && age<65){
          return 'img/users/athlete.png';
        }
        else if (gender === 'M' && age>65){
          return 'img/users/grandfather.png';
        }else if (gender === 'F' && age<18){
          return 'img/users/girl.png';
        } 
        else if (gender === 'F' && age>18 && age<65){
          return 'img/users/woman.png';
        }
        else if (gender === 'F' && age>65){
          return 'img/users/grandmother.png';
        }
      },
      calculateAge(birtdate) {
        var hoy = new Date();
        var cumpleanos = new Date(birtdate);
        var edad = hoy.getFullYear() - cumpleanos.getFullYear();
        var m = hoy.getMonth() - cumpleanos.getMonth();
        if (m < 0 || (m === 0 && hoy.getDate() < cumpleanos.getDate())) {
            edad--;
        }
        return edad;
      },
      getPatients(){
         axios.get(process.env.VUE_APP_BACKEND_SERVER+'/patients')
          .then(response => {
           // console.log("Pacientes", response.data);
            this.patients = response.data.slice(0, 9);
            for (var patient of this.patients) {
              var new_patient = patient;
              new_patient['age'] = this.calculateAge(new_patient['birthdate']);
              new_patient['image'] = this.assignImage(new_patient['gender'], new_patient['age'])
              this.tableData.push(new_patient);
            }
            console.log("this.patients", this.patients);
          })
          .catch(err => {
            console.log(err)
          })

      },
      handleEdit(row){
        console.log(`You want to edit row with id: ${row.id}`)
      },
      handleDelete(row){
        console.log(`You want to delete row with id: ${row.id}`)
      }
    }
  }
</script>
<style>
</style>

