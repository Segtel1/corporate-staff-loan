<template>
  <div class="c-app flex-row align-items-center">
    <CContainer>
      <CRow class="justify-content-center">
        <CCol md="8">
          <CCardGroup>
            <CCard class="p-4">
              <CCardBody>
                <CForm @submit.prevent="login">
                  <h4>Login</h4>
                  <p class="text-muted">Sign In to your account</p>
                  <CAlert
                      color="danger"
                      closeButton
                      v-show="error == true"
                    >
                      {{message}}
                  </CAlert>
                  <CInput
                    placeholder="Email"
                    v-model="email"
                  >
                    <template #prepend-content>@</template>
                  </CInput>
                  <CInput
                    placeholder="Password"
                    type="password"
                    v-model="password"
                  >
                    <template #prepend-content><CIcon name="cil-lock-locked"/></template>
                  </CInput>
                  <CRow>
                    <CCol col="6" class="text-left">
                      <CButton color="primary" type="submit" class="px-4">Login</CButton>
                    </CCol>
                    <CCol col="6" class="text-right">
                      <CButton color="link" class="px-0">Forgot password?</CButton>
                      <CButton color="link" class="d-lg-none">Register now!</CButton>
                    </CCol>
                  </CRow>
                </CForm>
              </CCardBody>
            </CCard>
            <CCard
              color="primary"
              text-color="white"
              class="text-center py-5 d-md-down-none"
              body-wrapper
            >
              <CCardBody>
                <h4>Sign up</h4>
                <p>Corporate staff loan application , disbursement and repayment made easy</p>
                <CButton
                  color="light"
                  variant="outline"
                  size="md"
                  href="/register"
                >
                  Register as a business
                </CButton>
                <CButton
                  color="light"
                  variant="outline"
                  size="md"
                  style="margin-left:10px;"
                    href="/staff/register"
                >
                  Register as a staff
                </CButton>
              </CCardBody>
            </CCard>
          </CCardGroup>
        </CCol>
      </CRow>
    </CContainer>
  </div>
</template>

<script>
import { AUTH_REQUEST } from "../../store/actions/auth";

export default {
  name: 'Login',
  data () {
    return {
      message:"",
      error:false,
      email:"",
        password:""
    }
  },
  methods:{
      login(){
          // const axios = require('axios');
          // axios.post(process.env.VUE_APP_API_BASE_URL +'Accounts', this.form)
          // .then((res) => {
          //     const token = res.data.accessToken
          //     localStorage.setItem('user-token', token)

          //     this.$router.push('/dashboard')
          // })
          // .catch((err) => {
          //   console.log(err.response)
          //     if(err.response){
          //         this.message = err.response.data.message; 
          //         this.error = true
          //       }    
          //   });

        const { email, password } = this
          this.$store.dispatch(AUTH_REQUEST, {email, password }).then(() => {
            this.$router.push('/')
          })
       }
  }
}
</script>
