<template>
    <section class="p-article_nextprev">
        <div class="row">
            <div class="col-6 text-left item" v-if="link_prev" @click="goTo(link_prev.url)">
                <div class="row">
                    <div class="col-auto p-article_nextprev-arrow"><i aria-hidden="true" class="icon mdi mdi-chevron-left"></i></div>
                    <div class="col-3 thumb" v-if="link_prev.img" :style="{backgroundImage: 'url(' + link_prev.img + '?width=200)' }"></div>
                    <div class="col"><span class="link">{{ link_prev.title }}</span></div>
                </div>
            </div>
            <div class="col-6 text-right item" v-if="link_next" @click="goTo(link_next.url)">
                <div class="row">
                    <div class="col"><span class="link">{{ link_next.title }}</span></div>
                    <div class="col-3 thumb" v-if="link_next.img" :style="{backgroundImage: 'url(' + link_next.img + '?width=200)' }"></div>
                    <div class="col-auto p-article_nextprev-arrow"><i aria-hidden="true" class="icon mdi mdi-chevron-right"></i></div>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
const props = defineProps(['nextPrevContent', 'path', 'apiContent']);
const data = props.nextPrevContent;
const pathProps = ref(props.path);
var path = pathProps.value;
const apiContent = props.apiContent;
var link_next, link_prev;

for (const key in data.list) {
    var item = data.list[key];
    let url = '';
    let category = '';
    let thumb;
    
    if (item.contents_type_slug) {
        category = item.contents_type_slug + '/';
    };
    if (path.substr(path.length - 1) !== '/') {
        path += '/';
    };
    if (item.slug) {
        url = path + category + item.slug;
    } else {
        url = path + category + item.topics_id;
    };
    if (Array.isArray(item.ext_1)) {
        thumb = item.ext_2;
    } else {
        thumb = item.ext_1;
    };
    let topicID = apiContent.topics_id ? apiContent.topics_id.toString() : "";
    let topicSlug= apiContent.topic_slug ? apiContent.topic_slug.toString() : "";
    if (item.topics_id.toString() !== topicID && item.slug.toString() !== topicSlug) {
        if (!link_next && key == 0) {
            let container = {};
            container.title = item.subject;
            container.id = item.topics_id;
            container.img = thumb;
            container.url = url;
            link_next = container;
        } else {
            let container = {};
            container.title = item.subject;
            container.id = item.topics_id;
            container.img = thumb;
            container.url = url;
            link_prev = container;
        }
    }
};


//Link function
import { useRouter } from 'vue-router';
const router = useRouter();
const goTo = (url) => {
    // window.location.href = url;
    router.push(url);
};
</script>
