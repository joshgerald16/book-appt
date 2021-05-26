<template>
    <div>
        <v-card height="300px" class="mx-auto mb-7 mt-2" max-width="480">
            <v-form ref="form" v-model="valid" lazy-validation class="pl-5 pr-5" style="padding-top:10px;">
                <v-text-field
                  v-model="name"
                  :counter="50"
                  :rules="nameRules"
                  label="Name"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="email"
                  :rules="emailRules"
                  label="E-mail"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="contact"
                  :rules="contactRules"
                  label="Contact Number"
                  required
                ></v-text-field>
            </v-form>
        </v-card>

        <v-btn text class="float-left" @click="back()">
            Back
        </v-btn>

        <v-btn color="primary" @click="validate()" class="float-right">
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
            valid: true,
            name: '',
            nameRules: [
                v => !!v || 'Name is required',
                v => (v && v.length <= 50) || 'Name must be less than 50 characters',
            ],
            email: '',
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+\..+/.test(v) || 'E-mail must be valid'
            ],
            contact: '',
            contactRules: [
                v => !!v || 'Contact number is required',
                v => /^\d+$/.test(v) || 'Contact number must be valid',
            ],
        }),
        methods: {
            validate () {
                if(this.$refs.form.validate()){
                    this.$emit('next');
                }
            },
            back(){
                this.$emit('prev');
            }
        }
    }
</script>