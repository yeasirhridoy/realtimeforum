<template>
    <v-card>
        <v-container fluid>
            <v-form @submit="update">
                <v-text-field
                        label="Title"
                        v-model="form.title"
                        type="text"
                        required
                ></v-text-field>

                <markdown-editor v-model="form.body"></markdown-editor>

                <v-card-actions>
                    <v-btn icon small type="submit">
                        <v-icon color="teal">save</v-icon>
                    </v-btn>
                    <v-btn icon small @click="cancel">
                        <v-icon color="black">cancel</v-icon>
                    </v-btn>
                </v-card-actions>
            </v-form>
        </v-container>
    </v-card>
</template>
<script>
    export default {
        props:['question'],
        data() {
            return {
                form: {
                    title: null,
                    body: null,
                    slug: null,
                }
            }
        },
        methods:{
            cancel(){
                EventBus.$emit('cancelEditing')
            },
            update(){
                axios.patch(`/api/question/${this.question.slug}`,this.form)
                    .then(res => this.cancel())
            }
        },
        created(){
            this.form = this.question
        }

    }
</script>