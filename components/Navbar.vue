<template>
    <nav class="navbar z-50  absolute top-0 left-0 w-full h-16 bg-white bg-opacity-10 ">
        <div class="container mx-auto flex justify-between items-center h-full">
            <!-- Logo -->
            <NuxtLink to="/">
                <img src="/logo.png" alt="MovieDB Logo" class="h-8 " />
            </NuxtLink>

            <!-- Search Field -->
            <div class="flex-1 mx-6 relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                    <img src="/icons/movie-icon.png" alt="Movie Icon" class="h-5 w-5" />
                </div>
                <input type="text" placeholder="Find Movie" v-model="query" @input="handleSearch"
                    class="search-input w-full pl-10 pr-10 py-2 rounded bg-gray-800 text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500" />
                <div class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                    <img src="/icons/search.png" alt="Search Icon" class="h-5 w-5" />
                </div>
                <div v-if="searchResults.length"
                    class="absolute left-0 right-0 mt-2 bg-black text-white rounded shadow-lg z-10">
                    <ul>
                        <li v-for="result in searchResults" :key="result.id"
                            class="px-4 py-2 hover:bg-gray-700 cursor-pointer" @click="selectResult(result)">
                            <span v-html="highlightMatch(result.title || result.name)"></span>
                        </li>
                    </ul>
                </div>
            </div>

            <!-- Navigation Links -->
            <ul class="flex space-x-6">
                <li class="relative group">
                    <div class="flex items-center space-x-2 cursor-pointer">
                        <img src="/icons/category.png" alt="Categories Icon" class="h-5 w-5" />
                        <span class="link-style">Categories</span>
                    </div>
                    <ul class="absolute left-0 mt-2 w-48 bg-white text-black rounded shadow-lg opacity-0 group-hover:opacity-100 transition-opacity duration-300">
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Action</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Adventure</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Animation</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Comedy</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Crime</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Documentary</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Drama</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Family</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Fantasy</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">History</li>
                        <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer uppercase text-sm">Horror</li>
                    </ul>
                </li>
                <li>
                    <NuxtLink to="/movies" class="hover:underline">Movies</NuxtLink>
                </li>
                <li>
                    <NuxtLink to="/tvshows" class="hover:underline">TV Shows</NuxtLink>
                </li>
                <li><span class="link-style">Login</span></li>
            </ul>
        </div>
    </nav>
</template>

<script setup>
import { useRouter } from 'vue-router';

const query = ref('');
const router = useRouter();
const searchResults = ref([]);

const runtimeConfig = useRuntimeConfig();
const apiKey = runtimeConfig.public.tmdbApiKey;

const handleSearch = async () => {
    if (query.value.trim() === '') {
        searchResults.value = [];
        return;
    }

    try {
        const response = await fetch(`https://api.themoviedb.org/3/search/multi?api_key=${apiKey}&query=${query.value}`);
        if (!response.ok) throw new Error('Failed to fetch search results');
        const data = await response.json();
        searchResults.value = data.results.slice(0, 5); // Limit to 5 results
    } catch (error) {
        console.error(error.message);
    }
};

const selectResult = (result) => {
    router.push({
        path: result.media_type === 'movie' ? `/movies/${result.id}` : `/tvshows/${result.id}`
    })


    query.value = '';
    searchResults.value = [];
};
const highlightMatch = (text) => {
    const regex = new RegExp(`(${query.value})`, 'gi');
    return text.replace(regex, '<span class="highlight" style="font-weight: 600">$1</span>');
};

</script>

<style scoped>
.navbar a,
.link-style {
    color: white;
    font-weight: 500;
}

.navbar a:hover,
.link-style:hover {
    text-decoration: underline;
}

.search-input {
    border: 1px solid rgba(255, 255, 255, 0.2);
}

.search-input:focus {
    border-color: rgba(255, 255, 255, 0.5);
}

.search-input::placeholder {
    color: rgba(255, 255, 255, 0.5);
}


.search-results {
    background-color: #2d2d2d;
    border: 1px solid #444;
    border-radius: 4px;
    margin-top: 5px;
    max-height: 200px;
    overflow-y: auto;
    position: absolute;
    width: 100%;
    z-index: 10;
}

.search-results li {
    padding: 10px;
    cursor: pointer;
}
</style>