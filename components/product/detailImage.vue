<template>
        <v-card
        outlined
        tile
        elevation="1"
        class="cardSection"
        >
            <div class="subtitle-2">상품이미지*</div>
            <v-file-input accept="image/*" show-size multiple  label="첫번째 선택한 이미지가 메인이미지가 됩니다." @change="fileupload($event)">
                <template v-slot:selection="{ index, text }">
                <v-chip
                    color="deep-purple accent-4"
                    dark
                    label
                    small
                >
                    {{ text }}
                </v-chip>
                </template>

            </v-file-input>
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
            file:[],
        }
    },
    methods: {
        fileupload(e) {
            this.file = []
            for(let i=0; i < e.length; i++) {
                
                let form = new FormData();
                form.append("image", e[i]);
                
                this.$axios.post(`${this.apiurl}/erp/image/`, form,{
                'headers': {
                    'Content-Type': "multipart/form-data;charset=utf-8;",
                    'Authorization' : `token ${this.token}`
                }
                })
                .then(r=> {
                    this.file.push(r.data.id)
                })
                .catch(e=> {
                    console.log('error');
                })
                                
            }
            this.$emit("product-image-func-parent", this.file)
            //console.log(this.file);

            // for (var key of form.keys()) {
            //     console.log(key);
            // }
            // for (var value of form.values()) {
            //     console.log(value);
            // }

            
        },
        close(index, text) {
            // this.file[index] = false
            // this.file.splice(index, 1)
            // this.file = [...this.file]
            this.file.splice(this.file.indexOf(text), 1)
            this.file = [...this.file]
            console.log(index, text);
        },
    }
}
</script>