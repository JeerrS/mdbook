<template>
    <div class="side-bar">
        <div class="tool-bar">
            <button
                @click="addNote"
                :title="notes.length + ' note(s) already'"
            >
                <i class="material-icons">add</i>
                Add note
            </button>
        </div>
        <div class="note-list">
            <div
                v-for="item in notes"
                :key="item.id"
                @click="selectNote(item.id)"
                :class="{selected: item.id === selectedId}"
            >
                {{ item.title }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'PageSide',
    data() {
        return {
            notes: JSON.parse(localStorage.getItem('notes')) || [],
            selectedId: localStorage.getItem('selected-id') || null
        }
    },
    watch: {
        notes: {
            handler: 'saveNotes',
            deep: true
        },
        selectedId: {
            handler(val) {
                localStorage.setItem('selected-id', val)
            }
        }
    },
    computed: {
        selectedNote() {
            return this.notes.find(note => note.id === this.selectedId);
        }
    },
    mounted() {
        this.$bus.$emit('select-note', this.selectedNote);
        this.$bus.$on('remove-note', this.removeNote);
    },
    methods: {
        addNote() {
            const time = Date.now();
            const note = {
                id: String(time),
                title: 'New Note ' + (this.notes.length + 1),
                content: 'Write Something In **Here**.',
                created: time,
                favorite: false
            }
            this.notes.push(note);
        },
        selectNote(noteId) {
            this.selectedId = noteId;
            this.$bus.$emit('select-note', this.selectedNote);
        },
        saveNotes() {
            localStorage.setItem('notes', JSON.stringify(this.notes));
        },
        removeNote() {
            this.notes.find((note, index) => {
                if (note.id === this.selectedId) {
                    this.notes.splice(index, 1);
                    this.selectedId = null;
                    this.$bus.$emit('select-note', null);
                }
            });
        }
    }
}
</script>

<style scoped>
.side-bar {
    width: 20%;
}
button {
    outline: none !important;
    background: #40b883;
    color: white;
    border-radius: 3px;
    border: none;
}
button:hover {
    background: #63c89b;
}

.selected {
    background: #40b883;
    color: white;
}
</style>
