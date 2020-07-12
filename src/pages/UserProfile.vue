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

  export default {
    components: {
      EditProfileForm,
      UserCard,
      PatientForm
    }
  }

</script>
<style>

</style>
