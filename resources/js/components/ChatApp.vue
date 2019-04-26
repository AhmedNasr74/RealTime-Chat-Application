<template>
    <div class="chat-app">
        <Conversation :contact="selectedContact" :messages="messages" @new="saveNewMessage"/>
        <ContactList :contacts="contacts" @selected="startConversationWith"/>
    </div>
</template>
<script>
    import Conversation from './Conversation';
    import ContactList from './ContactList';
    
    export default {
        props:{
            user:{
                type: Object,
                required: true
            }
        },
        data() {
            return {
                selectedContact:null,
                messages:[],
                contacts:[],
                }
        },
        methods: {
            startConversationWith(contact){
                this.updateUnreadcount(contact , true)
                axios.get(`conversation/${contact.id}`)
                .then((response)=>{
                    this.messages = response.data;
                    this.selectedContact = contact
                })
            },
            updateUnreadcount(contact , reset){
                    this.contacts = this.contacts.map((single)=>{
                        if(single.id != contact.id)
                            return single;
                        if (reset) 
                            single.unread = 0;
                        else
                            single.unread++;
                        return single;
                    })
            },
            saveNewMessage(message){
                this.messages.push(message);
            },
            handleIncoming(message){
                if (this.selectedContact && message.from == this.selectedContact.id) {
                    this.saveNewMessage(message)
                }
                this.updateUnreadCount(message.from_contact, false);
            }
        },
        mounted() {
            Echo.private(`messages.${this.user.id}`)
            .listen('NewMessage',(e)=>{
                this.handleIncoming(e.message)
            });

            axios.get('/contacts')
            .then((response)=>{
                this.contacts = response.data;
            })
        },components:{Conversation,ContactList}
    }
</script>