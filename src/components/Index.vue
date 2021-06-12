<template>
  <div class="hello">
  <div>
    
    
    <Form />

    
    <b-table :items="items" :busy="isBusy" class="mt-3" outlined>
      <template #table-busy>
        <div class="text-center text-danger my-2">
          <b-spinner class="align-middle"></b-spinner>
          <strong>Loading...</strong>
        </div>
      </template>
    </b-table>
    <b-table striped hover :items="data" :fields="fields">
      <template #cell(actions)="row">
        <Form />
        {{row.item.id}}
        <Delete :id="row.item.id"/>
      </template>
    </b-table>
  </div>


  </div>
</template>

<script>
import Form from './Form.vue'
import Delete from './Delete.vue'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components:{
    Form,Delete
  },
  data() {
    return {
      isBusy: false,
      fields: ['color','weight','length','width','sex','live_cycle','description','actions'],
      data:[],
      infoModal: {
        id: 'info-modal',
        title: '',
        content: ''
      },
      totalRows: 1,
      currentPage: 1,
      perPage: 5,
      pageOptions: [5, 10, 15, { value: 100, text: "Show a lot" }],
      sortBy: '',
      sortDesc: false,
      sortDirection: 'asc',
      filter: null,
      filterOn: [],
    }
  },

  mounted() {
    this.getFrogs();
    // Set the initial number of items
      this.totalRows = this.data.length
  },

  computed: {
      sortOptions() {
        // Create an options list from our fields
        return this.data
          .filter(f => f.sortable)
          .map(f => {
            return { text: f.label, value: f.key }
          })
      }
    },

  methods: {

    async getFrogs(){
      this.isBusy = !this.isBusy
      const req = await this.axios.get('frog/read.php');
      this.data = req.data
      this.isBusy = !this.isBusy
    },

    info(item, index, button) {
        this.infoModal.title = `Row index: ${index}`
        this.infoModal.content = JSON.stringify(item, null, 2)
        this.$root.$emit('bv::show::modal', this.infoModal.id, button)
    },

    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length
      this.currentPage = 1
    }

  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
