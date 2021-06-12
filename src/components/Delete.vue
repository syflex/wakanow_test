<template>
    <div>
        <b-button size="sm" @click="deleteFrog()" class="mr-1">
            Delete
        </b-button>
    </div>
</template>

<script>
export default {
    props:['id'],

    methods: {
        async deleteFrog(){
            const req = await this.axios.post('frog/delete.php', {id: this.id})
            console.log(req);
        }
    },
}
</script>