<template>
    <h2 v-if="note._id !== 0">
        <input placeholder="title" @click="note.title = ''" @blur="updateNote" class="h1 text" type="text"
            v-model="note.title" />
    </h2>
</template>

<script>
export default {
    props: {
        note: Object,
    },
    methods: {
        updateNote() {
            this.$emit('update-note');
        },
    },
};
</script>

<style scoped>
.h1 {
    font-size: 1.1em;
    font-weight: bold;
}

.text {
    width: 100%;
    background-color: rgba(240, 248, 255, 0);
    border: none;
    outline: none;
    color: #303036;
}
</style>