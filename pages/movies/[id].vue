<template>
    <div class="movie-details-container flex flex-col bg-[#2c2f36] h-full ">
        <!-- Hero Image Section (Fullwidth) -->
        <div class="hero-image w-full min-h-[400px] bg-cover bg-center relative flex flex-col items-end justify-end"
            :style="{
                backgroundImage: movie?.backdrop_path ?
                    `url(https://image.tmdb.org/t/p/w1280${movie.backdrop_path})` :
                    movie?.poster_path ?
                        `url(https://image.tmdb.org/t/p/w1280${movie.poster_path})` :
                        'url(https://via.placeholder.com/1280x400/888888/FFFFFF?text=No+Image)'
            }">
            <!-- Dark overlay on top of the image -->
            <div class="overlay absolute top-0 left-0 w-full h-full bg-black opacity-50"></div>

            <!-- Hero Text (Release Year, Title, Genre) -->
            <div class="hero-text text-white w-full z-10 px-28 flex flex-row py-2">
                <!-- Space for poster -->
                <div class="basis-1/4"></div>
                <div class="basis-3/4">
                    <!-- Release Year -->
                    <p class="text-lg font-normal">{{ movie?.release_date?.split('-')[0] }}</p>

                    <!-- Movie Title -->
                    <h1 class="text-4xl font-semibold mt-2">{{ movie?.title }}</h1>

                    <!-- Movie Genre -->
                    <div class="genres mt-2 text-sm">
                        <span v-for="(genre, index) in movie?.genres" :key="genre.id" class="mr-2">
                            {{ genre.name }}<span v-if="index < movie?.genres.length - 1">,</span>
                        </span>
                    </div>
                </div>
            </div>

            <!-- Hero Movie detail -->
            <div class="info-bar z-10 w-full flex flex-row text-white bg-black bg-opacity-50 px-28 py-2">
                <div class="basis-1/4"></div>
                <div class="basis-3/4 flex items-center justify-between">
                    <!-- Each Item is separated by vertical lines -->
                    <div class="info-item flex items-center justify-center">
                        <img :src="starIcon" alt="Votes" class="h-6" />
                        <span class="text-[#E5E5E5] text-3xl ml-4">{{ movie?.vote_average.toFixed(1) }}</span>
                    </div>
                    <div class="info-item text-xs uppercase flex-1 flex flex-col justify-center border-r border-white">
                        <span class="font-semibold opacity-50 ml-10">User Score</span>
                        <span class="ml-10">{{ movie?.vote_count }} votes</span>
                    </div>
                    <div class="info-item text-xs uppercase flex-1 flex flex-col justify-center border-r border-white">
                        <span class="font-semibold opacity-50 ml-10">Status</span>
                        <span class="ml-10">{{ movie?.status }}</span>
                    </div>
                    <div class="info-item text-xs uppercase flex-1 flex flex-col justify-center border-r border-white">
                        <span class="font-semibold opacity-50 ml-10">Language</span>
                        <span class="ml-10">{{ movie?.spoken_languages?.[0]?.english_name }}</span>
                    </div>
                    <div class="info-item text-xs uppercase flex-1 flex flex-col justify-center border-r border-white">
                        <span class="font-semibold opacity-50 ml-10">Budget</span>
                        <span class="ml-10">{{ movie?.budget ? `$${movie.budget.toLocaleString()}` : 'N/A' }}</span>
                    </div>
                    <div class="info-item text-xs uppercase flex-1 flex flex-col justify-center">
                        <span class="font-semibold opacity-50 ml-10">Production</span>
                        <span class="ml-10">{{ movie?.production_companies?.[0]?.name || 'N/A' }}</span>
                    </div>
                </div>
            </div>
        </div>
        <!-- Movie Overview Section -->
        <div class="overview-section px-28 pt-8 pb-4 flex flex-row bg-white text-black">
            <div class="basis-1/4 -mt-60 z-10 pr-8 py-8">
                <!-- Movie Poster -->
                <img :src="movie?.poster_path ? `https://image.tmdb.org/t/p/w500${movie.poster_path}`
                    : 'https://via.placeholder.com/500x750/888888/FFFFFF?text=No+Image'" alt="Movie Poster"
                    class="w-full rounded-lg" />
            </div>
            <div class="basis-2/4 ">
                <!-- Movie Title -->
                <h2 class="text-[#FF0000] text-md font-semibold uppercase">Overview</h2>
                <p class="mt-2 text-sm ">{{ movie?.overview }}</p>
            </div>
        </div>
        <div class="bg-white text-black px-28 pb-4">
            <!-- Movie reviews -->
            <h2 class="text-[#FF0000] text-md font-semibold mb-4 uppercase ">Reviews</h2>
            <div class="grid grid-cols-2 gap-4">
                <ReviewCard v-for="review in reviews" :key="review.id" :review="review" />
            </div>
        </div>

        <div class=" text-white px-28 py-12">
            <!-- Movie reviews -->
            <h2 class="text-md font-semibold mb-4 uppercase">Recommendation Movies</h2>
            <div class="grid grid-cols-5 gap-4">
                <MovieCard v-for="recommendation in recommendations" :key="recommendation.id" :movie="recommendation" />
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import starIcon from '/icons/star.png';
import ReviewCard from '~/components/ReviewCard.vue';
import MovieCard from '~/components/MovieCard.vue';

const movie = ref(null);
const reviews = ref([]);
const recommendations = ref([]);
const route = useRoute();
const movieId = route.params.id;

const runtimeConfig = useRuntimeConfig();
const apiKey = runtimeConfig.public.tmdbApiKey;

onMounted(async () => {
    try {
        // Fetch movie details
        const movieResponse = await fetch(`https://api.themoviedb.org/3/movie/${movieId}?api_key=${apiKey}`);
        if (!movieResponse.ok) throw new Error('Failed to fetch movie details');
        movie.value = await movieResponse.json();

        // Fetch Reviews
        const reviewsResponse = await fetch(`https://api.themoviedb.org/3/movie/${movieId}/reviews?api_key=${apiKey}`);
        if (!reviewsResponse.ok) throw new Error('Failed to fetch reviews');
        const reviewsData = await reviewsResponse.json();
        reviews.value = reviewsData.results.slice(0, 2); // Limit to 2 reviews

        // Fetch Recommendations
        const recommendationsResponse = await fetch(`https://api.themoviedb.org/3/movie/${movieId}/recommendations?api_key=${apiKey}`);
        if (!recommendationsResponse.ok) throw new Error('Failed to fetch recommendations');
        const recommendationsData = await recommendationsResponse.json();
        recommendations.value = recommendationsData.results.slice(0, 5); // Limit to 5 recommendations
    } catch (error) {
        console.error(error.message);
    }
});
</script>

<style scoped>
/* No additional CSS needed since we're using Tailwind utilities */
</style>