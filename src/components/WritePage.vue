<template>
    <div v-if="thisNote" style="width: 80%">
        <div class="top-toolbox">
            <input v-model="thisNote.title">
            <button @click="removeNote" title="Remove note">
                <i class="material-icons">delete</i>
            </button>
        </div>
        <div class="write-page">
            <section class="write-page__write">
            <textarea v-model="thisNote.content"
            ></textarea>
            </section>
            <aside
                class="write-page__preview"
                v-html="notePreview"
            ></aside>
        </div>
    </div>
</template>

<script>
import { marked } from 'marked';

export default {
    name: 'WritePage',
    data() {
        return {
            thisNote: null
        }
    },
    created() {
        this.$bus.$on('select-note', this.getNote);
    },
    computed: {
        notePreview() {
            return marked(this.thisNote.content);
        }
    },
    methods: {
        getNote(note) {
            this.thisNote = note;
        },
        removeNote() {
            this.$bus.$emit('remove-note');
        }
    }
}
</script>

<style scoped>
.write-page {
    width: 100%;
    display: flex;
}

.write-page__write {
    width: 50%;
    height: 100%;
}

textarea {
    font-family: sans-serif;
    width: 100%;
    border: 0;
    resize: none;
    outline: none !important;
}

.write-page__preview {
    width: 50%;
    text-align: left;
}
</style>
