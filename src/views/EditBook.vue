<template>
    <div class="create">
        <h1>Edit Book</h1>
        <form action="" method="POST" @submit.prevent="editBook">
            <div class="form-group">
                <label for="title">Title</label>
                <input type="text" name="title" id="title" v-model="title">
            </div>
            <div class="form-group">
                <label for="author">Author</label>
                <input type="text" name="author" id="author" v-model="author">
            </div>
            <div class="form-group">
                <img :src="`http://localhost:8000/img/${image}`" alt="book cover">
            </div>
            <div class="form-group">
                <label for="image">Image</label>
                <input type="text" name="image" id="image" v-model="image">
            </div>
            <div class="form-group">
                <label for="description">Description</label>
                <textarea name="description" id="description" cols="30" rows="10" v-model="description"></textarea>
            </div>
            <div class="form-group">
                <label>Link</label>
                <input type="text" name="link" id="link" v-model="link">
            </div>
            <div class="form-group">
                <label><input type="checkbox" name="featured" v-model="featured">Featured</label>
            </div>
            <div class="form-group">
                <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
                    <!-- The result will automatically updated -->
                    <template slot-scope="{ result: { data, loading }, isLoading }">
                        <!-- Some content -->
                        <div v-if="isLoading">Loading...</div>
                        <select v-else v-model="category">
                            <option href="#" v-for="category of data.categories" :key="category.id"
                                    :value="category.id">
                                {{category.name}}
                            </option>
                        </select>
                    </template>
                </ApolloQuery>
            </div>

            <div class="form-group">
                <button type="submit">Update Book</button>
            </div>
        </form>
    </div>
</template>

<script>
    import updateBook from '@/graphql/mutations/UpdateBook.gql'
    import book from '@/graphql/queries/Book.gql'

    export default {
        name: "AddBock",
        data() {
            return {
                title: '',
                author: '',
                image: '',
                description: '',
                link: '',
                featured: false,
                category: 1,
                book:null
            }
        },
        apollo: {
            // Query with parameters
            book: {
                query: book,
                // Reactive parameters
                variables() {
                    if(this.$route && this.$route.params){
                        return {
                            id: this.$route.params.id
                        }
                    }
                },
                result({data,loading, networkStatus}){
                    this.title = data.book.title
                    this.author = data.book.author
                    this.image = data.book.image
                    this.description = data.book.description
                    this.link = data.book.link
                    this.featured = data.book.featured
                    this.category = data.category.id
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

    input[type="text"] {
        padding: 10px 14px;
    }

    button {
        padding: 16px;
        background: #027bff;
        color: white;
        border-radius: 5px;
        font-size: 16px;
    }

</style>