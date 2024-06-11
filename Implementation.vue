<script setup>
import VideoPlayer from '../../Components/Web/VideoPlayer.vue';
import { usePage } from '@inertiajs/vue3';
</script>

<template>
    <div class="">
        <div class="container">
            <VideoPlayer
               :thumb="usePage().props.assetURL ? usePage().props.assetURL + '/web-assets/WhyChoose-Us-Video-Image-1.png' : '../../web-assets/WhyChoose-Us-Video-Image-1.png'"
               :videoType="'youtube'"
               :video="'zqBHTbNSWcY'"
               :removemargin = "'removemargin'"
            />
        </div>
    </div>
</template>
