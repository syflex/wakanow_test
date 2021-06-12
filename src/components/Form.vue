<template>
  <div>

    <b-button size="sm" v-b-modal.my-modal class="mr-1">
        Edit
    </b-button>
  
     <!-- Info modal -->
    <b-modal id="my-modal" :title="'register modal'" ok-only @hide="resetInfoModal">
     <b-form @submit="onSubmit" @reset="onReset">
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

      <b-form-group label="Weight(g):">
        <b-form-select
          v-model="form.weight"
          :options="weights"
           placeholder="Frog Weight"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group label="Length (in):">
        <b-form-select
          v-model="form.length"
          :options="lengths"
           placeholder="Frog Length"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group label="Width(in):">
        <b-form-select
          v-model="form.width"
          :options="widths"
          placeholder="Frog Width"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group label="Sex:">
        <b-form-select
          v-model="form.sex"
           :options="gender"
          placeholder="Frog Sex"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group label="Sex" v-slot="{ ariaDescribedby }">
        <b-form-checkbox-group
          v-model="form.sex"
          :aria-describedby="ariaDescribedby"
        >
          <b-form-checkbox value="me">Male</b-form-checkbox>
          <b-form-checkbox value="that">Female</b-form-checkbox>
        </b-form-checkbox-group>
      </b-form-group>

       <b-form-group label="Live Cycle:">
        <b-form-select
          v-model="form.live_cycle"
           :options="live_cycles"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group label="Description:">
        <b-form-textarea
          v-model="form.description"
        ></b-form-textarea>
      </b-form-group>

      

      <b-button type="submit" variant="primary">Register Frog</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
      <b-button type="reset" variant="danger">Cancel</b-button>

    </b-form>
    
    </b-modal>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      infoModal: {
        id: 'info-modal',
        title: '',
        content: ''
      },
      form: {
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
    
    info(item, index) {
        this.infoModal.title = `Row index: ${index}`
        this.infoModal.content = JSON.stringify(item, null, 2)
    },

    async onSubmit(event) {
        event.preventDefault()
        const req = await this.axios.post('frog/create.php', this.form)
        console.log(req);
    },

      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.food = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }

  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
