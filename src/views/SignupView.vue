<template>
  <div class="signup">
    <h1>This is an sign up page</h1>
    <div class="container">
      <div class="row">
        <div class="col-sm"></div>
        <div class="col-sm">
          <form>
            <div class="form-group">
              <label for="exampleInputEmail1">Email address</label>
              <input
                type="email"
                v-model="username"
                class="form-control"
                id="exampleInputEmail1"
                aria-describedby="emailHelp"
                placeholder="Enter email"
              />
              <small id="emailHelp" class="form-text text-muted"
                >We'll never share your email with anyone else.</small
              >
            </div>
            <div class="form-group">
              <label for="exampleInputEmail1">Enter Username</label>
              <input
                type="Text"
                class="form-control"
                id="exampleInputUsername1"
                placeholder="Enter Username"
              />
            </div>

            <div class="form-group">
              <label for="exampleInputEmail1">Gender</label>
              <div>
                <select
                  class="custom-select mr-sm-2"
                  id="inlineFormCustomSelect"
                >
                  <option selected>Choose...</option>
                  <option value="1">Male</option>
                  <option value="2">Female</option>
                  <option value="3">Other</option>
                </select>
              </div>
            </div>

            <div class="form-group">
              <label for="exampleInputPassword1">Password</label>
              <input
                type="password"
                v-model="password"
                class="form-control"
                id="exampleInputPassword1"
                placeholder="Password"
              />
            </div>
            <div class="form-group">
              <label for="exampleInputPassword1">Retype password</label>
              <input
                type="password"
                class="form-control"
                v-model="repassword"
                id="exampleInputPassword1"
                placeholder="Password"
              />
            </div>

            <button type="button" @click="signup" class="btn btn-primary">
              Submit
            </button>
          </form>
        </div>
        <div class="col-sm"></div>
      </div>
    </div>
  </div>
</template>
<script>
import { firebase } from "@/firebase";
export default {
  name: "sign-up",
  data() {
    return {
      username: "",
      password: "",
      repassword: "",
    };
  },
  methods: {
    signup() {
      if (this.password != this.repassword) {
        alert("vaš password se ne podudara");
      } else if (this.password.length < 6) {
        alert("password mora sadrzavati najmanje 6 znamenki");
      } else {
        firebase
          .auth()
          .createUserWithEmailAndPassword(this.username, this.password)
          .then(function () {
            console.log("uspješna auth");
          })
          .catch(function () {
            console.error("doslo je do greske");
          });
        console.log("Nastavak");
      }
    },
  },
};
</script>
