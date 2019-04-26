<template>
    <div class="container">
    <div class="row">
            <div class="composer p-0 col-12">
                <textarea 
                class="form-control " 
                placeholder="Type a Message....." 
                v-model="message" @keydown.enter="send"></textarea>
        </div>
    </div>
</div>
    
</template>
<script>
export default {
    data() {
        return {
            message:''
        }
    },
    methods: {
        send(e){
            e.preventDefault();
            
            if (this.message == '') {
                return;
            }
            this.$emit('send',this.message);
            this.message =''
        }
    },
}
</script>