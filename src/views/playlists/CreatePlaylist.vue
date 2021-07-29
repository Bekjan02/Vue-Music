<template>
    <form @submit.prevent="handleSubmit">
        <h4>Create New Playlist</h4>
        <input type="text" placeholder="Plalist" required v-model="title" />
        <textarea
            placeholder="Playlist description"
            required
            v-model="description"
        ></textarea>
        <label for="">Upload playlist cover image</label>
        <input type="file" @change="handleChange" />
        <div class="error">{{ fileError }} </div>
        <button>
            Create
        </button>
    </form>
</template>

<script>
import { ref } from "@vue/reactivity";
import useStorage from '@/composables/useStorage'
export default {
    setup() {
        const { filePath, url, uploadImage } = useStorage()
        const title = ref("");
        const description = ref("");
        const file = ref("");
        const fileError = ref('')

        const imgTypes = ["image/jpeg", "image/jpg", "image/png"];

        const handleSubmit = async () => {
            if(file.value) {
                await uploadImage(file.value);
                console.log('файл был загружен', url.value);
            }
        };

        const handleChange = (event) => {
            const selectedFile = event.target.files[0];
            if (selectedFile && imgTypes.includes(selectedFile.type)) {
                file.value = selectedFile;
                fileError.value = null
            } else {
                fileError.value = ' Пожалуйста выберите картинку (png/jpeg/jph) '
                file.value = null;
            }
        };

        return { title, description, fileError, handleSubmit, handleChange };
    },
};
</script>

<style>
form {
    background: white;
}
input[type="file"] {
    border: 0;
    padding: 0;
}
textarea {
    resize: none;
}
label {
    font-size: 12px;
    display: block;
    margin-top: 30px;
}
button {
    margin-top: 20px;
}
</style>
