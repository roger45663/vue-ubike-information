<script>
export default {
    props: {
        searchTxt: {
            type: String,
            default: ''
        }
    }
};
</script>

<template>
    <h1>YouBike 臺北市公共自行車即時資訊</h1>
    <p>站點名稱搜尋: <input type="text" v-model="searchTxt" /></p>
</template>

<style>

</style>
