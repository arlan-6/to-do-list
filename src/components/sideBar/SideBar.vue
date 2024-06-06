<script>
import axios from 'axios';
export default {
    data() {
        return {
            template: { title: "Title", description: 'Description', content: [{ type: 'text', text: "", unChecker: false },] },
            addedNote: {},
            urlTest: 'http://localhost:1010/note',
            urlMain: 'https://back-todo-list-lovat.vercel.app/note',
            active: {}
        }
    },
    props: {
        notesList: Array,
    },
    emits: ['deleteNote', 'setActive', 'addNote'],
    methods: {
        setActive(note) {
            this.active = note
            this.$emit('setActive', note)
        },
        async addNote() {
            await this.AddData()
            // this.$emit('addNote',this.addedNote)
        },
        deleteNote(id) {
            this.$emit('deleteNote', id)
            axios.delete(`${this.urlTest}/${id}`)
                .then(response => {
                    console.log('Data deleted successfully:', response.data);
                })
                .catch(error => {
                    console.error('Error deleting data:', error);
                });
        },
        async AddData() {

            await axios.post(`${this.urlTest}`, this.template)
                .then(r => r.data)
                .then(response => {
                    console.log('Data updated successfully:', response);
                    // this.addNote = response;
                    this.$emit('addNote', response)
                })
                .catch(error => {
                    console.error('Error updating data:', error);
                });
        }
    }
};
</script>

<template>
    <div class="m">
        <!-- <h2>Notes 📝</h2> -->
        <button class="addbtn" @click="addNote">Add</button>
        <button>Ai</button>
    </div>
    <!-- {{ notesList }} -->
    <div class="main">
        <div v-for="note in notesList" :key="note._id" class="note overflow anim"
            :style="{ borderWidth: this.active._id == note._id ? '3px' : '1px' }" v-show="note._id"
            @click="setActive(note)">
            <!-- {{ note }} -->
            <br>
            <p>

                <strong class="overflow">{{ note.title }} </strong> <br>
                <span class="overflow">{{ note.description }}</span>

            </p>
            <button class="trash" @click="deleteNote(note._id)">🗑️</button>
        </div>


    </div>
</template>

<style scoped>
.anim {
    animation: show 0.3s ease;
    transform: scaleX(1) scaleY(1);
}

@keyframes show {
    0% {
        transform: scaleX(0.9)scaleY(0.9);
    }

    100% {
        transform: scaleX(1);
    }
}

.overflow {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 310px;
    text-wrap: wrap;
}

.main {
    overflow-y: auto;
    height: 70%;
}

.main::-webkit-scrollbar-thumb {
    background-color: rgb(203, 122, 16);
}

.note {
    border: 1px solid #ccc;
    border-radius: 10px;
    margin-top: 10px;
    padding-left: 10px;
    cursor: pointer;
    transition: .2s;
    display: flex;
    text-align: center;
    justify-content: space-between;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.note:hover {
    background-color: #2c2a2722;
    border: 1px solid #e0e0e0;
}

button {
    border: none;
    outline: none;
    background-color: #2c2a271d;
    color: #303036;
    padding: 9px;
    border-radius: 5px;
}

button:hover {

    background-color: #3937342f;
    color: #303036;
}

.trash {

    border-radius: 10px;
}

.addbtn {
    margin-right: 10px;
}
</style>