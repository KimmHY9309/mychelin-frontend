l<template>
    <div>
        <BackNav navTitle="프로필 사진 변경" :routeBackTo="{ name: 'ProfileEdit' }" />
        <div id="body-wrap" class="d-flex flex-column align-items-center">
            <img :src="image" alt="" class="profile-img mb-3" />
            <p v-show="!imageTypeValid" class="not-valid-message">
                이미지 형식의 파일을 선택해주세요
            </p>
            <p v-show="!imageSizeValid" class="not-valid-message">
                3MB 이하의 이미지를 선택해주세요
            </p>
            <label for="img-input" class="img-button">파일 선택</label>
            <input
                type="file"
                accept="image/*"
                @change="convertImage"
                id="img-input"
            />
        </div>
        <button @click="registerImg" class="img-button" id="register-button">
            등록
        </button>
    </div>
</template>

<script>
import PostingApi from "@/apis/PostingApi";
import UserApi from "@/apis/UserApi";
import BackNav from '@/components/navs/BackNav.vue'

export default {
    name: "EditProfileImageChange",
    components: {
        BackNav,
    },
    data() {
        return {
            image: null,
            imageTypeValid: true,
            imageSizeValid: true,
        };
    },
    methods: {
        convertImage: function (event) {
            let image = event.target.files[0];
            if (!this.validateImg(image)) {
                return;
            }
            let formData = new FormData();
            formData.append("file", image);
            PostingApi.requestImageUrl(formData).then((res) => {
                this.image = res.data.data.image;
            });
        },
        validateImg: function (image) {
            if (["image/jpeg", "image/jpg", "image/png"].includes(image.type)) {
                this.imageTypeValid = true;
            } else {
                this.imageTypeValid = false;
                return false;
            }
            if (image.size > 1024 * 1024 * 3) {
                this.imageSizeValid = false;
                return false;
            }
            this.imageSizeValid = true;
            return true;
        },
        registerImg: function () {
            UserApi.registerProfileImage(this.image).then(
                this.$router.push({ name: "ProfileEdit" })
            );
        },
    },
    created() {
        UserApi.requestProfile(localStorage.getItem("nickname")).then((res) => {
            this.image = res.data.userProfile.profileImage;
        });
    },
};
</script>

<style scoped>
#body-wrap {
    margin: 90px 20px 0;
}

#img-input {
    visibility: hidden;
}

.profile-img {
    width: 200px;
    height: 200px;
    border-radius: 50%;
}

.img-button {
    background-color: #ff993c;
    color: #333;
    border-radius: 5px;
    padding: 8px 10px;
    margin: 30px auto auto;
    border-radius: 8px;
    background-color: #dbdbdb;
    font-size: 16px;
    font-weight: 500;
}


#register-button {
    width: 150px;
    display: block;
    margin: auto;
    background-color: #ff993c;
    color: white;
    font-weight: 400;   
}
</style>