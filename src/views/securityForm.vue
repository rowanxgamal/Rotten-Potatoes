<template>
<div class="all">
  <div class="container">
    <section>
      <div class="img">
        <img src="..\assets\image 2.png" alt="" />
      </div>
      <div class="form">
        <router-link to="/login">
          <h2>
            <i class="fa-solid fa-arrow-left" style="color: #000000"></i>
          </h2>
        </router-link>
        <form @submit.prevent="changePass">
          <h3>One More Step!</h3>
          <div class="row">
            <div class="col">
              <label>Question</label>
              <p>{{ question }}</p>
            </div>
          </div>
          <div class="row">
            <div class="col">
              <label>Answer</label>
              <input type="text" required v-model="Qanswer" />
            </div>
            <div v-show="wrongAnswer" class="alert alert-danger" role="alert">
              <h5>Wrong Answer!</h5>
            </div>
          </div>
          <div class="row">
            <button class="butn">Continue</button>
          </div>
        </form>
      </div>
    </section>
  </div>
  </div>
</template>

<script>
import $store from "../store/index.js";

export default {
  data() {
    return {
      email: $store.state.forgetUser.email,
      question: $store.state.forgetUser.securityQuestion,
      answer: $store.state.forgetUser.securityAnswer,
      Qanswer: "",
      wrongAnswer: false,
    };
  },
  methods: {
    changePass() {
      this.wrongAnswer = false;
      if (this.Qanswer == this.answer) {
        this.$router.push({
          name: "newPassword",
        });
      } else {
        this.wrongAnswer = true;
      }
    },
  },
};
</script>

<style scoped>
h3 {
  text-align: center;
}
.img {
  position: relative;
  width: 55%;
  top: -130px;
}

input {
  margin-right: 5px;
  display: block;
  padding: 4px 2px;
  width: 100%;
  border: none;
  border-bottom: 2px solid #ddd;
}
h2 {
  text-align: start;
  margin-bottom: 20px;
  cursor: pointer;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.form {
  position: absolute;
  border-radius: 40px;
  padding: 50px 50px 60px 50px;
  width: 400px;
  background-color: #e0dbdb;
  text-align: start;
  right: 5%;
  border-bottom: 5px solid #9c9c9c;
  top: 20px;
}
.butn {
  background-color: #bf7934;
  border-radius: 50px;
  margin-top: 30px;
  padding: 8px;
  border: 0px solid transparent;
}
label {
  margin: 25px 0px 15px;
  font-size: 16px;
  letter-spacing: 1px;
  font-weight: bold;
}
section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 600px;
}

h6 {
  direction: rtl;
  cursor: pointer;
  padding: 20px;
  text-decoration: underline;
}
.alert {
  margin-top: 10px;
  padding: 3px 5px;
}
h5 {
  position: relative;
  color: red;
  font-size: 14px;
  margin-top: 5px;
  margin-bottom: 5px;
  left: 10px;
}
</style>