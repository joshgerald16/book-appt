<template>
    <v-container fill-height fluid class="text-center">
        <v-row align="center" justify="center">
            <v-stepper v-model="step" width="550">
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

<style>
    .v-stepper__label{
        display: inline-block !important;
    }

    @media only screen and (max-width: 580px) {
      .v-stepper__step__step{
            display: none !important;
        }
    }


    @media only screen and (max-width: 480px) {
        body{
            overflow: hidden;
        }
        .v-stepper__step{
            display: none !important;
        }

        .v-stepper .v-divider{
            display: none;
        }

        .v-stepper__step--active.v-stepper__step{
        display: flex !important;
        }

        .v-item-group{
            display: block;
        }
        .v-item-group .v-btn{
            width: 100% !important;
        }

        .container {
            align-items: flex-start !important;
            margin:0px;
            padding:0px;
        }

        .container .row {
            height: 100%;
            margin-top: 5px;
        }

        .container .row .v-stepper{
            height: 100%;
            width: calc(100% + 12px) !important;
            position: absolute;
            left: 0px;
            top: 0px;
        }

        .container .row .v-stepper .v-stepper__items{
            height: calc(100% - 28px);
            width: 100vw;
        }

        .container .row .v-stepper .v-stepper__content{
            height: calc(100% - 35px);
            margin: 0px;
            padding: 0px !important;
        }

        .container .row .v-stepper .v-stepper__content .v-stepper__wrapper{
            height: 100%;
        }

        .container .row .v-stepper .v-stepper__content .v-stepper__wrapper>div{
            height: 100%;
        }

        .container .row .v-stepper .v-stepper__content .v-stepper__wrapper>div>.v-sheet{
            height: calc(100% - 75px) !important;
        }

        .v-picker, .v-picker>.v-picker__body{
            float: left;
            width: 250px !important;
        }

        .row.no-gutters{
            display: contents;
        }

        .time_select{
            width: calc(100% - 259px) !important;
            float: right !important;
            margin-top: -18px;
        }

        .v-card__text{
            padding-top: 0px;
        }
    }
</style>