<template>
    <div>
        <b-overlay :show="busy" rounded opacity="0.6"
            spinner-small spinner-variant="primary"
            class="d-inline-block"
        >
        <b-button size="sm" @click="deleteFrog()" class="mr-1" :disabled="busy">
            Delete
        </b-button>
        </b-overlay>
    </div>
</template>

<script>
export default {
    props:['id'],
    data() {
        return {
            busy: false,
        }
    },
    methods: {
        async deleteFrog(){
            this.busy = true;
           try {
                this.axios.post('frog/delete.php', {id: this.id})
                alert('Deleted successfully');                
                this.busy = false;
           } catch (error) {
               alert("An error accured: Kindly try again")
                this.busy = false;
           }
        }
    },
}
</script>