<template>
    <v-container fill-height fluid class="text-center">
        <v-row align="center" justify="center">
            <v-stepper v-model="step">
                <v-stepper-header style="height: 50px;">
                    <v-stepper-step :complete="step > 1" step="1" class="pt-0 pb-0">
                        Choose Appointment
                    </v-stepper-step>

                    <v-divider></v-divider>

                    <v-stepper-step :complete="step > 2" step="2"  class="pt-0 pb-0">
                        Your Information
                    </v-stepper-step>

                    <v-divider></v-divider>

                    <v-stepper-step step="3" class="pt-0 pb-0">
                        Confirmation
                    </v-stepper-step>
                </v-stepper-header>

                <v-stepper-items>
                    <v-stepper-content step="1" style="padding:10px;">
                        <choose v-on:next="nextChoose" />
                    </v-stepper-content>

                    <v-stepper-content step="2">
                        <info v-on:next="step=3" v-on:prev="step=1" />
                    </v-stepper-content>

                    <v-stepper-content step="3">
                        <confirmation :staff="staff" :service="service" :dateSched="dateSched" :timeSched="timeSched" />
                    </v-stepper-content>
                </v-stepper-items>
            </v-stepper>
        </v-row>
    </v-container>
</template>

<script lang="ts">
    import choose from "/src/components/appointment/choose.vue";
    import info from "/src/components/appointment/info.vue";
    import confirmation from "/src/components/appointment/confirmation.vue";

    export default({
        name: "Appointment",
        components: {
            choose,
            info,
            confirmation
        },
        data: () => ({
            step: 1,
            staff: null,
            service: null,
            dateSched: null,
            timeSched: null,
        }),
        methods: {
            nextChoose(staff, service, dateSched, timeSched){
                this.staff = staff;
                this.service = service;
                this.dateSched = dateSched;
                this.timeSched = timeSched;

                this.step = 2;
            }
        },
    });
</script>
