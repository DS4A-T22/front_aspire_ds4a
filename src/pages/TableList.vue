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
            <div class="loader" v-if="!show_table"></div>
            <l-table class="table table-hover" :columns="tableColumns" :data="tableData" v-if="show_table">
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
                  <button class="btn btn-icon btn-info" @click="handleEdit(row.id_patient)"><i class="fa fa-edit"></i></button>
                  <button class="btn btn-icon btn-danger" @click="handleDelete(row)"><i class="fa fa-trash"></i></button>
                </td>
                <td>{{row.adherence}}%</td>
              </template>
          </l-table>
          </card>
          <nav aria-label="Page navigation example">
            <ul class="pagination">
              <li v-for="(item, index) in 28" v-bind:key="index" class="page-item">
                <a class="page-link" v-on:click="btnPatients(index)"> {{ index + 1}}</a>
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
      this.getPatients(1);
    },
    data() {
      return {
        show_table:false,
        patients:[],
        tableColumns: ['Id', 'Name', 'Salary', 'Operations'],
        tableData: []
      }
    },
    methods: {
      btnPatients(event){
        this.tableData = [];
        this.show_table = false;
        this.getPatients(event+1);
      },
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
      getPatients(page){
         axios.get(process.env.VUE_APP_BACKEND_SERVER+'/patients?page='+page)
          .then(response => {
            this.show_table = true;
            var keysTable = Object.keys(response.data);
            var valuesTable = Object.values(response.data);
            this.show_table_1 = true;
            for (var key of keysTable) {
              var new_patient = valuesTable[key];
              new_patient['age'] = this.calculateAge(new_patient['birthdate']);
              new_patient['image'] = this.assignImage(new_patient['gender'], new_patient['age']);
              new_patient['adherence'] = (new_patient['pred_adherence'][1]*100).toFixed(2);
              this.tableData.push(new_patient);
              //this.tableData.data.push(valuesTable[key])
            }
           // console.log("this.patients", this.patients);
          })
          .catch(err => {
            console.log(err)
          })

      },
      handleEdit(id){
        this.$router.push('/admin/user/'+id);
        //console.log(`You want to edit row with id: ${row.id}`)
      },
      handleDelete(row){
        console.log(`You want to delete row with id: ${row.id}`)
      }
    }
  }
</script>
<style>

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

