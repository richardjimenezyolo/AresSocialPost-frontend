<template>
  <div>
    <v-toolbar color="primary">
      <v-spacer />
      <v-toolbar-title>
        Login
      </v-toolbar-title>
      <v-spacer />
    </v-toolbar>
    <v-container>
      <v-row justify="center" align-content="center">
        <form class="loginModel">
          <h1>Email</h1>
          <v-text-field v-model="email" />
          <h1>Password</h1>
          <v-text-field v-model="pwd" />
          <br />
          <v-btn color="pink" class="mx-2" @click="signup">Sign Up</v-btn>
          <v-btn color="primary" @click="login">Login</v-btn>
        </form>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  methods: {
    async login() {
      console.log("login");

      const response = await fetch("http://localhost:8000/api/login", {
        mode: "cors",
        method: "post",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json"
        },
        body: JSON.stringify({
          email: this.email,
          pwd: this.pwd
        })
      });

      const json = await response.json();

      const tokenId = json.Token.split("|")[1];

      localStorage.setItem("tokenId", tokenId);

      console.log(json);

      await this.getTwit();

      // console.log(await this.getTwit());
    },
    // This fucking functions is still giving me a fucking error
    // And i still have no fucking dump ass idea of why so is gonna stay there

    // Nevermind i just figured it out ;)

    async signup() {
      const credentials = JSON.stringify({
          "email": this.email,
          "pwd": this.pwd,
          "name": "Vue"
      });

      const response = await fetch("http://localhost:8000/api/signup", {
          mode: "cors",
          method: "POST",
          headers: {
          "Content-Type": "application/json"
            //   "Accept": "application/json"
          },
        body: credentials
      });

      const text = await response.text();

      console.log(text);

      // console.log(await this.getTwit());

    },

    async getTwit() {
      const url = 'http://localhost:8000/api/getOauthToken';
      
      const response = await fetch(url);

      const text = await response.text();

      // RESPONSE: oauth_token=P-F0BgAAAAABJpJxAAABdbx1FwQ&oauth_token_secret=FXzdGJ2GMEaQ7ixtGNUIfw5a1hiM7Sgv&oauth_callback_confirmed=true1

      const oauthToken = text.split("&")[0].replace("oauth_token=", "");

      location.href = `https://api.twitter.com/oauth/authorize?oauth_token=${oauthToken}`;

      // return oauthToken;
    }
  },
  data: () => ({
    email: "",
    pwd: ""
  })
};
</script>

<style>
.loginModel {
  background-color: #e3f2fd;
  border-radius: 10px;
  padding: 60px 100px;
  text-align: center;
}
</style>
