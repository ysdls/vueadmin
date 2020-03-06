<template>
        <v-card
        outlined
        tile
        elevation="1"
        class="cardSection"
        >
            <div class="subtitle-2">상품이미지</div>
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
        apiurl:String
    },
    data() {
        return {
            file:[]
        }
    },
    methods: {
        fileupload(e) {
 
            for(let i=0; i < e.length; i++) {
                
                let form = new FormData();
                form.append("image", e[i]);
                
                this.$axios.post(`${this.apiurl}/erp/image/`, form,{
                'headers': {
                    'Content-Type': "multipart/form-data;charset=utf-8;",
                    'Authorization' : "token a4f797d9efbdae9d5aef894c2aa4c54621435471"
                }
                })
                .then(r=> {
                    console.log('success', r);
                    this.file.push(r.data.image)
                })
                .catch(e=> {
                    console.log('error');
                })
                                
            }
            this.$emit("product-image-func-parent", this.file)

            // for (var key of form.keys()) {
            //     console.log(key);
            // }
            // for (var value of form.values()) {
            //     console.log(value);
            // }

            
        }
    }
}
</script>