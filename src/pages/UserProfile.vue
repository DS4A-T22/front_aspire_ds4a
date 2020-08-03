<template>
  <div class="content">
    <div class="container-fluid">
      <!-- <div class="row">
        <div class="col-md-8">
          <edit-profile-form>
          </edit-profile-form>
        </div>
        <div class="col-md-4">
          <user-card>
          </user-card>
        </div>
      </div> -->
      <div class="row">
        <div class="col-md-12">
          <patient-form>
          </patient-form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import EditProfileForm from './UserProfile/EditProfileForm.vue'
  import UserCard from './UserProfile/UserCard.vue'
  import PatientForm from '../components/PatientProfile/PatientForm.vue'
  import axios from 'axios';

  export default {
    components: {
      EditProfileForm,
      UserCard,
      PatientForm
    },
    data() {
      return {
        id:'',
      }
    },
    methods: {
      // getProfile(){
      //   axios.get(process.env.VUE_APP_BACKEND_SERVER+'/patients/'+this.id)
      //   .then(response => {
      //     this.patient = response.data[0];
      //     console.log("this.patients", response.data[0]);
      //   })
      //   .catch(err => {
      //     console.log(err)
      //   })
      // }
    }
  }

</script>
<style>

</style>
