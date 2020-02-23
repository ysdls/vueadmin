<template>
    <v-card
        outlined
        tile
        elevation="1"
        class="cardSection"
        >
            <div class="subtitle-2">구매조건</div>
            <v-row>
                <v-col cols="12" md="6">
                    <v-text-field
                        type="number"
                        label="최소구매수량"
                        hint="최소구매 수량은 2개부터 입력하세요. 기본값은 1이며 미입력시 1로 적용됩니다"
                        placeholder=""
                        clearable
                        required
                        persistentHint
                    ></v-text-field>
                </v-col>
            </v-row>
        </v-card>
</template>

