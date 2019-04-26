<template>
    <div class="contacts-list">
        <ul>
            <li 
                v-for="contact in sortContact" 
                :key="contact.id" 
                @click="selectContact(contact)"
                :class="{ 'selected':contact == selected}">
                <div class="avatar">
                    <img :src="contact.profile_img" :alt="contact.name">
                </div>
                <div class="contact">
                    <p class="name">{{contact.name}} </p>
                    <p class="email">{{contact.email}} </p>
                </div>
                <span class="unread" v-if="contact.unread">{{contact.unread}}</span>
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    props:{
        contacts:{
            type: Array,
            defualt: []
        }
    },data() {
        return {
            selected: this.contacts.length ? this.contacts[0] : null
        }
    },computed: {
        sortContact(){
            return _.sortBy(this.contacts , [(contact)=>{
                if (contact == this.selected) {
                    return Infinity;
                }
                return contact.unread;
            }]).reverse();
        }
    },
    methods: {
        selectContact(contact){
            
            this.selected = contact;
            this.$emit('selected', contact);
        }
    },
}
</script>