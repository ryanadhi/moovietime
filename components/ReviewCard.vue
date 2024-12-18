<template>
    <div
        class="review-card-container bg-[#F9F9F9] shadow-md flex flex-col p-4 rounded-2xl gap-4 h-[284px] overflow-hidden">
        <!-- Card title -->
        <div class="flex items-center justify-between">
            <!-- User -->
            <div class="flex items-center">
                <div v-if="review.author_details.avatar_path" class="w-10 h-10 rounded-full overflow-hidden">
                    <img :src="`https://image.tmdb.org/t/p/w500${review.author_details.avatar_path}`" alt="User Avatar"
                        class="w-full h-full object-cover" />
                </div>
                <div v-else class="w-10 h-10 rounded-full bg-[#1E232B36] flex items-center justify-center ">
                    <span class="text-white">{{ review.author.charAt(0).toUpperCase() }}</span>
                </div>
                <div class="flex flex-col">
                    <span class="ml-2 font-semibold">{{ review.author }}</span>
                    <span class="ml-2 text-xs text-[#666666]">{{ new Date(review.created_at).toLocaleDateString('en-US',
                        { year: 'numeric', month: 'long', day: 'numeric' }) }}</span>
                </div>
            </div>
            <!-- Rating -->
            <div class="flex items-start bg-[#C4C4C447] p-2 rounded-lg ">
                <img :src="starIcon" alt="Star Icon" class="h-4" />
                <span class="ml-2 text-3xl font-semibold ">{{ review.author_details.rating.toFixed(1) }}</span>

            </div>

        </div>

        <!-- Card content -->
        <div class="text-black text-sm italic overflow-auto flex-1">
            <span v-if="review.content.length > 700">
            <span v-if="!showFullReview">
                {{ review.content.substring(0, 700) }}...
                <a href="#" class="text-[#FF0000] underline" @click.prevent="showFullReview = !showFullReview">
                read the rest
                </a>
            </span>
            <span v-else>
                {{ review.content }}
                <a href="#" class="text-blue-500" @click.prevent="showFullReview = !showFullReview">
                show less
                </a>
            </span>
            </span>
            <span v-else>
            {{ review.content }}
            </span>
        </div>
    </div>
</template>

<script setup>
defineProps({
    review: Object,
});


import starIcon from '/icons/star.png';

const showFullReview = ref(false);
</script>

<style scoped>
/* No additional CSS needed since we're using Tailwind utilities */
</style>