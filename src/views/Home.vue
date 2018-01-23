/* eslint-disable */
<template>
  <div class="page">
    <h4 class="s-title">Instacart</h4>
    <div class="container">
      <div class="columns">
        <div class="column col-4 col-mx-auto application-container">
          <h1 class="s-title">Get paid to shop</h1>
          <div class="toast toast-error" v-if="error">
            <button class="btn btn-clear float-right" @click="error=null"></button>
            {{ error }}
          </div>
          <div v-if="step === 1">
            <div class="form-group">
              <input class="form-input" type="text" v-model="applicant.first_name" placeholder="First Name">
            </div>
            <div class="form-group">
              <input class="form-input" type="text" v-model="applicant.last_name" placeholder="Last Name">
            </div>
            <div class="form-group">
              <input class="form-input" type="email" v-model="applicant.email" placeholder="Email">
            </div>
            <div class="form-group">
              <input class="form-input" type="text" v-model="applicant.phone" placeholder="Phone">
            </div>
          </div>
          <div v-else-if="step === 2">
            <div class="panel">
              <div class="panel-header">
                <div class="panel-title">Shoppers Applicants Policy</div>
              </div>
              <div class="panel-body">
                1
                <br>2
                <br>3
                <br>4
                <br>5
                <br>5
                <br>6
                <br>7
                <br>8
                <br>9
                <br>10
              </div>
              <div class="panel-footer">
                <div class="form-group">
                  <label class="form-switch">
                    <input type="checkbox" v-model="applicant.agreed">
                    <i class="form-icon"></i> Allow Instacart to run a background check?
                  </label>
                </div>
              </div>
            </div>
          </div>
          <div v-else-if="step === 3">
            <p class="application-submitted">Your application is on the way. We will get back to you soon!</p>
          </div>
          <div class="form-group apply-now" v-if="step!==3">
            <button class="btn btn-primary input-group-btn" @click="submit">Apply Now</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Api from '../api'

export default {
  mounted() {
      let user = this.$localStorage.get('applicant');

      try {
        this.applicant = JSON.parse(user);
        if (this.applicant.agreed)
          this.step = 3;
        else if (this.applicant.email)
          this.step = 2;
      } catch (e) {
        console.log(e);
      }
    },
    watch: {
      step() {
        this.error = null;
      }
    },
    data() {
      return {
        applicant: {},
        step: 1,
        error: null
      }
    },
    methods: {
      submit() {
        switch (this.step) {
          case 1:
            if (this.applicant.email)
              this.step = 2;
            else
              this.error = "Email is required!";
            break;
          case 2:
            if (this.applicant.agreed)
              this.step = 3;
            else
              this.error = "You need to agree to continue";
            break;
        }
        if (this.applicant.email)
          this.$localStorage.set('applicant', JSON.stringify(this.applicant));
      },
    }
}

</script>
<style scoped>
.container {
  margin: auto;
  max-width: 1000px;
}

.input-group-btn {
  width: 100%;
}

.application-container {
  margin-top: 15vh;
}

.panel-body {
  height: 170px;
  overflow: scroll;
}

.apply-now {
  margin-top: 10px;
}

.application-submitted {
  margin-top: 70px;
}

.column {
  padding-right: 0;
}

</style>

