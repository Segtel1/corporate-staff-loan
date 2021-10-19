<template>
  <div class="d-flex align-items-center min-vh-100">
    <CContainer fluid>
      <CRow class="justify-content-center">
        <CCol md="6">
          <CCard class="mx-4 mb-0">
            <CCardBody class="p-4">
              <CForm @submit.prevent="saveBusiness">
                <h3>Register</h3>
                <p class="text-muted">Create a staff account</p>

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

                <CInput
                      placeholder="Business Code"
                      v-model="form.businessCode"
                      required
                    >
                      <template #prepend-content><CIcon name="cil-user"/></template>
                </CInput>
                
                <CButton color="success" type="submit" block>Create Account</CButton>
              </CForm>
            </CCardBody>
          </CCard>
        </CCol>
      </CRow>
    </CContainer>
  </div>
</template>

<script>
export default {
  name: 'Register',
  data () {
    return {
      selected: [], // Must be an array reference!
      horizontal: { label:'col-3', input:'col-9' },
      form: {
        contactLastName:"",
        contactOtherName:"",
        contactEmail:"",
        contactPhoneNumber:"",
        businessCode:"",
       }
    }
  },
  methods: {
    saveStaff(){
          const axios = require('axios');
          axios.post(process.env.VUE_APP_API_BASE_URL +'Onboard/staff', this.form)
          .then((res) => {
              // this.showGateway = true
              // this.pay_data.amount = res.data.amount
              // this.pay_data.ref = res.data.ref
              // this.pay_data.email = res.data.email
              
              console.log("success")
              })
          .catch((err) => {
              // if(err.data.message){
              //   this.$store.dispatch("alerts/error",err.data.message); 
              // }
              // this.$store.dispatch("alerts/error","Oops ,something went wrong");     
            })
       },
  },
  created() {

  },
}
</script>
