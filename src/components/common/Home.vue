<template>
    <div class="wrapper">
        <v-head></v-head>
        <v-sidebar></v-sidebar>
        <div class="content-box" :class="{'content-collapse':collapse}">
            <v-tags></v-tags>
            <div class="content" id="test">
                <happy-scroll color="rgba(0,0,0,.5)" resize>
                    <transition name="move" mode="out-in">
                        <router-view></router-view>
                    </transition>
                </happy-scroll>
            </div>
        </div>
    </div>
</template>
<script>
import vHead from './Header.vue';
import vSidebar from './Sidebar.vue';
import vTags from './Tags.vue';
import bus from './bus';
export default {
    data() {
        return {
            tagsList: [],
            collapse: false
        }
    },
    components: {
        vHead,
        vSidebar,
        vTags
    },
    created() {
        bus.$on('collapse', msg => {
            this.collapse = msg;
        })

        // 只有在标签页列表里的页面才使用keep-alive，即关闭标签之后就不保存到内存中了。
        bus.$on('tags', msg => {
            let arr = [];
            for (let i = 0, len = msg.length; i < len; i++) {
                msg[i].name && arr.push(msg[i].name);
            }
            this.tagsList = arr;
        });
    }
};

</script>
<style>
.content .happy-scroll {
    padding: 20px 0 20px
}

.content .happy-scroll-container {
    width: 100% !important;
    height: 100% !important;
}

.content .happy-scroll-content {
    padding: 0 0 0 20px;
}

</style>
