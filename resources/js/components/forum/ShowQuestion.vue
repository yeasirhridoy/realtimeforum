<template>
    <v-card>
        <v-container fluid>
            <v-card-title>
                <!--Header of the question-->
                <div>
                    <div class="headline">
                        {{question.title}}
                    </div>
                    <div class="grey--text">
                        {{question.user}} said {{question.created_at}}
                    </div>
                </div>
                <v-spacer></v-spacer>
                <v-btn color="teal" >{{question.reply_count}} Replies</v-btn>
            </v-card-title>

            <!--Body of the question-->
            <v-card-text v-html="body"></v-card-text>

            <!--Edit button-->
            <v-card-actions v-if="own">
                <v-btn icon small @click="edit">
                    <v-icon color="orange">edit</v-icon>
                </v-btn>
                <v-btn icon small @click="destroy">
                    <v-icon color="red">delete</v-icon>
                </v-btn>
            </v-card-actions>
        </v-container>
    </v-card>
</template>
<script>
    import md from 'marked'
    import User from "../../helpers/User";
    export default {
        props:['question'],
        data(){
            return {

            }
        },
        computed:{
            body(){
                return  md.parse(this.question.body);
            },
            own(){
                return  User.own(this.question.user_id);
            }
        },
        methods:{
            destroy(){
                axios.delete(`/api/question/${this.question.slug}`)
                    .then(res => this.$router.push('/forum'))
                    .catch(error => console.log(error.response.data))
            },
            edit(){
                EventBus.$emit('startEditing');
            }
        }
    }
</script>