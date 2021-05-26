<template>
    <div>
        <v-card height="350px" class="mx-auto mb-7 mt-2" max-width="480">
          
            <div class="text-h4 ml-5 text-left" style="padding-top:25px;">
                {{ service }}
            </div>
            <div class="text-subtitle-1 ml-5 text-left">
                Staff: {{ staff }}
            </div>


            <div class="text-h6 ml-5 text-left" v-if="dateSched !== null">{{ wordFormat(dateSched) }}</div>

            <div class="text-h6 ml-5 text-left">{{ timeSched }}</div>

            <div class="mt-5 text-left mb-5">
                <v-btn elevation="2" class="ml-5" dark>Cancel</v-btn>
                <v-btn elevation="2" class="ml-5" dark>Reschedule</v-btn>
            </div>

            <v-divider class="ml-5 mr-5"></v-divider>


            <div class="mt-5 text-left mb-5">
                <v-btn elevation="2" class="ml-5" small href="https://www.icloud.com/calendar"> 
                    <v-icon style="font-size:18px">mdi-star</v-icon>
                    Add to iCal
                </v-btn>
                <v-btn elevation="2" class="ml-5" small href='https://www.google.com'> 
                    <v-icon style="font-size:18px">mdi-star</v-icon>
                  Add to google
              </v-btn>
            </div>

            <div class="mt-5 text-left mb-5">
                <v-btn small color="blue lighten-3" elevation="2" class="ml-5">
                    <v-icon left>
                        mdi-pencil
                      </v-icon>
                    Register for an account
                </v-btn>
            </div>
        </v-card>

        <v-btn color="primary" class="float-right">
            Schedule another
        </v-btn>
    </div>
</template>

<script>
    import serviceCategory from "/spec/services.json";
    import staff from "/spec/staff.json";

    export default {
        name: "Default",
        data: () => ({
        }),
        methods: {
            wordFormat(dat){
                let arr_date = dat.split('-');
                let months   = ['', 'January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

                return months[parseInt(arr_date[1])] + ' ' + arr_date[2] + ', ' + arr_date[0];
            },
        },
        props:{
            staff:{
                type:String,
            },
            service:{
                type:String,
            },
            dateSched:{
                type:String,
            },
            timeSched:{
                type:String,
            },
        }
    }
</script>