<template>
  <div>

    <b-button :size="'sm'" variant="success" v-b-modal="modal_name" class="mr-1">
        Edit
    </b-button>


  
     <!-- Info modal -->
    <b-modal :id="modal_name" :title="title === 'New' ? 'Add New Frog'  : title" hide-header-close hide-footer>

     <b-form @submit="onSubmit" @reset="onReset">

        <b-row class="mt-3">
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

            <b-form-group label="Sex" v-slot="{ ariaDescribedby }" class="mt-2">
                <b-form-checkbox-group
                v-model="form.sex"
                :aria-describedby="ariaDescribedby"
                >
                <b-form-checkbox value="me">Male</b-form-checkbox>
                <b-form-checkbox value="that">Female</b-form-checkbox>
                </b-form-checkbox-group>
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
                    <b-button type="submit" variant="primary" :disabled="busy" class="m-2">{{title === "New" ? 'Add ' : "Edit "}}Frog</b-button>
                </b-overlay>
                <b-button type="reset" variant="danger" class="m-2">Cancel</b-button>
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
    modal_name: String,
    id: String,
    content: Object
  },
  data() {
    return {
        busy: false,
        counter: 0,
        form: {
            id: this.content.id ?? this.content.id,
            color:  this.content.color ?? this.content.color,
            weight:  this.content.weight ?? this.content.weight,
            length:  this.content.length ?? this.content.length,
            width:  this.content.width ?? this.content.width,
            sex:  this.content.sex ?? this.content.sex,
            live_cycle:  this.content.live_cycle ?? this.content.live_cycle,
            description:  this.content.description ?? this.content.description,
        },
        weights: [{ text: 'Select Weight', value: null }, '5', '10', '15', '20', '25', '30'],
        lengths: [{ text: 'Select Length', value: null }, '1', '2', '4', '6', '8', '10'],
        widths: [{ text: 'Select Width', value: null }, '1', '2', '3', '4', '5', '6'],
        gender: [{ text: 'Select One', value: null }, 'Male', 'Female'],
        live_cycles: [{ text: 'Select Live Cycle', value: null }, 'Tadpole ', 'Froglet', 'Adult Frog'],
    }
  },

  mounted() {
      this.getFrog();
  },

  methods: {
      
    async getFrog(){
      this.busy = !this.busy
      const req = await this.axios.get('frog/read_single.php?id=' + this.id);
      this.data = req.data
      this.busy = !this.busy
    },

    async onSubmit() {
        this.busy = true
        try {
            const req = await this.axios.post('frog/update.php', this.form)
            console.log(req);
            this.busy = false
            this.$router.go()
        } catch (error) {
            this.busy = false
        }
    },

      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.color = ''
        this.form.weight = ''
        this.form.length = ''
        this.form.sex = ''
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
