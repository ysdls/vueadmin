<template>
    <v-card
    outlined
    tile
    elevation="1"
    class="cardSection"
    >
        <div class="subtitle-2">카테고리</div>
        <v-row>
            <v-col cols="12" sm="3">
                <v-select
                :items="items"
                label="대분류"
                outlined
                dense
                ></v-select>
            </v-col>

            <v-col cols="12" sm="3">
                <v-select
                :items="items"
                label="중분류"
                outlined
                dense
                ></v-select>
            </v-col>

            <v-col cols="12" sm="3">
                <v-select
                :items="items"
                label="소분류"
                outlined
                dense
                ></v-select>
            </v-col>

            <v-col cols="12" sm="3">
                <v-select
                :items="items"
                label="세분류"
                outlined
                dense
                ></v-select>
            </v-col>
        </v-row>
    </v-card>
</template>

<script>
export default {
    data() {
        return {
            items: ['Foo', 'Bar', 'Fizz', 'Buzz'],
        } 
    }
}
</script>