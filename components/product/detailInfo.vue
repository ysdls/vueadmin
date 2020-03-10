<template>
    <v-card
        outlined
        tile
        elevation="1"
        class="cardSection"
        >
            <div class="subtitle-2">상세정보*</div>
            <div class="quill-editor" :content="content" @change="onEditorChange($event)" @blur="onEditorBlur($event)" @focus="onEditorFocus($event)" v-quill:myQuillEditor="editorOption" ref="quillEdit" style="height:300px;"></div>
            <input type="file" @change="uploadFunction" id="file" hidden>
        </v-card>
</template>

<script>
export default {
    props : {
        apiurl:String,
        token:String
    },
    data() {
        return {
            editorOption: {
                    // some quill options
                modules: {
                    toolbar:{
                        container: [
                            [{ 'size': ['small', 'normal', 'large', 'huge'] }],
                            ['bold', 'italic', 'underline', 'strike'],
                            ['blockquote', 'code-block', 'image'],
                            [{ align: '' }, { align: 'center' }, { align: 'right' }, { align: 'justify' }]
                        ],
                        handlers: { 
                            'image': function(){
                                document.getElementById('file').click()
                            } 
                        }
                    } 
                }
            },
            content:"",
            selectedFile : '',
            editor: ""
        }
    },
    methods: {
        onEditorChange({ editor, html, text }) {
            // console.log('editor change!', editor, html, text)
            this.content = html
        },
        onEditorFocus(editor) {
            this.editor = editor
        },
        onEditorBlur() {
            this.$emit("detail-info-func-parent", this.content)
        },
        uploadFunction(e){
            this.selectedFile = e.target.files[0];

            let form = new FormData();
            form.append("image", this.selectedFile);

            //upload image to server
            this.$axios.post(`${this.apiurl}/erp/image/`, form,{
                'headers': {
                    'Content-Type': "multipart/form-data;charset=utf-8;",
                    'Authorization' : `token ${this.token}`
                }
            })
            .then(r=> {
                console.log('success');
                const range = this.editor.getSelection()
                this.editor.insertEmbed(range.index, 'image',  r.data.image);
            })
            .catch(e=> {
                console.log(e.response);
            })
        }

    }
    
}
</script>