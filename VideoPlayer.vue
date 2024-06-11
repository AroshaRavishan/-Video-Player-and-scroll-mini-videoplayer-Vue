<script setup>
import {
    ref,
    defineProps,
    onMounted,
    watchEffect
} from 'vue';

const videoRef = ref(null);
const scrollVideoRef = ref(null);
const showVideo = ref(false);
const showVideoScroll = ref(false);

const props = defineProps({
    thumb: String,
    thumbWidth: Number,
    thumbHeight: Number,
    thumbAlt: String,
    videoWidth: Number,
    videoHeight: Number,
    removemargin: {
        type: String,
        default: null,
    },
    imgSec: {
        type: Boolean,
        default: true,
    },
    videoType: {
        type: String,
        default: 'html5', // Default to HTML5 video
    },
    video: String,
});

watchEffect(() => {
    if (videoRef.value) {
        videoRef.value.play();
    }
});

const playVideo = () => {
    if (props.videoType === 'html5' && videoRef.value) {
        videoRef.value.play().catch(() => {
            // Handle play error
        });
    }
};

const stopVideo = () => {
    showVideo.value = false;
    if (props.videoType === 'html5' && videoRef.value) {
        videoRef.value.pause();
        videoRef.value.currentTime = 0;
    }
};

const stopScrollVideo = () => {
    showVideoScroll.value = false;
    // Stop and reset the scroll video
    if (scrollVideoRef.value) {
        scrollVideoRef.value.pause();
        scrollVideoRef.value.currentTime = 0;
    }
    // Pause the main video
    stopVideo();
};

onMounted(() => {
    // Auto-play the video when the component is mounted
    playVideo();
    // Check scroll position on mount
    checkScrollPosition();
    // Listen for scroll events
    window.addEventListener("scroll", checkScrollPosition);
});

const checkScrollPosition = () => {
    const mainVideoContainer = document.querySelector(".video-container");
    const mainVideoPlayer = document.querySelector(".main-video-player");

    if (mainVideoContainer && mainVideoPlayer) {
        const mainVideoContainerRect = mainVideoContainer.getBoundingClientRect();
        const mainVideoPlayerRect = mainVideoPlayer.getBoundingClientRect();

        // Check if the main video player is not visible on the screen
        if (
            mainVideoPlayerRect.bottom < 0 ||
            mainVideoPlayerRect.top > window.innerHeight
        ) {
            showVideoScroll.value = true;
        } else {
            showVideoScroll.value = false;
        }
    }
};


// Set hardcoded data for the props
props.thumbWidth = 877;
props.thumbHeight = 500;
props.thumbAlt = 'Thumbnail Alt Text';
props.videoWidth = 877;
props.videoHeight = 500;
</script>


<template>
    <div class="bg-white video-container container video" :class="props.removemargin ? 'pt-10' : 'mt-10 md:mt-30'">
        <div v-if="!showVideo" class="relative flex justify-center items-center focus:outline-none focus-visible:ring focus-visible:ring-indigo-300 rounded-3xl group" @click="showVideo = true" aria-label="Watch the video">
            <img loading="lazy" class="rounded-3 shadow-2xl transition-shadow duration-300 ease-in-out object-cover" :src="props.thumb" :style="{ width: props.thumbWidth + 'px', maxHeight: props.thumbHeight + 'px' }" :alt="props.thumbAlt" />
            <!-- Play icon -->
            <svg class="waves-block absolute pointer-events-none group-hover:scale-110 transition-transform duration-300 ease-in-out" xmlns="http://www.w3.org/2000/svg" width="72" height="72">
                <circle class="fill-white" cx="36" cy="36" r="36" fill-opacity=".8" />
                <path class="fill-primary" d="M44 36a.999.999 0 0 0-.427-.82l-10-7A1 1 0 0 0 32 29V43a.999.999 0 0 0 1.573.82l10-7A.995.995 0 0 0 44 36V36c0 .001 0 .001 0 0Z" />
            </svg>
            <div class="waves wave-1"></div>
            <div class="waves wave-2"></div>
            <div class="waves wave-3"></div>
        </div>

        <div v-if="showVideo">
            <div class="main-video-player">
                <div class="flex justify-center">
                    <!-- Video player -->
                    <div v-if="props.videoType === 'html5'" class="relative">
                        <video ref="videoRef" loop controls autoplay :style="{ width: props.videoWidth + 'px', maxHeight: props.videoHeight + 'px' }">
                            <source :src="props.video" type="video/mp4" />
                            Your browser does not support the video tag.
                        </video>
                        <span class="absolute top-2 right-2 cursor-pointer bg-opacity-50 text-white px-2 py-1 rounded-full bg-black-900 flex items-center justify-center w-9 h-9" @click="stopVideo()"> <i class="fa fa-times" aria-hidden="true"></i></span>
                    </div>

                    <div v-else-if="props.videoType === 'youtube'" class="relative youtube-play">
                        <!-- YouTube iframe player -->
                        <iframe class="player" width="877" height="500" :src="'https://www.youtube.com/embed/' + props.video" frameborder="0" allowfullscreen></iframe>
                        <span class="absolute top-2 right-2 cursor-pointer bg-opacity-50 text-white px-2 py-1 rounded-full bg-black-900 flex items-center justify-center w-9 h-9" @click="stopVideo()"> <i class="fa fa-times" aria-hidden="true"></i></span>
                    </div>
                    <!-- End: Video player -->
                </div>
            </div>
        </div>
        <!--scroll-video-container-->
        <div v-if="showVideoScroll" class="scroll-video-container relative">
            <div v-if="props.videoType === 'html5'">
                <video ref="scrollVideoRef" loop controls autoplay :style="{ width: props.videoWidth + 'px', maxHeight: props.videoHeight + 'px' }">
                    <source :src="props.video" type="video/mp4" />
                    Your browser does not support the video tag.
                </video>
                <span class="absolute top-2 right-2 cursor-pointer bg-opacity-50 text-white px-2 py-1 rounded-full bg-black-900 flex items-center justify-center w-9 h-9" @click="stopScrollVideo()">
                    <i class="fa fa-times" aria-hidden="true"></i>
                </span>
            </div>
            <div v-else-if="props.videoType === 'youtube'" class="relative">
                <!-- YouTube iframe player -->
                <iframe class="player-responsive" width="877" height="500" :src="'https://www.youtube.com/embed/' + props.video" frameborder="0" allowfullscreen></iframe>
                <span class="absolute top-2 right-2 cursor-pointer bg-opacity-50 text-white px-2 py-1 rounded-full bg-black-900 flex items-center justify-center w-9 h-9" @click="stopScrollVideo()">
                    <i class="fa fa-times" aria-hidden="true"></i>
                </span>
            </div>
        </div>
    </div>
</template>

<style scoped>
.scroll-video-container {
    position: fixed;
    bottom: 0;
    right: 0;
    margin-bottom: 10px;
    margin-right: 10px;
    transition: opacity 0.5s ease-in-out;
    max-width: 300px;
    z-index: 1;
}

.scroll-video-container.show {
    opacity: 1;
}

/* Additional styles for the video element inside the container */
.scroll-video-container video {
    max-width: 100%;
    /* Make the video responsive within the container *
    border-radius: 8px; /* Optional: Add border-radius to the video element */
}

.scroll-video-container.relative .player-responsive{
    width: 354px !important;
    height: 200px !important;
    margin-left: -60px;
    position: relative;
    border-radius: 12px;
}

video ,.player {
    border-radius: 12px;
}


@keyframes waves {
    0% {
        -webkit-transform: scale(0.2, 0.2);
        transform: scale(0.2, 0.2);
        opacity: 0;
        -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
    }

    50% {
        opacity: 0.9;
        -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=90)";
    }

    100% {
        -webkit-transform: scale(0.9, 0.9);
        transform: scale(0.9, 0.9);
        opacity: 0;
        -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
    }
}

.waves {
    position: absolute;
    width: 150px;
    height: 150px;
    background: rgba(238, 238, 238, 0.3);
    opacity: 0;
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
    border-radius: 100%;
    z-index: 10;
    -webkit-animation: waves 3s ease-in-out infinite;
    animation: waves 3s ease-in-out infinite;
}

.wave-1 {
    -webkit-animation-delay: 0s;
    animation-delay: 0s;
}

.wave-2 {
    -webkit-animation-delay: 1s;
    animation-delay: 1s;
}

.wave-3 {
    -webkit-animation-delay: 2s;
    animation-delay: 2s;
}

@media screen and (max-width: 991px) {
    .player{
        width: 100%;
        position: absolute;
        height: 100%;
        overflow: hidden;
    }

    .relative.youtube-play {
        width: 100%;
        padding-bottom: 56%;
    }
}

</style>
