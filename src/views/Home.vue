<template>
  <div>
    <v-toolbar color="primary">
      <v-toolbar-title>
        Ares Social Post
      </v-toolbar-title>
    </v-toolbar>
    <v-container>
      <post @reload_posts="reload_posts" />
      <br />
      <feed :changes="posts"/>
    </v-container>
  </div>
</template>

<script lang="ts">
import Post from "@/components/Post.vue";
import Feed from '@/components/Feed.vue';

export default {
  components: { Post, Feed },
  data() {
    return {
      posts: 0
    }
  },
  created() {
    const token = localStorage.tokenId;
    console.log(token);

    if (!token) {
      alert("You are not registerd!");
      location.href = "#/login";

      

    } else {
      
      if (localStorage.oauth_token == undefined) {
        this.SendTokens()
      }



    }
  },
  methods: {
    async SendTokens() {
      const url = new URLSearchParams(location.search);
      const oauthToken = url.get("oauth_token");
      const oauthVerifier = url.get("oauth_verifier");

      if (oauthToken == null) {
        console.log("no params")
        location.href = '#/register'
      }

      // console.log([oauthToken, oauthVerifier]);

      const response = await fetch('http://localhost:8000/api/get_tokens', {
        mode: 'cors',
        method: 'POST',
        headers: {
          "Content-Type": "application/json",
          "Accept": "application/json"
        },
        body: JSON.stringify({
          "oauth_token": oauthToken,
          "oauth_verifier": oauthVerifier
        }),
      });

      console.log(response.status)

      const text = await response.text();

      console.log(text);

      // oauth_token=3179672652-Ds2ziK4Thc8925A3q0vfDe3wuzkI62RmIvD6aTs&oauth_token_secret=FJYd0v1Kzg6vTTRqh3YIieFBmng3CcZpvm8ZPi7JB2PXV&user_id=3179672652&screen_name=RichardJimenez0https://api.twitter.com/oauth/access_token?oauth_consumer_key=Y1fNSKYjazdbihdqlu11XcZHK&oauth_token=AHXOjgAAAAABJpJxAAABdbyYSIg&oauth_verifier=RkGqp1ZmFx9pCcJHIYrGIWVHsb9U35W71

      const tokens = {
        "oauth": text.split("&")[0].replace("oauth_token=", ""),
        "secret": text.split("&")[1].replace("oauth_token_secret=", "")
      }
      localStorage.setItem("oauth_token", tokens.oauth)
      localStorage.setItem("secret", tokens.secret)
      console.log(tokens);
    },

    reload_posts(id: number) {
      console.log(id);

      this.posts += id
      
    }
  }
};
</script>
