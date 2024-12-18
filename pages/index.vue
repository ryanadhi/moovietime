<template>
    <div class="container mx-auto py-24">

        <!-- Now Playing Carousel -->
        <div v-if="nowPlaying.length" class="now-playing-carousel mb-8">
            <carousel v-bind="config">
                <slide v-for="slide in nowPlaying" :key="slide">
                    <CarouselCard :card="slide" />
                </slide>

                <template #addons>
                    <navigation />
                    <pagination />
                </template>
            </carousel>
        </div>

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
import CarouselCard from '~/components/CarouselCard.vue';
import 'vue3-carousel/dist/carousel.css'
import { Carousel, Slide, Pagination, Navigation } from 'vue3-carousel'

// State to hold the popular movies
const topMovies = ref([]);
const loading = ref(true);
const nowPlaying = ref([]);

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

        topMovies.value = data.results.slice(0, 10);
    } catch (error) {
        console.error('Error fetching movies:', error);
    } finally {
        loading.value = false; // Stop loading after fetch
    }
};


// Fetch now playing movies from TMDB API
const fetchNowPlaying = async () => {
    try {
        const response = await fetch(
            `https://api.themoviedb.org/3/movie/now_playing?api_key=${apiKey}&language=en-US&page=1`
        );
        if (!response.ok) {
            throw new Error('Failed to fetch data');
        }
        const data = await response.json();
        nowPlaying.value = data.results.slice(0, 5);
    } catch (error) {
        console.error('Error fetching now playing movies:', error);
    }
};

// Carousel config
const config = {
    autoplay: 5000,
    wrapAround: true,
    pauseAutoplayOnHover: true,
    itemsToShow: 2,
};

onMounted(() => {
    fetchNowPlaying();
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

.carousel__slide {
    padding: 5;
}

.carousel__viewport {
    perspective: 2000px;
}

.carousel__track {
    transform-style: preserve-3d;
}

.carousel__slide--sliding {
    transition: 0.5s;
}

.carousel__slide {
    opacity: 0.9;
    transform: rotateY(-20deg) scale(0.9);
}

.carousel__slide--active~.carousel__slide {
    transform: rotateY(20deg) scale(0.9);
}

.carousel__slide--prev {
    opacity: 0.5;
    transform: rotateY(-10deg) scale(0.8);
}

.carousel__slide.carousel__slide--next {
    opacity: 0.5;
    transform: rotateY(10deg) scale(0.8);
}

.carousel__slide--active {
    opacity: 1;
    transform: rotateY(0) scale(1);
}

:deep(.carousel__pagination-button::after) {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background-color: white;
}

/* active  */
:deep(.carousel__pagination-button--active::after) {
    background-color: #E74C3C;
    width: 48px;
    border-radius: 6px;
    transition: all 0.3s ease-in-out;
    transform: scale(1.2);
}
</style>