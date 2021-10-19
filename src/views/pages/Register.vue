<template>
  <div class="d-flex align-items-center min-vh-100">
    <CContainer fluid>
      <CRow class="justify-content-center">
        <CCol md="6">
          <regSuccess :email="resendEmail" v-if="success == true"></regSuccess>
          <CCard class="mx-4 mb-0" v-else>
            <CCardBody class="p-4">
              <CForm @submit.prevent="saveBusiness">
                <h3>Register</h3>
                <p class="text-muted">Create your account</p>
                <CAlert
                    color="danger"
                    closeButton
                    v-show="error == true"
                   >
                    {{message}}
                </CAlert>
                <CRow>
                  <CCol md="6">
                    <CInput
                      placeholder="Lastname"
                      autocomplete="username"
                      v-model="form.contactLastName"
                      required
                    >
                      <template #prepend-content><CIcon name="cil-user"/></template>
                    </CInput>
                  </CCol>

                  <CCol md="6">
                    <CInput
                      placeholder="Firstname"
                      autocomplete="username"
                      v-model="form.contactOtherName"
                      required
                    >
                      <template #prepend-content><CIcon name="cil-user"/></template>
                    </CInput>
                  </CCol>
                </CRow>

                <CRow>
                  <CCol md="6">
                      <CInput
                        placeholder="Email"
                        type="email"
                        prepend="@"
                        v-model="form.contactEmail"
                        required
                      />
                  </CCol>

                  <CCol md="6">
                    <CInput
                      placeholder="Phone number"
                      type="number"
                      v-model="form.contactPhoneNumber"
                      required
                    >
                      <template #prepend-content><CIcon name="cil-contact-phone"/></template>
                    </CInput>
                  </CCol>
                </CRow>

                <CRow>
                  <CCol md="6">
                    <CInput
                      placeholder="Business name"
                      autocomplete="username"
                      v-model="form.businessName"
                      required
                    >
                      <template #prepend-content><CIcon name="cil-user"/></template>
                    </CInput>
                  </CCol>

                  <CCol md="6">
                      <select
                        class="form-control"
                        placeholder="Corporate Sector"
                        v-model="form.sector"
                        required
                      >
                         <option value="">-- Corporate Sectors --</option>
                         <option :value="cs.id" v-for="cs in corporate_sectors" :key="cs.id">{{cs.sectorName}}</option>
                      </select>
                  </CCol>
                </CRow>

                <CRow>

                  <CCol md="6">
                      <select
                        class="form-control"
                        placeholder="Corporate Type"
                        v-model="form.corporateType"
                        required
                      >
                         <option value="">-- Corporate Type --</option>
                         <option :value="ct.id" v-for="ct in corporate_types" :key="ct.id">{{ct.corporateName}}</option>
                      </select>
                  </CCol>

                  <CCol md="6">
                    <CInput
                      placeholder="Corporate Number"
                      type="number"
                      v-model="form.corporateNumber"
                    >
                      <template #prepend-content><CIcon name="cil-user"/></template>
                    </CInput>
                  </CCol>
                </CRow>

                <CRow>
                  <CCol md="6">
                      <select
                        class="form-control"
                        v-model="form.countryCode"
                        @change="getState(form.countryCode)"
                        required
                      >
                         <option value="">-- Select Country --</option>
                         <option :value="cn.id" v-for="cn in countries" :key="cn.id">{{cn.countryName}}</option>
                      </select>
                  </CCol>

                  <CCol md="6">
                      <select
                        class="form-control"
                        :disabled="states.length == 0"
                        v-model="form.state"
                        required
                      >
                         <option value="">-- Select State --</option>
                         <option :value="st.id" v-for="st in states[0]" :key="st.id">{{st.stateName}}</option>
                      </select>
                  </CCol>
                </CRow>

                <CRow>
                  <CCol md="6">
                    <label for="">Year of Incorporation</label>
                    <CInput
                      type="number"
                      v-model="form.yearOfIncorporation"
                      placeholder="2000"
                    >
                      <template #prepend-content><CIcon name="cil-calendar"/></template>
                    </CInput>
                  </CCol>

                  <CCol md="6">
                    <label for="">Number of Employees</label>
                    <CInput
                      type="number"
                      v-model="form.numberOfEmployees"
                    >
                      <template #prepend-content><CIcon name="cil-group"/></template>
                    </CInput>
                  </CCol>
                </CRow>

                <CInput
                      placeholder="Business website url"
                      autocomplete="username"
                      v-model="form.businessWebsite"
                    >
                      <template #prepend-content><CIcon name="cil-url"/></template>
                </CInput>

                <!-- <CInput
                  placeholder="Password"
                  type="password"
                  autocomplete="new-password"
                >
                  <template #prepend-content><CIcon name="cil-lock-locked"/></template>
                </CInput> -->
                
                <CButton color="success" type="submit" block>Create Account</CButton>
              </CForm>
            </CCardBody>
            <!-- <CCardFooter class="p-4">
              <CRow>
                <CCol col="6">
                  <CButton block color="facebook">
                    Facebook
                  </CButton>
                </CCol>
                <CCol col="6">
                  <CButton block color="twitter">
                    Twitter
                  </CButton>
                </CCol>
              </CRow>
            </CCardFooter> -->
          </CCard>
          
        </CCol>
      </CRow>
    </CContainer>
  </div>
</template>

<script>
import RegistrationSuccess from './RegistrationSuccess.vue';
export default {
  name: 'Register',
  components: {
    regSuccess: RegistrationSuccess
  },
  data () {
    return {
      selected: [], // Must be an array reference!
      horizontal: { label:'col-3', input:'col-9' },
      corporate_types: [],
      corporate_sectors: [],
      countries:[],
      states:[],
      resendEmail:"",
      success:false,
      message:"",
      error:false,
      form: {
        contactLastName:"",
        contactOtherName:"",
        contactEmail:"",
        contactPhoneNumber:"",
        businessName:"",
        sector:0,
        corporateType:"",
        corporateNumber:"",
        countryCode:"",
        state:0,
        yearOfIncorporation:0,
        numberOfEmployees:0,
        businessWebsite:""
       }
    }
  },
  methods: {
    getCorporateTypes() {
      const axios = require('axios');
      axios
        .get(process.env.VUE_APP_API_BASE_URL +'CorporateTypes')
        .then((res) => {
          this.corporate_types = res.data.responseData;      
        })
        .catch((err) => {
        // window.location.replace("/login"); 
        }) 
    },
    getCorporateSector() {
      const axios = require('axios');
      axios
        .get(process.env.VUE_APP_API_BASE_URL +'Sectors')
        .then((res) => {
          this.corporate_sectors = res.data.responseData;      
        })
        .catch((err) => {
        // window.location.replace("/login"); 
        }) 
    },
    getCountries() {
      const axios = require('axios');
      axios
        .get(process.env.VUE_APP_API_BASE_URL +'Countries')
        .then((res) => {
          this.countries = res.data.responseData;      
        })
        .catch((err) => {
        // window.location.replace("/login"); 
        }) 
    },
    getState(cId){
      this.countries.forEach(c => {

          if(c.id == cId){
            let arr = c.states;   
            this.states.push(arr) 
          }
                   
        }); 
    },
    saveBusiness(){
          const axios = require('axios');
          this.resendEmail = this.form.contactEmail
          axios.post(process.env.VUE_APP_API_BASE_URL +'Onboard/business', this.form)
          .then((res) => {
               this.success = true
              })
          .catch((err) => {
              if(err.response){
                  this.message = err.response.data.message; 
                  this.error = true
                }    
            })
       },
  },
  created() {
    this.getCorporateTypes();
    this. getCorporateSector();
    this. getCountries();
  },
}
</script>
