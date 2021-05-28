<template>
    <v-app>
        <v-app-bar app color="primary" dark>
            <div class="d-flex align-center">
                <strong>Booking Appointment</strong>
            </div>

            <v-spacer></v-spacer>

            <template v-if="userid !== '' && userid !== null && userid !== undefined">
                id: {{ customer.filter(x=>x.id == userid)[0].name }}
            </template>
        </v-app-bar>

        <v-main>
            <Appointment v-if="param === 'appointment'" />
            <Home v-else />
        </v-main>
    </v-app>
</template>

<script lang="ts">
    import Vue from "vue";
    import Home from "./components/Home.vue";
    import Appointment from "./components/Appointment.vue";
    import customer from "/spec/customer.json";

    export default Vue.extend({
        name: "App",

        components: {
            Home,
            Appointment,
        },
        data() {
            return {
                param: window.location.href.split('/')[window.location.href.split('/').length-1],
                userid: '',
                customer: customer
            }
        },
        created(){
            this.userid = localStorage.getItem("user_id");
        }
    });
</script>
