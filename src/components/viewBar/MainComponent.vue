<template>
    <div class="anim">
        <NoteHeader :note="active" @update-note="updateNote" />
        <div v-if="active._id !== 0" class="anim">
            <NoteDescription :note="active" @update-note="updateNote" />
            <NoteContent :note="active" @update-note="updateNote" />
            <button @click="addContent">➕</button>
        </div>
        <div v-else>
            <br /><br /> ← Choose one note
        </div>
    </div>
</template>

<script>
import NoteHeader from './NoteHeader.vue';
import NoteDescription from './NoteDescription.vue';
import NoteContent from './NoteContent.vue';
import axios from 'axios';

export default {
    components: {
        NoteHeader,
        NoteDescription,
        NoteContent,
    },
    props: {
        active: Object,
    },
    data() {
        return {
            urlTest: 'http://localhost:1010/note',
            urlMain: 'https://back-todo-list-lovat.vercel.app/note',
        };
    },
    methods: {
        async updateNote() {
            try {
                const response = await axios.patch(`${this.urlTest}/${this.active._id}`, this.active);
                console.log('Data updated successfully:', response.data);
                this.$emit('change', this.active);
            } catch (error) {
                console.error('Error updating data:', error);
            }
        },
        addContent() {
            this.active.content.push({
                type: 'checkbox',
                checked: false,
                text: '',
                unChecker: false,
                id: Date.now(),
            });
            this.updateNote();
        },
    },
};
</script>

<style scoped>
.anim {
    animation: show 0.5s ease;
    transform: scaleX(1);
}

@keyframes show {
    0% {
        transform: scaleX(0.8)scaleY(0.3);
    }

    100% {
        transform: scaleX(1)scaleY(1);
    }
}

button {
    margin-top: 20px;
    border: none;
    outline: none;
    background-color: #d6d2cb27;
    color: #303036;
    padding: 9px;
    border-radius: 5px;
    transition: .3s;
}
</style>