<template>
    <div class="container mx-auto py-24">
        <!-- Line above the title -->
        <div class="line bg-[#E74C3C] w-[112px] h-[6px] mb-4"></div>
        <!-- Title -->
        <h2 class="text-[24px] font-semibold text-[#E5E5E5] mb-8">Discover Movies</h2>

        <!-- Loading State -->
        <div v-if="loading" class="loading-spinner text-white">Loading...</div>

        <!-- Movie List -->
        <div class="movie-list grid grid-cols-5 gap-6">
            <MovieCard v-for="movie in topMovies" :key="movie.id" :poster="movie" type="movie" />
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import MovieCard from '~/components/PosterCard.vue';

// State to hold the popular movies
const topMovies = ref([]);
const loading = ref(true);

// API Key
const runtimeConfig = useRuntimeConfig();
const apiKey = runtimeConfig.public.tmdbApiKey;

// Fetch top 10 popular movies from TMDB API
const fetchTopMovies = async () => {
    try {
        const response = await fetch(
            `https://api.themoviedb.org/3/movie/popular?api_key=${apiKey}&language=en-US&page=1`
        );
        if (!response.ok) {
            throw new Error('Failed to fetch data');
        }
        const data = await response.json();

        topMovies.value = data.results.slice(0, 10); // Get the top 10 movies
    } catch (error) {
        console.error('Error fetching movies:', error);
    } finally {
        loading.value = false; // Stop loading after fetch
    }
};

onMounted(() => {
    fetchTopMovies();
});
</script>

<style scoped>
.movie-list {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 20px;
}

@media (max-width: 1024px) {
    .movie-list {
        grid-template-columns: repeat(3, 1fr);
        /* 3 columns on medium screens */
    }
}

@media (max-width: 768px) {
    .movie-list {
        grid-template-columns: repeat(2, 1fr);
        /* 2 columns on small screens */
    }
}

@media (max-width: 480px) {
    .movie-list {
        grid-template-columns: 1fr;
        /* 1 column on extra small screens */
    }
}

.loading-spinner {
    text-align: center;
    font-size: 1.5rem;
    color: white;
}
</style>