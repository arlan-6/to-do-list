<script>
import axios from 'axios';
// import { VueAnime } from 'vue-anime'
export default {
    // components: { VueAnime, },
    data() {
        return {
            showAddContent: false,
            urlTest: 'http://localhost:1010/note',
            urlMain: 'https://back-todo-list-lovat.vercel.app/note'
        }
    },
    props: {
        active: Object,
    },
    methods: {
        changeValue() {
            this.$emit('change', this.active)
            console.log(this.active.title)
            this.updateData()
        },
        deleteContent(event, id) {
            console.log(event.target)
            this.active.content = this.active.content.filter(item => item.id !== id)
            this.updateData()
        },
        addContent() {
            this.active.content = [...this.active.content, {
                type: 'checkbox',
                checked: false,
                text: "",
                unChecker: false,
                id: Date.now(),
            }]
            this.updateData()
        },
        changType(contentId) {
            const content = this.active.content.find(item => item.id === contentId);

            if (content) {
                // Toggle content type
                content.type = content.type === 'text' ? 'checkbox' : 'text';

                // Emit change event to notify parent component of the update
                this.$emit('change', this.active);
                this.updateData()
            }
        },
        async updateData() {
            console.log(this.active)
            await axios.patch(`${this.urlTest}/${this.active._id}`, this.active)
                .then(response => {
                    console.log('Data updated successfully:', response.data);
                })
                .catch(error => {
                    console.error('Error updating data:', error);
                });
        },
        getElem(e) {
            console.log(e)
        }

    },
    emits: ['change'],
};
</script>

<template>
    <div class="anim">
        <h2 v-if="active._id != 0"><input placeholder="title" @blur="changeValue" class="h1 text" type="text"
                v-model="active.title" /></h2>
        <!-- {{ active }} -->
        <div class="anim" v-if="active._id != 0">
            <!-- <input placeholder="title" @blur="changeValue" class="texta text" type="text" v-model="active.title" /> -->
            <input placeholder="description" @blur="changeValue" class="text texta" type="text"
                v-model="active.description" />
            <div class="main anim">
                <div v-for="content in active.content" class="box anim" :style="{
            marginTop: content.type === 'text' ? '26px' : '6px',

            backgroundColor: content.type === 'checkbox' && content.unChecker === true ? '#d79b3227' : auto
        }">
                    <input @click="changType(content.id)" value="🔁" title="Change to checkbox or text" class="tr"
                        type="button" name="" id="">

                    <!-- {{ content }} -->
                    <div class="content">
                        <p v-if="content.type === 'text'" style="font-size: larger;"></p>
                        <input @click="updateData" class="checkbox" v-model="content.checked" type="checkbox"
                            name="done" v-else-if="content.type === 'checkbox'" id="">
                        <input placeholder="text" @blur="changeValue" class="text " type="text" :style="{
            textDecoration: content.checked ? 'line-through' : 'none',
            fontSize: content.type === 'text' ? '1.1em' : '1em',
        }" v-model="content.text" />
                    </div>
                    <div class="tr" v-show="content.type === 'checkbox'" title="Auto unCheck">⏰</div>
                    <input @click="changeValue" v-model="content.unChecker" v-show="content.type === 'checkbox'"
                        value=content.unChecker title="Auto unCheck" class="tr" type="checkbox" name="" id="">

                    <button class="trr" @click="(event) => {
            deleteContent(event, content.id);

        }">🗑️</button>
                </div>
            </div>
            <button @click="addContent">➕</button>

            <div class="" v-show="showAddContent">
                bu
            </div>
        </div>
        <div v-else><br><br> ← Choose one note</div>
    </div>
</template>

<style scoped>
.anim {
    animation: show .5s ease;
    /* position: relative; */
    left: 0;
    /* float: left; */
    /* background-color: #303036; */
    transform: scaleX(1);
}

@keyframes show {
    0% {
        /* height: 0%;
        width: 0%; */

        transform: scaleX(0);
        /* background-color: #15154b; */
    }

    100% {
        /* height: 100%;
        width: 100%; */

        transform: scaleX(1);
        /* background-color: #303036; */
    }
}

.main {

    /* margin-top: 120px; */
    overflow-y: auto;
    height: 300px;
}

.box {
    margin-top: 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-radius: 5px;
    padding: 4px;
    padding-left: 9px;
    padding-right: 9px;
    transition: .3s;
    box-shadow: rgba(0, 0, 0, 0.05) 0px 0px 0px 1px, rgb(209, 213, 219) 0px 0px 0px 1px inset;
}

.box:hover {
    box-shadow: rgba(0, 0, 0, 0.05) 0px 0px 0px 1px, #ccd4e0 0px 0px 0px 1px inset;
}

.content {
    /* border: 2px solid wheat; */
    margin-top: 0px;
    display: flex;
    gap: 10px;
    align-items: center;
    flex: 3;
}

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

.texta {
    font-size: large;
    /* width: 200%; */
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

button:hover {

    background-color: #afa5951d;
    color: #F0E3CA;
}

.tr {
    cursor: pointer;
    font-size: .7em;
    padding: 0px;
    margin-top: 0px;
    border-radius: 3px;
    background-color: #d5ccbe10;
    border: none;
    outline: none;
}

.trr {
    cursor: pointer;
    font-size: .7em;
    padding: 0px;
    margin-top: 0px;
    margin-left: 20px;
    border-radius: 3px;
    background-color: #3e3e3e00;
    border: none;
    outline: none;
}

.tr:hover~.box {
    background-color: #ae7e36;
}

.check {
    margin-right: 5px;
}

.checkbox {
    margin-left: 20px;
}
</style>