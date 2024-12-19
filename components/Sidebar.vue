<template>
    <div class="sidebar sticky top-0 h-screen">
        <div class="bg-gradient-to-b from-[#0E1723] to-[#1E232A00] w-11/12 rounded-lg relative z-50">
            <div class="p-4">
                <h3 class="text-md font-semibold mb-2">Sort Results By</h3>

                <!-- Dropdown -->
                <div class="relative">
                    <select class="w-full bg-[#E0E0E021] text-[#E5E5E5] rounded mt-2 p-2 pr-10 relative z-50"
                        @change="handleSortSelection" v-model="sort">
                        <option value="popularity.asc">Popularity Ascending</option>
                        <option value="popularity.desc">Popularity Descending</option>
                        <option value="release_date.asc">Release Date Ascending</option>
                        <option value="release_date.desc">Release Date Descending</option>
                        <option value="vote_average.asc">Rating Ascending</option>
                        <option value="vote_average.desc">Rating Descending</option>
                    </select>
                    <div class="absolute inset-y-0 top-2 right-0 flex items-center pr-2 pointer-events-none">
                        <svg class="w-4 h-4 text-[#E5E5E5]" fill="none" stroke="currentColor"
                            viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M19 9l-7 7-7-7"></path>
                        </svg>
                    </div>
                </div>
            </div>

            <h3 class="text-md font-semibold mt-8 mb-2 px-4">Genres</h3>
            <hr class="border-[#fff] border-opacity-20 mb-4" />
            <!-- Genres Checkbox -->
            <div class="px-4">
                <label class="flex justify-between items-center mb-2" v-for="genre in genres" :key="genre.id">
                    <span class="text-[#E5E5E5]">{{ genre.name }}</span>
                    <input type="checkbox" class="custom-checkbox ml-2" :value="genre.id" v-model="selectedGenres"
                        @change="handleGenreSelection" />
                </label>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
    type: {
        type: String,
        required: true,
        validator: value => ['movie', 'tv'].includes(value)
    },
    sort: String,
    selectedGenres: Array
});

const emit = defineEmits(['update:sort', 'update:selectedGenres']);

const genres = ref([]);
const sort = ref(props.sort || 'popularity.desc');
const selectedGenres = ref(props.selectedGenres || []);

const runtimeConfig = useRuntimeConfig();
const apiKey = runtimeConfig.public.tmdbApiKey;

const fetchGenres = async () => {
    try {
        const response = await fetch(`https://api.themoviedb.org/3/genre/${props.type}/list?api_key=${apiKey}&language=en-US`);
        if (!response.ok) {
            throw new Error('Failed to fetch genres');
        }

        const data = await response.json();
        genres.value = data.genres;
    } catch (error) {
        console.error('Error fetching genres:', error);
    }
};

const handleSortSelection = (event) => {
    emit('update:sort', event.target.value);
};

const handleGenreSelection = () => {
    emit('update:selectedGenres', [...selectedGenres.value]);
};

onMounted(() => {
    fetchGenres();
});
</script>

<style scoped>

select {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
}

.custom-checkbox {
    appearance: none;
    background-color: rgba(255, 255, 255, 0.2);
    border: 1px solid rgba(255, 255, 255, 0.5);
    width: 16px;
    height: 16px;
    cursor: pointer;
    position: relative;
    border-radius: 4px;
}

.custom-checkbox:checked::after {
    content: '✔';
    position: absolute;
    top: 0;
    left: 3px;
    font-size: 10px;
    color: white;
}

.custom-checkbox:checked {
    background-color: #E74C3C;
}
</style>