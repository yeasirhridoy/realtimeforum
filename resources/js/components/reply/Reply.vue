<template>
    <div class="mt-4">
        <v-card>
            <v-card-title>
                <div class="headline">{{ data.user }}</div>
                <div class="ml-2">{{ data.created_at }}</div>
            </v-card-title>
            <v-divider></v-divider>
            <v-card-text v-html="reply"></v-card-text>

            <v-divider></v-divider>

            <v-card-actions v-if="own">
                <v-btn icon small>
                    <v-icon color="orange">edit</v-icon>
                </v-btn>
                <v-btn icon small>
                    <v-icon color="red" @click="destroy">delete</v-icon>
                </v-btn>
            </v-card-actions>
        </v-card>
    </div>
</template>
<script>
    import md from 'marked';
    export default {
        props: ['data','index'],
        data(){
            return {

            }
        },
        computed:{
            own(){
                return User.own(this.data.user_id)
            },
            reply(){
                return md.parse(this.data.reply)
            }
        },
        methods:{
            destroy(){
                EventBus.$emit('destroyReply',this.index)
            }
        }
    }
</script>