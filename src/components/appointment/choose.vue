<template>
    <div>
        <v-card class="mx-auto mb-7 mt-2" height="500px" max-width="480" color="blue-grey lighten-5">
            <v-card-text>
                <v-row align="center" justify="center" class="mt-1">
                    <v-btn-toggle mandatory block v-model="chooseAppSelect">
                        <v-btn>
                            Service Category
                        </v-btn>
                        <v-btn>
                            Staff Prefered
                        </v-btn>
                        <v-btn>
                            Schedule
                        </v-btn>
                    </v-btn-toggle>
                </v-row>
            </v-card-text>

            <v-alert v-if="error_message !== ''" border="left" colored-border color="red accent-4" style="padding:5px;">
                {{ error_message }}
            </v-alert>

            <v-card max-width="450" class="mx-auto mt-3 overflow-y-auto" max-height="400" v-if="chooseAppSelect === 0">
                <v-list v-if="serviceCategory">
                    <v-list-item-group v-model="selectedService">
                        <v-list-item v-for="(item, i) in serviceCategory" :key="i">
                            <v-list-item-content>
                                <v-list-item-title>{{ item.name }}</v-list-item-title>
                            </v-list-item-content>
                        </v-list-item>
                    </v-list-item-group>
                </v-list>
            </v-card>

            <v-card max-width="450" class="mx-auto mt-3 overflow-y-auto" max-height="400" v-if="chooseAppSelect === 1">
                <v-list v-if="staff">
                    <v-list-item-group v-model="selectedStaff">
                        <v-list-item v-for="(item, i) in staff" :key="i" v-show="checkAvailableStaff(item)">
                            <v-list-item-content>
                                <v-list-item-title>{{ item.name }}</v-list-item-title>
                            </v-list-item-content>
                        </v-list-item>
                    </v-list-item-group>
                </v-list>
            </v-card>

            <v-alert border="right" colored-border type="error" elevation="2" 
                v-if="chooseAppSelect === 2 && (selectedService === null || selectedStaff === null)" class="ml-4 mr-4 mt-4">
                Please select first a service and a staff before selecting a schedule
            </v-alert>
            <v-card class="mx-auto mt-3 overflow-y-auto" max-height="400" v-else-if="chooseAppSelect === 2">
                <v-row no-gutters>
                    <v-col cols="12" sm="6" md="8" max-height="400">
                        <v-card class="pa-2" outlined tile>
                            <v-date-picker md="8" v-model="dateSchedule" show-adjacent-months :allowed-dates="allowedDates" :min="new Date().toISOString().slice(0, 10)" @click:date="changedateSched()"></v-date-picker>
                        </v-card>
                    </v-col>
                    <v-col cols="6" md="4" class="time_select">
                        <v-card class="pa-2 overflow-y-auto" max-height="400">
                            <v-btn v-for="(x, key) in listTime(staff[selectedStaff].schedule.filter(x=>x.date == dateSchedule))" :key='key' block class="mb-3" v-bind:class="timeSchedule === x ? 'blue-grey white--text' : ''" @click="timeSchedule = x">{{ x }}</v-btn>
                        </v-card>
                    </v-col>
                </v-row>
            </v-card>
        </v-card>

        <v-btn text class="float-left" href='/'>
            Back to home
        </v-btn>

        <v-btn color="primary" @click="gotoInfo()" class="float-right mr-2">
            Next
        </v-btn>
    </div>
</template>

<script>
    import serviceCategory from "/spec/services.json";
    import staff from "/spec/staff.json";

    export default {
        name: "Default",
        data: () => ({
            chooseAppSelect: null,
            error_message: '',
            selectedService: null,
            selectedStaff: null,
            serviceCategory: serviceCategory,
            staff: staff,
            dateSchedule: new Date().toISOString().substr(0, 10),
            timeSchedule: null,
        }),
        watch: {
            selectedService: function (val) {
                this.selectedStaff = null;
            }
        },
        methods: {
            checkAvailableStaff(staff){
                if(this.selectedService === null){
                    return true;
                } else {
                    return this.serviceCategory[this.selectedService].staff.includes(staff.id);
                }
            },
            allowedDates(val){
                if([...new Set(this.staff[this.selectedStaff].schedule.map(x=>x.date))].includes(val)){
                    return true;
                } else {
                    return false;
                }
            },
            listTime(timeRange){
                let timeList = [];
                
                for(let x=0; x<timeRange.length; x++){
                    let startArr = timeRange[x].start.split(':');
                    let startNumb = (parseInt(startArr[0])*3600) + (parseInt(startArr[1])*60) + parseInt(startArr[2]);

                    // make it exact by 15 min
                    if(startNumb%900 !== 0){
                        startNumb = (startNumb + (900 - (startNumb%900)));
                    }

                    let endArr = timeRange[x].end.split(':');
                    let endNumb = (parseInt(endArr[0])*3600) + (parseInt(endArr[1])*60) + parseInt(endArr[2]);

                    for(let t=startNumb; t<endNumb; t+=900){
                        let newH = Math.floor(t/3600);
                        let newM = Math.floor((t%3600)/60);
                        let newS = Math.floor(((t%3600)%60));

                        let newTime = ("0" + newH).slice(-2) + ':' + ("0" + newM).slice(-2) + ':' + ("0" + newS).slice(-2);

                        timeList.push(newTime);
                    }
                }
                return timeList;
            },
            changedateSched(){
                this.timeSchedule = null;
            },
            gotoInfo(){
                if(this.selectedService === '' || this.selectedService === null || this.selectedService === undefined){
                    this.error_message = 'Please select a service';
                } else if(this.selectedStaff === '' || this.selectedStaff === null || this.selectedStaff === undefined){
                    this.error_message = 'Please select a staff';
                } else if(this.dateSchedule === '' || this.dateSchedule === null || this.dateSchedule === undefined || this.timeSchedule === '' || this.timeSchedule === null || this.timeSchedule === undefined){
                    this.error_message = 'Please select a schedule date and time';
                } else {
                    this.error_message = '';
                    this.$emit('next', this.staff[this.selectedStaff].name, this.serviceCategory[this.selectedService].name, this.dateSchedule, this.timeSchedule);
                }
            },
        }
    }
</script>