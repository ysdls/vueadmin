<template>

</template>

<script>
export default {
    props: {
        topc:String,
        middlec:String,
        bottomc:String,
        detailc:String,
        productName: String,
        brand: String
    },
    data() {
        return {
            word:"",
            searchword: [
                this.topc, this.middlec, this.bottomc, this.detailc, this.productName, this.brand
            ]
        }
    },

    methods: {
        remove (item) {
            this.searchword.splice(this.searchword.indexOf(item), 1)
            this.searchword = [...this.searchword]
        }
    }

    
}
</script>