<template>
  <div class="c-app flex-row align-items-center">
    <CContainer>
      <CRow class="justify-content-center">
        <CCol md="5">
            <CCard class="p-4">
              <CCardBody>
                <CForm @submit.prevent="confirm">
                  <h3>Confirm Account</h3>
                  <p class="text-muted">Password must contain minimum of 8 characters , atleast 1 alphabet, 1 number and a special character</p>
                    <CAlert
                        color="danger"
                        closeButton
                         v-show="error == true"
                    >
                        {{message}}
                    </CAlert>
                    
                    <CAlert
                        color="success"
                        v-show="success == true"
                    >
                        Account confirmed successfully <a href="/login" class="alert-link">click here to proceed to login</a>.
                    </CAlert>

                    <CAlert
                        color="success"
                        v-show="resendSuccess == true"
                    >
                       <b>Success</b> Kindly follow the link send to your email for account activation
                    </CAlert>
                  <CInput
                    placeholder="Password"
                    type="password"
                    v-model="form.password"
                  >
                    <template #prepend-content><CIcon name="cil-lock-locked"/></template>
                  </CInput>
                  <p class="text-danger" v-show="match == true">Password does not match!</p>
                  <CInput
                    placeholder="Confirm Password"
                    type="password"
                    v-model="form.confirmPassword"
                  >
                    <template #prepend-content><CIcon name="cil-lock-locked"/></template>
                  </CInput>
                  <CRow>
                    <CCol col="6" class="text-left">
                      <CButton color="success" type="submit" class="px-4">Submit</CButton>
                      <CButton color="link" class="px-0" @click="resend()">Resend link</CButton>
                    </CCol>
                  </CRow>
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
  data() {
    return {
      match:false,
      message:"",
      error:false,
      success:false,
      resendSuccess:false,
      rdata:{
          email:""
      },
      form: {
        password:"",
        confirmPassword:"",
        token:this.$route.query.token
      }
    }
  },
  methods:{
    confirm(){
         if(this.form.password != this.form.confirmPassword){
           this.match = true 
         }
         else {
            this.match = false
            this.error = false

            const axios = require('axios');
            axios.post(process.env.VUE_APP_API_BASE_URL +'Accounts/confirm-account', this.form)
            .then((res) => {
               this.form.password = ""
               this.form.confirmPassword = ""
               this.success = true
            })
            .catch((err) => {
                
                if(err.response){
                  this.message = err.response.data.message; 
                  this.error = true
                }    
            });
         }

       },
    resend(){
          const axios = require('axios');
          this.rdata.email = "profsegtel@gmail.com"
          axios.post(process.env.VUE_APP_API_BASE_URL +'Accounts/resend-confirm-account?email='+this.rdata.email )
          .then((res) => {
                this.resendSuccess = true
              })
          .catch((err) => {
               if(err.response){
                  this.message = err.response.data.message; 
                  this.error = true
                }      
            })
       },
  }
}
</script>
