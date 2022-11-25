<template>
    <div id="title-container">
        <h1>Title</h1>
        <input name="title" id="title" type="text">
    </div>
</template>

<script>
export default {
    name: 'FormTitle'
}
</script>

<style scoped>
    #title-container {
        display: flex;
        flex-direction: column;
        text-align: left;
    }
    input {
        padding: 10px;
        border: 1px solid #23232328;
        border-radius: 10px;
    }
</style>