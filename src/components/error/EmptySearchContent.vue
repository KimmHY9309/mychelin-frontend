<template>
    <div>
        <div class="d-flex flex-column text-center">
            <div v-if="isloading">
                <img class="img-empty" src="@/assets/images/asdasd.jpg" alt="" />
            </div>
            <div v-else>
                <img class="img-empty" src="@/assets/images/food_focus.jpg" alt="" />
                <p class="big-font">'{{ data }}'(으)로 검색한 결과가 없습니다.</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        data: String,
    },
    computed: {
        isloading() {
            return this.$store.state.loading;
        },
    },
};
</script>

<style scoped>
.img-empty {
    width: 150px;
    height: 150px;
    border-radius: 10em;
}

.big-font {
    font-size: 1.2em;
}
</style>
