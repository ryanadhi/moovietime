<template>
    <div class="container mx-auto py-24">
        <div class="absolute top-16 left-0 w-full h-[320px] bg-white opacity-10"></div>

        <div class="z-50 relative">
            <div class="line bg-[#E74C3C] w-[112px] h-[6px] mb-4"></div>
            <h2 class="text-[36px] font-semibold text-[#E5E5E5] mb-8">TV Shows</h2>
        </div>

        <div class="container flex relative z-50">
            <div class="basis-1/4">
                <Sidebar :sort="sort" :selectedGenres="selectedGenres" @update:sort="handleSortSelection"
                    @update:selectedGenres="handleGenreSelection" type="tv" />
            </div>
            <div class="content basis-3/4 flex  flex-col items-center">
                <div class="content-list grid grid-cols-4 gap-6">
                    <TvShowCard v-for="tvshow in tvShows" :key="tvshow.id" :poster="tvshow" type="tv" />
                </div>
                <div v-if="loading" class="text-[#E5E5E5] text-center mt-8">Loading...</div>
                <div v-else-if="!tvShows.length" class="text-[#E5E5E5] text-center mt-8">No TV shows found</div>
                <button v-else @click="handleLoadMore"
                    class="load-more-button mt-8 bg-[#ff0000] text-white px-4 py-2 rounded-3xl text-sm font-semibold w-36">
                    Load More
                </button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import TvShowCard from '~/components/PosterCard.vue';
import Sidebar from '~/components/Sidebar.vue';

const tvShows = ref([]);
const loading = ref(true);
const page = ref(1);
const sort = ref('popularity.desc');
const selectedGenres = ref([]);

// API Key
const runtimeConfig = useRuntimeConfig();
const apiKey = runtimeConfig.public.tmdbApiKey;

const fetchTvShows = async () => {
    try {
        const genreQuery = selectedGenres.value.length ? `&with_genres=${selectedGenres.value.join(',')}` : '';
        const response = await fetch(
            `https://api.themoviedb.org/3/discover/tv?api_key=${apiKey}&language=en-US&sort_by=${sort.value}&page=${page.value}${genreQuery}`
        );
        if (!response.ok) {
            throw new Error('Failed to fetch data');
        }
        const data = await response.json();

        tvShows.value = [...tvShows.value, ...data.results];
    } catch (error) {
        console.error('Error fetching TV Shows:', error);
    } finally {
        loading.value = false;
    }
};

const handleSortSelection = (value) => {
    sort.value = value;
    movies.value = [];
    page.value = 1;
    loading.value = true;
    fetchMovies();
};

const handleGenreSelection = (newSelectedGenres) => {
    selectedGenres.value = newSelectedGenres;
    movies.value = [];
    page.value = 1;
    loading.value = true;
    fetchMovies();
};

const handleLoadMore = () => {
    page.value++;
    fetchTvShows();
};

onMounted(() => {
    fetchTvShows();
});
</script>