<template>
    <div>
        <div class="home-bgi"></div>
        <h1 style="display:none;">마이슐랭</h1>

        <router-link v-bind:to="{ name: 'HomePage' }"> <span class="home-name">MYCHÉLIN</span></router-link>

        <router-link v-bind:to="{ name: 'Login' }"> <button class="home-login-btn" type="button">로그인</button></router-link>

        <p class="home-mobile-title">나를 위한,<br />나에 의한,<br />멋진 맛집에 대해<br />이야기하다</p>

        <div class="home-search-bar">
            <input type="text" placeholder="관심 있는 음식을 검색해보세요" name="" class="home-search-input" v-model="searchKeyword" 
            v-on:keyup.enter="search"/>
            <span v-on:click="search"><i class="fa fa-search fa-flip-horizontal home-search-btn"></i></span>
            <button type="button" class="home-web-search-btn" v-on:click="search">검색하기</button>
        </div>

        <!-- <p class="home-mobile-title-sub">방금 추가된<br />따끈따끈한 마이슐랭 리스트</p> -->

        <p class="home-web-title">나를 위한, 나에 의한,<br />멋진 맛집에 대해 이야기하다</p>

        <p class="home-web-title-second">당신만의 미슐랭 - 마이슐랭에서 내가 만든 맛집에 대해<br />사람들과 다양한 이야기를 나눌 수 있습니다.</p>

        <!-- <div class="home-angle">
            <i class="fas fa-angle-double-down"></i>
        </div> -->

        <!-- <div class="home-list">
            <carousel :perPage="1" :paginationEnabled="false" style="width: 100%;height:160px;">
                <slide>
                    <img class="img-post" style="width:100%; height:100%" src="../../assets/images/12.jpg" />
                </slide>
                <slide>
                    <img class="img-post" style="width:100%; height:100%" src="../../assets/images/32.jpg" />
                </slide>
                <slide>
                    <img class="img-post" style="width:100%; height:100%" src="../../assets/images/34.jpg" />
                </slide>
            </carousel>
        </div> -->
        <!-- <p class="home-web-title-sub">
            방금 추가된 따끈따끈한 마이슐랭 리스트</p> -->
    </div>
</template>

<script>
import axios from "axios";
import SearchApi from "@/apis/SearchApi";
// import { Carousel, Slide } from "vue-carousel";
export default {
    name: "Home",
    components: {
        // Carousel,
        // Slide,
    },
    beforeCreate() {},
    created() {
        axios
            .get("https://koreanjson.com/users")
            .then((dataArray) => {
                this.users = dataArray.data;
            })
            .catch((e) => console.log(e));
    },
    methods: {
        search() {
            if (this.searchKeyword !== ''){
                let data = {'keyword': this.searchKeyword, 'limit': 1}
                    SearchApi.requestRestaurants(data)
                    .then((res) =>{
                        this.$store.commit('INIT_SEARCH_PLACE')
                        if (res.data.data) {
                            this.$store.commit('GET_SEARCH_PLACE', res.data.data.data); 
                        } 
                        else this.$store.commit('GET_SEARCH_PLACE', []);
                        this.$store.commit('SET_SEARCH_PAGE_KEYWORD', this.searchKeyword)
                        this.$store.commit('SWAP_SEARCH_PAGE', 2)
                        this.$router.push({name: 'SearchPage'})
                    })
            } else window.swal('검색어를 입력해주세요!')
            // this.selectedIdx = 2;
            
            // window.swal("로그인이 필요합니다.");
            // PostsApi.requestRestaurants(this.searchKeyword)
            // this.$store.commit('GET_SEARCH_KEYWORD', this.searchKeyword)
            // this.$store.commit('SWAP_PAGE', 1)
            // this.$router.push('main').catch(()=>{});  // 같은 라우터로 이동할때 에러 메세지 발생
        },
    },
    data() {
        return {
            users: [],
            searchKeyword: "",
        };
    },
};
</script>

<style>
* {
    font-family: "Spoqa Han Sans Neo", "sans-serif";
}
input:hover {
    border: none;
}
input::placeholder {
    font-weight: normal;
}

.home-bgi {
    position: absolute;
    background: linear-gradient(0deg, rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.6)), url("../../assets/images/daan-evers-tKN1WXrzQ3s-unsplash.jpg");
    background-size: cover;
    width: 100vw;
    height: 100vh;
    top: 0px;
}
.home-name {
    position: absolute;
    width: 28.26vw;
    height: 7.25vw;
    left: 6.52vw;
    top: 5.56vw;
    font-size: 5.8vw;
    line-height: 7.25vw;
    color: #ffffff;
}
.home-login-btn {
    position: absolute;
    width: 19.32vw;
    height: 7.25vw;
    left: 74.15vw;
    top: 5.56vw;
    border: 2px solid #ffffff;
    box-sizing: border-box;
    border-radius: 20px;
    font-size: 4.83vw;
    line-height: 6.04vw;
    color: #ffffff;
}
.home-mobile-title {
    position: absolute;
    width: 41.3vw;
    height: 28.99vw;
    left: 6.52vw;
    top: 27.29vw;
    font-size: 5.8vw;
    line-height: 7.25vw;
    color: #ffffff;
}
.home-search-bar {
    position: absolute;
    width: 86.96vw;
    height: 12.08vw;
    left: 6.52vw;
    top: 66.18vw;
    background: #fdfdfd;
    border: 1px solid white;
    box-sizing: border-box;
    border-radius: 25px;
    display: flex;
}
.home-search-input {
    height: 12.08vw;
    width: 100%;
    border: none;
    padding-bottom: 0.24vw;
    font-size: 4.35vw;
    border: none;
}
.home-search-btn {
    margin: 2.66vw 4.35vw 0 0;
    font-size: 5.8vw;
    color: #ff993c;
    font-weight: lighter;
}
.home-mobile-title-sub {
    position: absolute;
    width: 55.56vw;
    height: 12.08vw;
    left: 6.52vw;
    top: 133.57vw;
    font-weight: bold;
    font-size: 4.83vw;
    line-height: 6.04vw;
    color: #ffffff;
}
.home-web-title {
    display: none;
}
.home-web-title-second {
    display: none;
}
.home-web-search-btn {
    display: none;
}
.home-web-title-sub {
    display: none;
}
.home-angle {
    position: absolute;
    color: white;
    width: 100%;
    /*left: 47.1vw;*/
    text-align: center;
    top: 168vw;
    font-size: 5.8vw;
    line-height: 7.25vw;
}
.home-list {
    position: absolute;
    height: 180px;
    top: 100vh;
    width: 100%;
    padding-top: 10px;
}
@media screen and (min-width: 760px) {
    .home-list {
        display: none;
    }
    .home-angle {
        display: none;
    }
    .home-bgi {
        height: 41.32vw;
    }
    .home-name {
        width: 11.53vw;
        height: 3.13vw;
        left: 6.25vw;
        top: 2.08vw;
        font-weight: 500;
        font-size: 2.5vw;
        line-height: 3.13vw;
        letter-spacing: -0.05em;
    }
    .home-login-btn {
        width: 4.66vw;
        height: 2.08vw;
        left: 90.9vw;
        top: 2.57vw;
        border: none;
        font-size: 1.67vw;
        line-height: 2.08vw;
        font-weight: bolder;
    }
    .home-mobile-title {
        display: none;
    }
    .home-web-title {
        display: block;
        position: absolute;
        width: 100vw;
        height: 6.67vw;
        left: 0vw;
        top: 16.74vw;
        font-size: 2.5vw;
        line-height: 3.13vw;
        text-align: center;
        color: #ffffff;
    }
    .home-web-title-second {
        display: block;
        position: absolute;
        width: 100vw;
        height: 3.54vw;
        left: 0vw;
        top: 26.46vw;
        font-size: 1.67vw;
        line-height: 2.08vw;
        text-align: center;
        color: #ffffff;
    }
    .home-search-bar {
        position: absolute;
        width: 48.89vw;
        height: 4.38vw;
        left: 25.56vw;
        top: 32.29vw;
        background: #fdfdfd;
        border: 1px solid #000000;
        box-sizing: border-box;
        border-radius: 0px;
    }
    .home-search-input {
        height: 4.38vw;
        width: 100%;
        border: none;
        padding-bottom: 1px;
        font-size: 1.67vw;
        border: none;
    }
    .home-web-search-btn {
        display: block;
        width: 10vw;
        background: #ffb5a7;
        border-left: 1px solid black;
        box-sizing: border-box;
        font-size: 1.39vw;
        line-height: 2.08vw;
        color: #000000;
    }
    .home-search-btn {
        display: none;
    }
    .home-mobile-title-sub {
        display: none;
    }
    .home-web-title-sub {
        display: block;
        position: absolute;
        width: 49.31vw;
        height: 3.61vw;
        left: 5.14vw;
        top: 45.97vw;
        font-weight: bold;
        font-size: 2.5vw;
        line-height: 3.13vw;
        color: #000000;
    }
}
</style>
