<script>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import { ref } from "vue";
import { useQuery, useResult } from "@vue/apollo-composable";
import ALL_BOOKS_QUERY from "./graphql/allBooks.query.gql";
export default {
    name: "App",
    setup() {
        const searchTerm = ref("");
        const { result, loading, error } = useQuery(
            ALL_BOOKS_QUERY,
            () => ({
                search: searchTerm.value,
            }),
            () => ({
                debounce: 500,
                // enabled: searchTerm.value.length > 1,
            })
        );
        const books = useResult(result, [], (data) => data.allBooks);
        console.log(books);
        return { books, searchTerm, loading, error };
    },
};
</script>

<template>
    <div>
        <input type="text" v-model="searchTerm" />
        <p v-if="loading">..loading</p>
        <p v-else-if="error">There is an error {{ error }}</p>
        <template v-else>
            <p v-for="(item, index) in books" :key="index">
                {{ item.title }}
            </p>
        </template>
    </div>
</template>

<style scoped>
.logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
}
.logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
    filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
