<template>
    <div v-if="thisNote" style="width: 80%">
        <div class="top-toolbox">
            <div class="top-toolbox__title">Title:</div>
            <input class="top-toolbox__doc-name" v-model="thisNote.title">
            <button @click="removeNote" title="Remove note">
                <i class="material-icons">delete</i>
            </button>
            <button @click="favoriteNote" title="Favorite note">
                <i class="material-icons">{{ thisNote.favorite ? 'star' : 'star_border' }}</i>
            </button>
        </div>
        <div class="write-page">
            <div class="write-page__box">
            <section class="write-page__write">
                <textarea v-model="thisNote.content"
                ></textarea>
            </section>
            <aside
                class="write-page__preview"
                v-html="notePreview"
            ></aside>
            </div>
            <span class="write-page span">{{"Created: " + timeStr + " Lines " + linesCount + " Words " + wordsCount + " Characters " + charsCount}}</span>
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
        },
        timeStr() {
            return (new Date(this.thisNote.created)).toLocaleString();
        },
        linesCount() {
            if (this.thisNote) {
                return this.thisNote.content.split(/\r\n|\r|\n/).length;
            } else {
                return null
            }
        },
        wordsCount() {
            if (this.thisNote) {
                let s = this.thisNote.content;
                s = s.replace(/\n/g, '');
                s = s.replace(/(^\s*)|(\s*$)/gi, '');
                s = s.replace(/\s\s+/gi, ' ');
                return s.split(' ').length;
            } else {
                return null
            }
        },
        charsCount() {
            if (this.thisNote) {
                return this.thisNote.content.split('').length;
            } else {
                return null
            }
        }
    },
    methods: {
        getNote(note) {
            this.thisNote = note;
        },
        removeNote() {
            this.$bus.$emit('remove-note');
        },
        favoriteNote() {
            this.thisNote.favorite ^= true;
        }
    }
}
</script>

<style scoped>
button {
    outline: none !important;
    background: #40b883;
    color: white;
    border-radius: 3px;
    border: none;
    font-size: 16px;
    line-height: 24px;
    display: flex;
    margin-right: 10px;
}
button:hover {
    background: #63c89b;
}
.top-toolbox {
    height: 43px;
    border-bottom: #81d1ba 1px solid;
    box-sizing: border-box;
    display: flex;
    align-items: flex-start;
}
.top-toolbox__title {
    height: 26px;
    font-size: 18px;
    font-weight: bold;
    line-height: 26px;
    margin-right: 8px;
}
.top-toolbox__doc-name {
    height: 22px;
    font-size: 18px;
    line-height: 22px;
    outline-style: none;
    border: 0;
    border-bottom: 1px #40b883 solid;
    margin-right: 10px;
}
.write-page {
    width: 100%;
    display: flex;
    flex-direction: column;
}

.write-page__box {
    width: 100%;
    display: flex;

}

.write-page__write {
    width: 50%;
    height: calc(100vh - 140px);
    border-right: #81d1ba 1px solid;
    padding-left: 16px;
    padding-right: 16px;
    padding-top: 16px;
}

.write-page.span {
    display: block;
    text-align: left;
}

textarea {
    font-family: sans-serif;
    width: calc(100% - 4px);
    border: 0;
    resize: none;
    outline: none !important;
    height: calc(100% - 16px);
}

.write-page__preview {
    padding-left: 16px;
    padding-right: 16px;
    width: 50%;
    text-align: left;
    height: calc(100vh - 140px);
}
</style>
