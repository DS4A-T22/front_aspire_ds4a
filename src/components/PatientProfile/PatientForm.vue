<template>
    <card>
        <h4 slot="header" class="card-title">Paciente {{completeName}}</h4>
        <form>
            <div class="row">
                <div class="col-md-4">
                <base-input type="text"
                            label="Nombres"
                            placeholder="Nombres"
                            v-model="patient.firstName">
                </base-input>
                </div>
                <div class="col-md-4">
                <base-input type="text"
                            label="Apellidos"
                            placeholder="Apellidos"
                            v-model="patient.lastName">
                </base-input>
                </div>
                <div class="col-md-2">
                <base-input type="date"
                            label="Fecha de nacimiento"
                            placeholder="Fecha de nacimiento"
                            v-model="patient.birthdate">
                </base-input>
                </div>
                <div class="col-md-2">
                    <div class="form-group">
                        <label for="sel1">Genero</label>
                        <select class="form-control" id="sel1" v-model="patient.gender">
                            <option>Femenino</option>
                            <option>Masculino</option>
                        </select>
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-md-3">
                    <div class="form-group">
                        <label for="sel1">Tipo de identificación</label>
                        <select class="form-control" id="sel1" v-model="patient.identification_type">
                            <option>C.C - Cédula de Ciudadanía</option>
                            <option>C.E - Cédula de Extrajería</option>
                            <option>T.I - Tarjeta de Identidad</option>
                            <option>P.A - Pasaporte</option>
                        </select>
                    </div>
                </div>
                <div class="col-md-3">
                <base-input type="text"
                            label="Nº de identificación"
                            placeholder="Nº de identificación"
                            v-model="patient.identification">
                </base-input>
                </div>
                <div class="col-md-3">
                    <div class="form-group">
                        <label for="sel1">Estado civil</label>
                        <select class="form-control" id="sel1" v-model="patient.identification_type">
                            <option>Casado(a)</option>
                            <option>Soltero(a)</option>
                            <option>Separado(a)</option>
                            <option>Union Libre</option>
                            <option>Viudo(a)</option>
                            <option>Sin definir</option>
                        </select>
                    </div>
                </div>
                <div class="col-md-3">
                <base-input type="text"
                            label="Correo"
                            placeholder="Correo"
                            v-model="patient.email">
                </base-input>
                </div>
            </div>
            <div class="row">
                <div class="col-md-3">
                <base-input type="text"
                            label="Departamento"
                            placeholder="Departamento"
                            v-model="patient.department">
                </base-input>
                </div>
                <div class="col-md-3">
                <base-input type="text"
                            label="Ciudad"
                            placeholder="Ciudad"
                            v-model="patient.city">
                </base-input>
                </div>
                <div class="col-md-2">
                    <div class="form-group">
                        <label for="sel1">Zona</label>
                        <select class="form-control" id="sel1" v-model="patient.zone">
                            <option>Urbana</option>
                            <option>Rural</option>
                        </select>
                    </div>
                </div>
                <div class="col-md-2">
                    <div class="form-group">
                        <label for="sel1">Estrato</label>
                        <select class="form-control" id="sel1" v-model="patient.social_stratum">
                            <option>1</option>
                            <option>2</option>
                            <option>3</option>
                            <option>4</option>
                            <option>5</option>
                        </select>
                    </div>
                </div>
                <div class="col-md-2">
                    <div class="form-group">
                        <label for="sel1">Nivel Sisben</label>
                        <select class="form-control" id="sel1" v-model="patient.socioeconomic_level">
                            <option>Nivel 0</option>
                            <option>Nivel 1</option>
                            <option>A</option>
                            <option>B</option>
                            <option>C</option>
                            <option>N.A</option>
                        </select>
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-md-3">
                    <div class="form-group">
                        <label for="sel1">Regimen</label>
                        <select class="form-control" id="sel1" v-model="patient.social_security_regime">
                            <option>Contributivo</option>
                            <option>Subsidiado</option>
                            <option>Particular</option>
                            <option>Otro</option>
                        </select>
                    </div>
                </div>
                <div class="col-md-3">
                    <div class="form-group">
                        <label for="sel1">Tipo de afiliación</label>
                        <select class="form-control" id="sel1" v-model="patient.social_security_affiliation_type">
                            <option>Cotizante</option>
                            <option>Beneficiario</option>
                            <option>Subsidiado</option>
                        </select>
                    </div>
                </div>
                <div class="col-md-3">
                    <div class="form-group">
                        <label for="sel1">Ocupacion</label>
                        <select class="form-control" id="sel1" v-model="patient.occupation">
                            <option>Empleado</option>
                            <option>Estudiante</option>
                            <option>Sin definir</option>
                            <option>Pensionado</option>
                            <option>Desempleado</option>
                            <option>Ama de casa</option>
                            <option>Jubilado</option>
                            <option>Independiente</option>
                        </select>
                    </div>
                </div>
                <div class="col-md-3">
                    <div class="form-group">
                        <label for="sel1">Tipo de empleo</label>
                        <select class="form-control" id="sel1" v-model="patient.social_security_regime">
                            <option>Empleado</option>
                            <option>Menor no labora</option>
                            <option>Sin empleo</option>
                            <option>Pensionado</option>
                            <option>Independiente</option>
                        </select>
                    </div>
                </div>
            </div>
            <div class="text-center">
        <button type="submit" class="btn btn-info btn-fill float-right" @click.prevent="savePatient">
          Guardar
        </button>
      </div>
        </form>
    </card>
</template>
<script>
  import Card from 'src/components/Cards/Card.vue'

  export default {
    components: {
      Card
    },
    computed: {
    // a computed getter
        completeName: function () {
        // `this` points to the vm instance
            return `${this.patient.firstName} ${this.patient.lastName}`
        }
    },
        data () {
      return {
        patient: {
          identification_type: '',
          identification: '',
          email: '',
          firstName: '',
          lastName: '',
          gender: '',
          education: '',
          civil_status: '',
          birthdate: '',
          department: '',
          city:'',
          zone: '',
          social_stratum: '',
          socioeconomic_level: '',
          social_security_regime: '',
          social_security_affiliation_type: '',
          occupation: '',
          employment_type: ''
        }
      }
    },
    methods: {
      savePatient () {
          //const color = Math.floor((Math.random() * 4) + 1)
            this.$notifications.notify(
            {
                message: `<span>Paciente <b>${this.completeName}</b> - guardado con exito!</span>`,
                icon: 'nc-icon nc-app',
                horizontalAlign: 'center',
                verticalAlign: 'top',
                type: 'success'
            })
            this.patient =  {
                identification_type: '',
                identification: '',
                email: '',
                firstName: '',
                lastName: '',
                gender: '',
                education: '',
                civil_status: '',
                birthdate: '',
                department: '',
                city:'',
                zone: '',
                social_stratum: '',
                socioeconomic_level: '',
                social_security_regime: '',
                social_security_affiliation_type: '',
                occupation: '',
                employment_type: ''
            }
       }
    }
  }

</script>
<style>

</style>