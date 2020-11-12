<template>
  <v-card>
    <v-card-title>
      Schedule a post for tomorrow
    </v-card-title>

    <v-card-text>
      Msg:
    </v-card-text>

    <v-text-field class="mx-9" v-model="msg" />

    <input type="datetime-local" v-model="date" />
    
    <v-card-actions>
      <v-btn color="green accent-3" @click="send">Schedule</v-btn>
    </v-card-actions>

  </v-card>
</template>


<script>
export default {
  name: "Post",
  data: _ => ({
    date: "",
    msg: ""
  }),
  methods: {
    async send() {

      const body = {
        msg: this.msg,
        at: this.date,
        oauth: localStorage.getItem("oauth_token"),
        secret: localStorage.getItem("secret")
      }

      const response = await fetch("http://localhost:8000/api/post", {
        mode: "cors",
        method: "POST",
        headers: {
          Authorization: `Bearer ${localStorage.getItem("tokenId")}`,
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        body: JSON.stringify(body)
      })

      console.log(await response.text())

      console.log("sended!");
    }
  }
}
</script>