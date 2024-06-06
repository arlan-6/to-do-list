<template>
    <div class="main anim">
        <div v-for="content in note.content" :key="content.id" class="box anim" :style="{
            marginTop: content.type === 'text' ? '26px' : '6px',
            backgroundColor: content.type === 'checkbox' && content.unChecker ? '#d79b3227' : 'auto',
        }">
            <input @click="() => changeType(content.id)" value="🔁" title="Change to checkbox or text" class="tr"
                type="button" />
            <div class="content">
                <p v-if="content.type === 'text'" style="font-size: larger;"></p>
                <input @click="updateNote" class="checkbox" v-model="content.checked" type="checkbox" name="done"
                    v-else-if="content.type === 'checkbox'" />
                <input placeholder="text" @blur="updateNote" class="text" type="text" :style="{
            textDecoration: content.checked ? 'line-through' : 'none',
            fontSize: content.type === 'text' ? '1.1em' : '1em',
        }" v-model="content.text" />
            </div>
            <div class="tr" v-show="content.type === 'checkbox'" title="Auto unCheck">⏰</div>
            <input @click="updateNote" v-model="content.unChecker" v-show="content.type === 'checkbox'"
                title="Auto unCheck" class="tr" type="checkbox" />
            <button class="trr" @click="() => deleteContent(content.id)">🗑️</button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        note: Object,
    },
    methods: {
        changeType(contentId) {
            const content = this.note.content.find(item => item.id === contentId);
            if (content) {
                content.type = content.type === 'text' ? 'checkbox' : 'text';
                this.updateNote();
            }
        },
        deleteContent(id) {
            this.note.content = this.note.content.filter(item => item.id !== id);
            this.updateNote();
        },
        updateNote() {
            this.$emit('update-note');
        },
    },
};
</script>

<style scoped>
.anim {
    animation: show 0.3s ease;
    transform: scaleX(1);
}

@keyframes show {
    0% {
        transform: scaleX(0.9)scaleY(0.4);
    }

    100% {
        transform: scaleX(1)scaleY(1);
    }
}

.main {
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
    transition: 0.3s;
    box-shadow: rgba(0, 0, 0, 0.05) 0px 0px 0px 1px, rgb(209, 213, 219) 0px 0px 0px 1px inset;
}

.box:hover {
    box-shadow: rgba(0, 0, 0, 0.05) 0px 0px 0px 1px, #ccd4e0 0px 0px 0px 1px inset;
}

.content {
    margin-top: 0px;
    display: flex;
    gap: 10px;
    align-items: center;
    flex: 3;
}

.text {
    width: 100%;
    background-color: rgba(240, 248, 255, 0);
    border: none;
    outline: none;
    color: #303036;
}

.tr {
    cursor: pointer;
    font-size: 0.7em;
    padding: 0px;
    margin-top: 0px;
    border-radius: 3px;
    background-color: #d5ccbe10;
    border: none;
    outline: none;
}

.trr {
    cursor: pointer;
    font-size: 0.7em;
    padding: 0px;
    margin-top: 0px;
    margin-left: 20px;
    border-radius: 3px;
    background-color: #3e3e3e00;
    border: none;
    outline: none;
}

.checkbox {
    margin-left: 20px;
}
</style>