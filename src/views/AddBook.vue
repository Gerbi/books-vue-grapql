<template>
    <div class="create max-w-sm mx-auto mt-6">
            <h1 align="center" class="text-grey-light">Create Book</h1><br>
            <form method="POST" class="bg-dark shadow-md rounded px-8 pt-4 pb-8 mb-4" @submit.prevent="addBook">
                <div class="mb-5">
                    <label class="text-grey font-bold mb-2" for="title">Title</label>
                    <input type="text" id="title" name="title" placeholder="Title" class="bg-grey-darkest text-grey-lighter w-full py-2" v-model="title">
                </div>
                <div class="mb-5">
                    <label class="text-grey font-bold mb-2" for="title">Author</label>
                    <input type="text" id="author" name="author" placeholder="Author" class="bg-grey-darkest text-grey-lighter w-full" v-model="author">
                </div>
                <div class="mb-5">
                    <label class="text-grey font-bold mb-2" for="image">Image</label>
                    <input type="text" id="image" name="image" placeholder="Image" class="bg-grey-darkest text-grey-lighter w-full" v-model="image">
                </div>
                <div class="mb-5">
                    <label class="font-bold mb-2" for="description">Description</label>
                    <textarea name="description" id="description" class="bg-grey-darkest text-grey-lighter w-full" rows="8" v-model="description"></textarea>
                </div>
                <div class="mb-5">
                    <label class="font-bold mb-2" for="link">Link</label>
                    <input type="text" name="link" id="link" class="bg-grey-darkest text-grey-lighter w-full" placeholder="Link" v-model="link">
                </div>
                <div class="mb-5">
                    <label class="font-bold mb-2"><input type="checkbox" name="featured" v-model="featured" class="mr-2">Featured</label>
                </div>
                <div class="mb-5">
                    <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
                        <template slot-scope="{ result: { data, loading }, isLoading }">
                            <div v-if="isLoading">Loading...</div>
                            <select v-else v-model="category" class="bg-dark pt-2 pb-1 pl-2 rounded text-grey-dark w-full">
                                <option class="text-grey-light border border-none" v-for="category of data.categories" :key="category.id" :value="category.id">
                            {{ category.name }}
                            </option>
                            </select>
                        </template>
                    </ApolloQuery>
                </div>
                <div class="flex items-center justify-end">
                    <button class="bg-dark text-grey font-bold py-2 px-4 mr-4 rounded focus:outline-none focus:shadow-outline" type="button">
                        Cancel
                    </button>
                    <button type="submit" class="bg-grey hover:bg-grey-lightest text-grey-darkest font-bold py-2 px-4 rounded">Add Book</button>
                </div>
            </form>
            <p class="text-center text-grey text-xs mb-4">
                ©2019 GerbiDev. All rights reserved.
            </p>
        </div>
</template>

<script>
    import addBook from '@/graphql/mutations/AddBook.gql'

    export default {

        data() {
            return {
                title: '',
                author: '',
                image: '',
                description: '',
                link: '',
                featured: false,
                category: 1
            }
        },
        methods: {
            addBook() {
                this.$apollo.mutate({
                    // Query
                    mutation: addBook,
                    // Parameters
                    variables: {
                        title: this.title,
                        author: this.author,
                        image: this.image,
                        link: this.link,
                        description: this.description,
                        featured: this.featured,
                        category: this.category
                    }
                }).then((data) => {
                    console.log(data)
                    this.$router.push('/')
                }).catch((error) => {
                    console.error(error)
                })
            }
        }
    }
</script>


<style scoped>
    input[type="text"], textarea {
        color: #f1f5f8;
        padding: 10px 14px;
        border: 0px solid lightgray;
        border-radius: 5px;
    }
    select {
        color: #f1f5f8;
        background-color: #3d4852;
        font-size: 16px;
    }

    label {
        display: block;
    }
</style>

