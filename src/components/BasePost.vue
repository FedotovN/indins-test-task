<template>
  <div class="post">
        <div class="post_header">
            <small>{{ getLocaleDate(post.date) }}</small>
            <span>/</span>
            <small style="font-weight: bold;">{{ post.authorName }}</small>
            <span>/</span>
        <a :href="post.authorUrl">{{post.authorUrl}}</a>
        </div>
        <div class="post_content">
            <p ref="postContent">{{ post.content }}</p>
        </div>
    </div>
</template>

<script>
/*eslint-disable*/
import getColorByTone from "../utils/getColorByTone"
export default {
    name: 'BasePost',
    props: {
        post: {
            type: Object,
            required: true
        }
    },
    methods: {
        getLocaleDate(utcDate) {
            const date = Date.parse(utcDate)
            return Intl.DateTimeFormat('RU-ru', {
                hour: '2-digit',
                minute: '2-digit',
                day: 'numeric',
                month: 'long',
                year: 'numeric',
            }).format(date)
        }
    },
    mounted() {
        const textNode = this.$refs.postContent.innerHTML,
              paintInfo = this.post.contentPostTones
        this.$refs.postContent.innerHTML = ''
        
        textNode.split('').forEach(character => {
            const span = document.createElement('span')
            span.textContent = character
            this.$refs.postContent.appendChild(span)
        })

        paintInfo.forEach(part => {
            const { position, length, tone } = part,
                  inlineColorValue = getColorByTone(tone),
                  childrenArr = this.$refs.postContent.childNodes
            childrenArr.forEach((node, ind) => {
                if(ind < position + length && ind >= position) {
                    node.style.backgroundColor = inlineColorValue
                }
            })
        });
    }
}
</script>

<style>

</style>