<template>
    <div>
        <h1>Feed: {{ posts.length }}</h1>

        <div v-for="post in posts" :key="post.id">
            <v-card>
                <v-card-title>
                    {{ post.msg }}
                </v-card-title>

                <v-card-text>
                    <div v-if="post.posted">
                        Posted 
                        {{ post.since }} min
                        ago
                    </div>

                    <div v-else>
                        Is gonna be post on: 
                        <!-- {{ post.upload_at }} -->
                        {{ post.since }} min
                    </div>
                    
                    <v-spacer />
                    <div v-if="post.posted" class="float-right">
                        ✅
                    </div>
                </v-card-text>

                

                <v-card-actions>
                    <v-btn color="pink accent-3" @click="deletePost(post.id)" v-if="!post.posted">
                        Delete
                    </v-btn>
                </v-card-actions>
            </v-card>
        </div>

    </div>
</template>

<script lang="ts">
import Day from 'dayjs';

interface post {
    id: number;
    msg: string;
    upload_at: any;
}

export default {
    props: ['changes'],
    created() {
        this.load()
    },
    methods: {
        async deletePost(id: number) {

            const tokenId = localStorage.getItem("tokenId");

            console.log("Deleted!", id);

            const body = {
                "id": id
            }

            const response = await fetch("http://localhost:8000/api/del_post", {
                headers: {
                    "Content-Type": "application/json",
                    "Accept": 'application/json',
                    "Authorization": `Bearer ${tokenId}`
                },
                method: "POST",
                body: JSON.stringify(body)
            })

            const json = await response.json();

            if (json.code == 200) {
                // alert("Deleted!");

                this.load()
            } else {
                alert("Something went wrong");
            }
        },

        async load() {
            this.posts = []

            const tokenId = localStorage.getItem("tokenId");

            const response = await fetch("http://localhost:8000/api/get_all", {
                headers: {
                    "Content-Type": "application/json",
                    'Accept': 'application/json',
                    "Authorization": `Bearer ${tokenId}`
                },
                method: "POST",
            });

            const posts: any = await response.json()

            posts.forEach( (post: any) => {
                const now  = Day();
                const time = post.upload_at;
                post.since = now.diff(time, 'minute');
                this.posts.push(post)
            });

            this.posts.reverse();
        },
    },
    data: () => ({
        posts: [] as post[],
    }),
    watch: {
        changes() {
            this.load()
            console.log("yolo")
        }
    }
}
</script>