<template>
    <div>
        <button @click="getFile()">Open</button>
        <button @click="saveFile()">Save</button>
    </div>
    <div>
        {{ fileName }}
    </div>
    <div>
        <textarea cols="30" rows="10" v-model="fileText"></textarea>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import * as fs from 'wicg-file-system-access'

export default defineComponent({
    data() {
        return {
            fileText: '' as string,
            fileName: '' as string,
            fileHandle: null as FileSystemFileHandle | null
        }
    },
    methods: {
        async getFile() {
            let handles: Array<FileSystemFileHandle> = await window.showOpenFilePicker();
            this.fileHandle = handles[0];
            let file = await handles[0].getFile();
            this.fileName = file.name;
            this.fileText = (await (await handles[0].getFile()).text());
        },
        async saveFile() {
            const writable = await this.fileHandle?.createWritable();
            await writable?.write(this.fileText);
            await writable?.close();
            alert("Changes saved");
        }
    }
}) 
</script>
