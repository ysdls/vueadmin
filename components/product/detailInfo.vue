<template>
    <v-card
        outlined
        tile
        elevation="1"
        class="cardSection"
        >
            <div class="subtitle-2">상세정보</div>
            <div class="quill-editor" v-quill:myQuillEditor="editorOption">
            </div>
        </v-card>
</template>

<script>
export default {
    data() {
        return {
            editorOption: {
                    // some quill options
                modules: {
                    toolbar: [
                    ['bold', 'italic', 'underline', 'strike'],
                    ['blockquote', 'code-block', 'link', 'image']
                    ]
                }
            },
        }
    }
}
</script>