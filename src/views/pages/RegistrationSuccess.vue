<template>
    <!-- <CRow>
       <CCol col="12" md="6"> -->
            <center>
                <CAlert show color="success">
                    <CIcon name="cil-check-circle" :height="60"/> <br> <br>
                    <h5 class="alert-heading">SUCCESS</h5>
                    <p>
                        A message has been sent to the email address you provided , kindly click on the link provided in the mail to confirm your account.
                    </p>
                    <p>
                       If you did not receive a mail after five minutes ,  <CLink href="#" class="alert-link" @click="resend">click here to resend the mail.</CLink>
                    </p>
                    <!-- <hr>
                    <p class="mb-0">
                    Whenever you need to, be sure to use margin utilities to keep things nice and tidy.
                    </p> -->
                </CAlert>
            </center>
        <!-- </CCol>
    </CRow> -->
</template>

<script>
export default {
   props: {
     email:String
   },
   methods: {
    resend(){
          const axios = require('axios');
         
          axios.post(process.env.VUE_APP_API_BASE_URL +'Accounts/resend-confirm-account', this.email)
          .then((res) => {
               console.log("success")
              })
          .catch((err) => {
              // if(err.data.message){
              //   this.$store.dispatch("alerts/error",err.data.message); 
              // }
              // this.$store.dispatch("alerts/error","Oops ,something went wrong");     
            })
       },
   }
}
</script>