<template>
    <nav class="navbar px-6 py-4">
        <div class="container mx-auto flex justify-between items-center">
            <!-- Logo -->
            <NuxtLink to="/">
                <img src="/logo.png" alt="MovieDB Logo" class="h-10" />
            </NuxtLink>

            <!-- Search Field -->
            <div class="flex-1 mx-6 relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                    <img src="/icons/movie-icon.png" alt="Movie Icon" class="h-5 w-5" />
                </div>
                <input type="text" placeholder="Find Movie"
                    class="search-input w-full pl-10 pr-10 py-2 rounded bg-gray-800 text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500" />
                <div class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                    <img src="/icons/search.png" alt="Search Icon" class="h-5 w-5" />
                </div>
            </div>

            <!-- Navigation Links -->
            <ul class="flex space-x-6">
                <!-- Categories with Icon -->
                <li class="flex items-center space-x-2">
                    <img src="/icons/category.png" alt="Categories Icon" class="h-5 w-5" />
                    <span class="link-style">Categories</span>
                </li>
                <li>
                    <NuxtLink to="/movies" class="hover:underline">Movies</NuxtLink>
                </li>
                <li>
                    <NuxtLink to="/tv-shows" class="hover:underline">TV Shows</NuxtLink>
                </li>
                <li><span class="link-style">Login</span></li>
            </ul>
        </div>
    </nav>
</template>

<style scoped>
.navbar {
    background-color: rgba(255, 255, 255, 0.05);
    /* White with 5% opacity */
    color: white;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    /* Optional shadow */
}

.navbar a,
.link-style {
    color: white;
    /* Ensure links and plain text are styled the same */
    font-weight: 500;
    cursor: pointer;
    /* Add pointer cursor for plain text to simulate a link */
}

.navbar a:hover,
.link-style:hover {
    text-decoration: underline;
    /* Add hover effect for plain text and links */
}

.search-input {
    border: 1px solid rgba(255, 255, 255, 0.2);
    /* Subtle border */
    transition: border-color 0.3s ease;
    background-color: #1E232B;
    /* Match overall background color */
}

.search-input:focus {
    border-color: rgba(255, 255, 255, 0.5);
}

.search-input::placeholder {
    color: rgba(255, 255, 255, 0.5);
    /* Placeholder color */
}
</style>