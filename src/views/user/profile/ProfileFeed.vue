<template>
    <div>
        <SweetModal ref="modal">
            <FeedPageElement :feed="feed" v-if="!isEmptyFeed" />
        </SweetModal>
        <div v-if="isEmpty">
            <EmptyContent data="포스트가" />
        </div>
        <div class="row row-cols-3 g-1">
            <div class="col" v-for="post in posts" v-bind:key="post.id">
                <div v-on:click="openModal(post)">
                    <div class="crop">
                        <div class="created-tag">{{ post.createDate }}</div>
                        <img v-if="isImage(post.images[0])" class="post-img" :src="imageUrl(post.images[0])" loading="lazy" />
                        <div v-if="!isImage(post.images[0])" class="text-replaced border ">{{ postTrimmed(post.content) }}</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { SweetModal } from "sweet-modal-vue";
import EmptyContent from "@/components/error/EmptyContent";
import FeedPageElement from "@/views/main/feed/FeedPageElement";
export default {
    components: {
        SweetModal,
        EmptyContent,
        FeedPageElement,
    },
    data() {
        return {
            feed: {},
            temp: false,
        };
    },
    beforeDestroy() {
        this.$refs.modal.close();
    },
    computed: {
        isEmpty() {
            if (!this.posts) return true;
            return this.posts.length === 0;
        },
        isEmptyFeed() {
            return Object.keys(this.feed).length === 0;
        },
        existFeed() {
            return this.temp;
        },
    },
    props: {
        posts: Array,
    },
    methods: {
        openModal(post) {
            this.feed = post;
            this.$refs.modal.open();
            this.temp = true;
        },
        isImage(imgContent) {
            return imgContent !== undefined;
        },
        imageUrl(imgContent) {
            if (imgContent === undefined) return "https://picsum.photos/200/200";
            if (imgContent.slice(0, 4) === "http") {
                return imgContent;
            } else return "https://picsum.photos/200/200";
        },
        postTrimmed(content) {
            let i = 100;
            if (content.length < i) return content;
            else return content.slice(0, i - 2) + "...";
        },
    },
};
</script>

<style scoped>
.created-tag {
    font-size: 12px;
    position: absolute;
    color: white;
    bottom: 0.4em;
    right: 0.4em;
    background: rgba(16, 16, 16, 0.3);
    border-radius: 2.42vw;
    /* transform:translateY(300%); */
    padding: 0.1em 0.5em 0.1em;
    margin-left: 0.2em;
    z-index: 2;
}

.text-replaced {
    position: absolute;
    width: 100%;
    height: 100%;

    font-family: Spoqa Han Sans Neo;
    font-style: normal;
    font-weight: 300;
    font-size: 12px;
    line-height: 20px;
    padding: 0.5em;
    background-color: rgba(100, 172, 109, 0.1);
    color: #000000;
}
.post-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}
</style>
