<template>
    <v-container fill-height fluid class="text-center">
        <v-row align="center" justify="center">
            <v-col md="4" sm="6">
                <div class="my-2 mt-7" v-if="userid === '' || userid === null || userid === undefined">
                    <v-dialog v-model="dialog" persistent max-width="600px">
                        <template v-slot:activator="{ on, attrs }">
                            <v-btn large block v-bind="attrs" v-on="on">
                                Login
                            </v-btn>
                        </template>
                        <v-card>
                            <v-card-title>
                                <span class="headline">Login</span>
                            </v-card-title>
                            <v-card-text>
                                <v-alert border="left" colored-border elevation="2" color="red" v-if="incorrectLogin">
                                    Invalid username and password
                                </v-alert>
                                <v-container>
                                    <v-row>
                                        <v-col cols="12">
                                            <v-text-field label="Email*" required v-model="email"></v-text-field>
                                        </v-col>
                                        <v-col cols="12">
                                            <v-text-field label="Password*" type="password" required v-model="password"></v-text-field>
                                        </v-col>
                                    </v-row>
                                </v-container>
                            </v-card-text>

                            <v-card-actions>
                                <v-spacer></v-spacer>
                                <v-btn color="blue darken-1" text @click="dialog = false" >
                                    Close
                                </v-btn>
                                <v-btn color="blue darken-1" text @click="validate()" >
                                    Login
                                </v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-dialog>
                </div>
                <div class="my-2 mt-7">
                    <v-btn large block href='/appointment'>
                        Book an appointment
                    </v-btn>
                </div>
                <div class="my-2 mt-7" v-if="userid !== '' && userid !== null && userid !== undefined">
                    <v-btn large block @click="logout">
                        Logout
                    </v-btn>
                </div>
            </v-col>
        </v-row>
    </v-container>
</template>

<script lang="ts">
    import customer from "/spec/customer.json";

    export default({
        name: "Home",

        data: () => ({
            dialog: false,
            email: '',
            password: '',
            customer: customer,
            incorrectLogin: false,
            userid: '',
            nameR: '',
            emailR: '',
            passwordR: '',
            CpasswordR: ''
        }),
        methods: {
            validate(){
                let emailIndex = this.customer.map(x=>x.email).indexOf(this.email);
                if(emailIndex === -1){
                    this.incorrectLogin = true;
                } else {
                    if(this.customer[emailIndex].password === this.password){
                        localStorage.setItem("user_id", this.customer[emailIndex].id);
                        location.reload();
                    } else {
                        this.incorrectLogin = true;
                    }
                }
            },
            logout(){
                localStorage.removeItem("user_id");
                location.reload();
            }
        },
        mounted(){
            this.userid = localStorage.getItem("user_id");
        }
    });
</script>
