<script setup lang="ts">
import { onMounted, ref } from 'vue';
import constant from '@/stores/constant';
import axios from "axios"
import { useRouter } from 'vue-router';

const props = defineProps({
    postId: {
        type: [Number, String],
        require: true,
    }
});

const post = ref({
    id: 0,
    title: "",
    content: "",
});

const router = useRouter();

const moveToEdit = function () {
    router.push({ name: "edit", params: { postId: props.postId } })
};

const deletePost = function () {
    axios.delete(constant.J_LOG_POST_API + `/${props.postId}`).then(() => {
        alert("게시글이 삭제되었습니다.");
        router.replace({ name: "home" });
    });
};

onMounted(() => {
    axios.get(constant.J_LOG_POST_API + `/${props.postId}`).then((response) => {
        post.value = response.data;
    });
});

</script>

<template>
    <el-row class="mt-3">
        <el-col>
            <h2 class="title">{{ post.title }}</h2>

            <div class="sub">
                <div class="sub d-flex">
                    <div class="category">개발</div>
                    <div class="regDate">2023-08-25</div>
                </div>
            </div>
        </el-col>
    </el-row>

    <el-row class="mt-3">
        <el-col>
            <div class="content">{{ post.content }}</div>
        </el-col>
    </el-row>

    <el-row>
        <el-col class="d-flex justify-content-end mt-3">
            <el-button type="warning" @click="moveToEdit()">수정</el-button>
            <el-button type="danger" @click="deletePost()">삭제</el-button>
        </el-col>
    </el-row>
</template>

<style scoped lang="scss">
.title {
    margin-top: -1rem;
    font-size: 1.8rem;
    font-weight: 700;
    color: #575653;
}

.content {
    margin-top: 1rem;
    font-size: 1.1rem;
    color: #515351;
    white-space: break-spaces;
}

.sub {
    margin-top: 2px;
    font-size: 0.8rem;

    .regDate {
        margin-left: 5px;
    }
}
</style>