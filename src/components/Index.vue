<template>
    <div class="container">
        <h1 class="mt-5">Almost google</h1>
        <SearchBar></SearchBar>
    </div>
</template>

<script>
    import SearchBar from "./SearchBar";
    export default {
        name: "Index",
        components: {
            SearchBar,
        }
    }
</script>

<style scoped>

</style>