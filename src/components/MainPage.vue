<template>
  <div>
    <main>
        <base-post v-for="(post, ind) in posts" :key="ind" :post="post" />
    </main>
    <div>
        <p v-if="loading" class="loader">Загрузка...</p>
    </div>
    <div id="intersectionTrigger"></div>
  </div>
</template>

<script>
import BasePost from "./BasePost.vue"
import feed from "../assets/feed.json"
export default {
    name: "MainPage",
    components: { BasePost },
    data: () => ({
        posts: [],
        loading: false
    }),
    created() {
        this.posts = feed
        this.loading = true
    },
    async mounted() {
        const intersectionTrigger = document.getElementById('intersectionTrigger')
        const int = new IntersectionObserver(async () => {
            // Здесь мог быть ваш await fetch('https://...')
            this.loading = true
            setTimeout(() => {
                const startPosition = Math.floor(Math.random() * (feed.length - 10)),
                      fetchedPosts = feed.slice(startPosition, startPosition + 10)
                fetchedPosts.forEach(post => {
                    this.posts.push(post)
                });
                this.loading = false
            }, 500)
        }, {threshold: 0.8})
        int.observe(intersectionTrigger)
    }
}
</script>

<style lang="scss">
    main {
        font-family: sans-serif;
        padding: 10px 15px;
        height: calc(100% - 20px);
        margin: 0 auto;
        max-width: 800px;
        display: flex;
        flex-flow: column nowrap;
        align-items: center;
        gap: 1rem;
    }
    .post {
        text-overflow: ellipsis;
        width: 100%;
        display: flex;
        flex-flow: column nowrap;
        gap: .75rem;
        border-bottom: 1px solid #eee;
        padding: 10px 5px;
        .post_header {
            display: flex;
            align-items: center;
            flex-flow: row nowrap;
            gap: .25rem;
            color: #444;
            white-space: nowrap;
            a {
                font-size: small;
                color: inherit;
                text-decoration: none;
                transition: .1s color;
                &:hover {
                    color: #555;
                }
            }
        }
        .post_content {
            max-height: 150px;
            overflow: hidden;
            p {
                color: #444;
                font-size: small;
                text-overflow: ellipsis;
                overflow: hidden;
            }
        }
    }
    .loader {
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: large;
        font-family: sans-serif;
    }
</style>