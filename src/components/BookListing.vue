<template>
    <div>
        <router-link :to="`/books/${book.id}`">
            <img :src="`http://localhost:8000/img/${book.image}`" alt="cover image" class="h-64 mb-2">
        </router-link>
        <router-link :to="`/books/${book.id}`" class="text-lg font-bold text-grey-light hover:text-white mb-1 block">{{book.title}}</router-link>
        <div class="text-grey-dark">{{book.author}}</div>
    </div>
</template>

<script>
    export default {
        name: "BookListing",
        props: ['book']
    }
</script>

<style scoped>

</style>