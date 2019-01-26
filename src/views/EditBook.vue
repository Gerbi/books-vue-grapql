<template>
    <div class="create max-w-sm mx-auto mt-6">
        <h1 align="center" class="mb-3">Edit Book</h1>
        <form action="#" method="POST" @submit.prevent="editBook" class="bg-dark shadow-md rounded px-8 pt-6 pb-8 mb-4">
            <div class="mb-5">
                <label class="text-grey font-bold mb-2" for="title">Title</label>
                <input type="text" id="title" name="title" class="bg-grey-darkest text-grey-lighter w-full py-2" v-model="title">
            </div>
            <div class="mb-5">
                <label class="text-grey font-bold mb-2" for="title">Author</label>
                <input type="text" id="author" name="author" class="bg-grey-darkest text-grey-lighter w-full" v-model="author">
            </div>
            <div class="form-group">
                <img :src="`http://localhost:8000/img/${image}`" alt="book cover">
            </div>
            <div class="mb-5">
                <label class="text-grey font-bold mb-2" for="image">Image</label>
                <input type="text" id="image" name="image" class="bg-grey-darkest text-grey-lighter w-full" v-model="image">
            </div>
            <div class="mb-5">
                <label class="font-bold mb-2" for="description">Description</label>
                <textarea name="description" id="description" class="bg-grey-darkest text-grey-lighter w-full" rows="8" v-model="description"></textarea>
            </div>
            <div class="mb-5">
                <label class="font-bold mb-2" for="link">Link</label>
                <input type="text" name="link" id="link" class="bg-grey-darkest text-white w-full" v-model="link">
            </div>

            <div class="mb-5">
                <label class="font-bold mb-2"><input type="checkbox" name="featured" v-model="featured" class="mr-2">Featured</label>
            </div>

            <div class="mb-5">
                <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
                    <template slot-scope="{ result: { data, loading }, isLoading }">
                        <div v-if="isLoading">Loading...</div>
                        <select v-else v-model="category" class="bg-dark pt-2 pb-1 pl-2 rounded text-grey-dark w-full">
                            <option v-for="category of data.categories" :key="category.id" :value="category.id">
                                {{ category.name }}
                            </option>
                        </select>
                    </template>
                </ApolloQuery>
            </div>

            <div class="flex items-center justify-end">
                <router-link :to="`/books/${book.id}`" class="bg-dark text-grey hover:text-orange font-bold py-2 px-4 mr-4 rounded focus:outline-none focus:shadow-outline" type="button">
                    Cancel
                </router-link>
                <button type="submit" class="bg-grey hover:bg-green-dark hover:text-white text-grey-darkest py-2 px-4 rounded">Update Book</button>
            </div>

        </form>
    </div>
</template>

<script>
    import updateBook from '@/graphql/mutations/UpdateBook.gql'
    import book from '@/graphql/queries/Book.gql'

    export default {

        data() {
            return {
                title: '',
                author: '',
                image: '',
                description: '',
                link: '',
                featured: false,
                category: 1,
                book: null
            }
        },
        apollo: {
            // Advanced query with parameters
            // The 'variables' method is watched by vue
            book: {
                query: book,
                // Reactive parameters
                variables () {
                    if (this.$route && this.$route.params) {
                        return {
                            id: this.$route.params.id
                        }
                    }
                },

                // Optional result hook
                result ({ data: {book} }) {
                    this.title = book.title
                    this.author = book.author
                    this.image = book.image
                    this.description = book.description
                    this.link = book.link
                    this.featured = book.featured
                    this.category = book.category.id
                },
            },
        },
        methods: {
            editBook() {
                this.$apollo.mutate({
                    // Query
                    mutation: updateBook,
                    // Parameters
                    variables: {
                        id: this.$route.params.id,
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
                    this.$router.push(`/books/${this.$route.params.id}`)
                }).catch((error) => {
                    console.error(error)
                })
            }
        }
    }
</script>


<style scoped>
    .form-group {
        margin-bottom: 32px;
    }
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

