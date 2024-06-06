<template>
    <input placeholder="description" @click="note.description = ''" @blur="updateNote" class="text texta" type="text"
        v-model="note.description" />
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
.text {
    width: 100%;
    background-color: rgba(240, 248, 255, 0);
    border: none;
    outline: none;
    color: #303036;
}

.texta {
    font-size: large;
}
</style>