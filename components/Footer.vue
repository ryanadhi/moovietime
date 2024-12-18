<template>
    <footer class="footer py-6">
        <div class="container mx-auto flex justify-between items-center">
            <!-- Copyright -->
            <p class="text-gray-400">© 2021 MoovieTime. All rights reserved.</p>

            <!-- Logo -->
            <img src="/logo-grey.png" alt="MovieDB Logo" class="footer-logo" />

            <!-- Nuxt Logo -->
            <div class="flex items-center space-x-2">
                <span class="text-gray-400">Made with</span>
                <img src="/nuxt-logo.png" alt="Nuxt.js Logo" class="footer-logo" />
            </div>
        </div>
    </footer>
</template>

<style scoped>
.footer {
    background-color: rgba(0, 0, 0, 0.25);
    /* Black with 25% opacity */
    color: white;
    margin-top: auto;
}

.footer p {
    font-size: 14px;
}

.footer-logo {
    height: 24px;
    /* Set all footer images to 24px height */
}

.footer img {
    display: block;
}
</style>