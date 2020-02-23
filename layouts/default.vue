<template>
    <!-- App.vue -->
    <v-app>
        <v-navigation-drawer v-model="drawer" :clipped="$vuetify.breakpoint.lgAndUp" app>
        <!-- -->
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
            drawer: null
        }
    }
}
</script>