<template>
    <NuxtLink :to="`/${type === 'movie' ? 'movies' : 'tvshows'}/${poster.id}`">
        <div class="poster-card overflow-hidden text-white w-[220px] mb-4 relative">
            <div class="relative">
                <!-- Movie Image with fallback to gray poster -->
                <img :src="poster.poster_path ? `https://image.tmdb.org/t/p/w220_and_h330_bestv2${poster.poster_path}` : ''"
                    alt="Movie Image" class="poster-image w-full h-[330px] object-cover" />
                <div v-if="!poster.poster_path" class="gray-poster">
                    <span class="poster-text">Poster</span>
                </div>

                <!-- Rating -->
                <div
                    class="rating absolute top-0 right-0 bg-[#1E232BCC] text-[#E5E5E5] text-[18px] py-1 px-2 font-bold">
                    {{ poster.vote_average ? poster.vote_average.toFixed(1) : 'N/A' }}
                </div>

                <!-- Overlay with View Button -->
                <div
                    class="overlay absolute inset-0 bg-black flex flex-col items-center justify-center opacity-0 transition-opacity duration-300 gap-4">
                    <div class="flex items-center" v-if="poster.vote_average">
                        <img :src="starIcon" alt="Star Icon" class="h-6" />
                        <span class="ml-2 text-3xl font-semibold "> {{ poster.vote_average ?
                            poster.vote_average.toFixed(1) : 'N/A' }}</span>
                    </div>
                    <button class="view-button bg-[#FF0000] text-white px-4 py-2 rounded-3xl">View</button>
                </div>
            </div>

            <!-- Movie Title -->
            <h3 class="poster-title text-[16px] text-[#e5e5e5] mt-2 font-semibold ">{{ title || 'No Title' }}</h3>
            <!-- Release Year -->
            <p class="release-year text-[14px] text-[#929292] mt-1 font-normal ">
                {{ releaseDate ? releaseDate.split('-')[0] : 'Unknown' }}
            </p>
        </div>
    </NuxtLink>
</template>

<script setup>
const props = defineProps({
    poster: {
        type: Object,
        required: true,
        validator: (value) => {
            return (value && value.id && value.title && value.poster_path !== undefined) ||
                (value && value.id && value.name && value.poster_path !== undefined);
        }
    },
    type: {
        type: String,
        required: true,
        validator: (value) => ['movie', 'tv'].includes(value)
    }
});

import starIcon from '/icons/star.png';

const title = computed(() => {
    return props.type === 'movie' ? props.poster.title : props.poster.name;
});

const releaseDate = computed(() => {
    return props.type === 'movie' ? props.poster.release_date : props.poster.first_air_date;
});
</script>

<style scoped>
.gray-poster {
    width: 100%;
    height: 330px;
    background-color: #888;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-size: 18px;
    font-weight: bold;
    text-align: center;
}

.poster-card {
    position: relative;
}

.poster-card:hover .overlay {
    opacity: 1;
}
</style>