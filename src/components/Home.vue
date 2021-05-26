<template>
    <v-container fill-height fluid class="text-center">
        <v-row align="center" justify="center">
            <v-col md="4" sm="6">
                <div class="my-2 mt-7">
                    <v-btn large block >
                        Login
                    </v-btn>
                </div>
                <div class="my-2 mt-7">
                    <v-btn large block >
                        Register
                    </v-btn>
                </div>
                <div class="my-2 mt-7">
                    <v-btn large block href='/appointment'>
                        Book an appointment
                    </v-btn>
                </div>
            </v-col>
        </v-row>
    </v-container>
</template>

<script lang="ts">
    export default({
        name: "Home",

        data: () => ({

        }),
    });
</script>
