<template>
  <div>

    <b-button :size="title === 'New' ? '' : 'sm'" variant="success" v-b-modal="modal_name" class="mr-1">
        {{title === 'New' ? 'Add New Frog' : title}} 
    </b-button>
  
     <!-- Info modal -->
    <b-modal :id="modal_name" :title="title === 'New' ? 'Add New Frog'  : title" hide-header-close hide-footer>

     <b-form>

        <b-row class="mt-3">
            <b-form-group
                label="Number(eg 0000):"
            >
                <b-form-input
                v-model="form.number"
                type="text"
                placeholder="Frog Number(0000)"
                required
                ></b-form-input>
            </b-form-group>

            <b-form-group
                label="Color:"
            >
                <b-form-input
                v-model="form.color"
                type="text"
                placeholder="Frog Color"
                required
                ></b-form-input>
            </b-form-group>

            <b-form-group label="Weight(g):" class="mt-2">
                <b-form-select
                type="text"
                v-model="form.weight"
                :options="weights"
                placeholder="Frog Weight"
                required
                ></b-form-select>
            </b-form-group>

            <b-form-group label="Length (in):" class="mt-2">
                <b-form-select
                v-model="form.length"
                :options="lengths"
                placeholder="Frog Length"
                required
                ></b-form-select>
            </b-form-group>

            <b-form-group label="Width(in):" class="mt-2">
                <b-form-select
                v-model="form.width"
                :options="widths"
                placeholder="Frog Width"
                required
                ></b-form-select>
            </b-form-group>

            <b-form-group label="Individual radios" v-slot="{ ariaDescribedby }">
                <b-form-radio v-model="form.sex" :aria-describedby="ariaDescribedby" value="Male">Male</b-form-radio>
                <b-form-radio v-model="form.sex" :aria-describedby="ariaDescribedby" value="Female">Female</b-form-radio>
            </b-form-group>

            <b-form-group label="Live Cycle:" class="mt-2">
                <b-form-select
                v-model="form.live_cycle"
                :options="live_cycles"
                required
                ></b-form-select>
            </b-form-group>

            <b-form-group label="Description:" class="mt-2">
                <b-form-textarea
                v-model="form.description"
                ></b-form-textarea>
            </b-form-group>
        </b-row>

        <b-row class="mt-2">
             
    
            <b-col md="12">
                <b-overlay :show="busy" rounded opacity="0.6"
                    spinner-small spinner-variant="primary"
                    class="d-inline-block"
                >
                    <b-button @click="saveFrog()" variant="primary" :disabled="busy" class="m-2">{{title === "New" ? 'Add ' : "Edit "}}Frog</b-button>
                </b-overlay>
                <b-button @click="onReset()" variant="danger" class="m-2">Cancel</b-button>
            </b-col> 
        </b-row>

    </b-form>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'Form',
  props: {
    title: String,
    modal_name: String
  },
  data() {
    return {
        busy: false,
        counter: 0,
        form: {
            number: '',
            color: '',
            weight: '',
            length: '',
            width: '',
            sex: '',
            live_cycle: '',
            description: ''
        },
        weights: [{ text: 'Select Weight', value: null }, '5', '10', '15', '20', '25', '30'],
        lengths: [{ text: 'Select Length', value: null }, '1', '2', '4', '6', '8', '10'],
        widths: [{ text: 'Select Width', value: null }, '1', '2', '3', '4', '5', '6'],
        gender: [{ text: 'Select One', value: null }, 'Male', 'Female'],
        live_cycles: [{ text: 'Select Live Cycle', value: null }, 'Tadpole ', 'Froglet', 'Adult Frog'],
    }
  },

  methods: {
    async saveFrog() {
        this.busy = true
        try {
            const req = await this.axios.post('frog/create.php', this.form)
            const res = req.data
            if (res.message == 'Frog Registered') {
                console.log(res);
            }
            this.busy = false;
            this.onReset();
        } catch (error) {
            this.busy = false
        }
    },

      onReset() {
        // Reset our form values
        this.form.number = ''
        this.form.color = ''
        this.form.weight = ''
        this.form.length = ''
        this.form.sex = ''
        this.form.live_cycle = ''
        this.form.description = ''
        this.$bvModal.hide(this.modal_name)
        location.reload(); 
      },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
