<template>
    <div>
        <v-toolbar color="primary">
            <v-toolbar-title>
                Give us access to your favorite Social Network
            </v-toolbar-title>
        </v-toolbar>
        <h1>Supported Paltforms:</h1>

        <section>
            <v-btn color="cyan accent-3" @click="getTwit">
                Twitter
            </v-btn>
        </section>

    </div>
</template>

<script lang="ts">
export default {
    name: "Twitter",
    methods: {
        async getTwit() {
        const url = 'http://localhost:8000/api/getOauthToken';
        
        const response = await fetch(url);

        const text = await response.text();

        // RESPONSE: oauth_token=P-F0BgAAAAABJpJxAAABdbx1FwQ&oauth_token_secret=FXzdGJ2GMEaQ7ixtGNUIfw5a1hiM7Sgv&oauth_callback_confirmed=true1

        const oauthToken = text.split("&")[0].replace("oauth_token=", "");

        location.href = `https://api.twitter.com/oauth/authorize?oauth_token=${oauthToken}`;

        // return oauthToken;
    }
    }
}
</script>