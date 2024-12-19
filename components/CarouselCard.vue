<template>
    <div class="carousel-card h-full">
        <NuxtLink :to="`/movies/${card.id}`">

            <div class="flex justify-center items-center h-full">
                <div class="w-1/3">
                    <img :src="`https://image.tmdb.org/t/p/w500${card.poster_path}`" alt="Movie Poster"
                        class="w-full h-full object-cover" />
                </div>
                <div class="w-2/3 p-4 bg-black min-h-[80%] text-left flex flex-col gap-4">
                    <div class="flex items-center" v-if="card.vote_average">
                        <img :src="starIcon" alt="Star Icon" class="h-6" />
                        <span class="ml-2 text-3xl font-semibold ">{{ card.vote_average.toFixed(1) }}</span>
                    </div>
                    <h3 class="text-2xl font-semibold">{{ card.title }}</h3>
                    <p class="text-sm">{{ new Date(card.release_date).getFullYear() }}</p>
                    <p class="text-sm">{{ card.overview }}</p>
                </div>
            </div>
        </NuxtLink>
    </div>

</template>

<script setup>
defineProps({
    card: Object,
});

import starIcon from '/icons/star.png';
</script>