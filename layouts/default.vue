<template>
    <!-- App.vue -->
    <v-app>
        <v-navigation-drawer v-model="drawer" :clipped="$vuetify.breakpoint.lgAndUp" app>
            <!--  -->
            <v-list dense>
                <a href="/product/add">상품추가</a>
                <a href="/product/multiple">일괄업로드</a>
                <!-- <template v-for="item in items">
                    <v-list-group
                        v-if="item.children"
                        :key="item.text"
                        v-model="item.model"
                        :prepend-icon="item.model ? item.icon : item['icon-alt']"
                        append-icon=""
                    >
                        <template v-slot:activator>
                            <v-list-item-content>
                                <v-list-item-title>
                                {{ item.text }}
                                </v-list-item-title>
                            </v-list-item-content>
                        </template>
                        <v-list-item
                            v-for="(child, i) in item.children"
                            :key="i"
                            link
                        >
                        <v-list-item-action v-if="child.icon">
                            <v-icon>{{ child.icon }}</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title>
                            {{ child.text }}
                            </v-list-item-title>
                        </v-list-item-content>
                        </v-list-item>
                    </v-list-group>
                    <v-list-item
                        v-else
                        :key="item.text"
                        link
                    >
                        <v-list-item-action>
                        <v-icon>{{ item.icon }}</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                        <v-list-item-title>
                            {{ item.text }}
                        </v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </template> -->
            </v-list>
        </v-navigation-drawer>

        <v-app-bar :clipped-left="$vuetify.breakpoint.lgAndUp" app>
             <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
             <v-toolbar-title
                style="width: 300px"
                class="ml-0 pl-4"
            >
                <span class="hidden-sm-and-down">Airsupply Dashboard</span>
            </v-toolbar-title>
        <!-- -->
        </v-app-bar>

        <!-- Sizes your content based upon application components -->
        <v-content>

        <!-- Provides the application the proper gutter -->
        <v-container fluid class="grey lighten-5">

            <!-- If using vue-router -->
            <router-view></router-view>
        </v-container>
        </v-content>

        <v-footer app>
        <!-- -->
        </v-footer>
    </v-app>
</template>

<script>
export default {
    data() {
        return {
            drawer: null,
            items: [
                { icon: 'mdi-contacts', text: '상품추가', link: "/product/add" },
                // { icon: 'mdi-history', text: 'Frequently contacted' },
                // { icon: 'mdi-content-copy', text: 'Duplicates' },
                // {
                //     icon: 'mdi-chevron-up',
                //     'icon-alt': 'mdi-chevron-down',
                //     text: 'Labels',
                //     model: true,
                //     children: [
                //         { icon: 'mdi-plus', text: 'Create label' },
                //     ],
                // },
                // {
                // icon: 'mdi-chevron-up',
                // 'icon-alt': 'mdi-chevron-down',
                // text: 'More',
                // model: false,
                // children: [
                //     { text: 'Import' },
                //     { text: 'Export' },
                //     { text: 'Print' },
                //     { text: 'Undo changes' },
                //     { text: 'Other contacts' },
                // ],
                // },
                // { icon: 'mdi-settings', text: 'Settings' },
                // { icon: 'mdi-message', text: 'Send feedback' },
                // { icon: 'mdi-help-circle', text: 'Help' },
                // { icon: 'mdi-cellphone-link', text: 'App downloads' },
                // { icon: 'mdi-keyboard', text: 'Go to the old version' },
            ],
        }
    }
}
</script>