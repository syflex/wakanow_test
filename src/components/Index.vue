<template>
  <div>
    <h4>{{msg}}</h4>

    <b-card>

    <Form :title="'New'" :modal_name="'modal-0'"/>

    <b-table striped responsive hover caption-top :items="data" :fields="fields" :busy="isBusy" class="mt-4">
       <template #table-busy>
        <div class="text-center text-danger my-2">
          <b-spinner class="align-middle"></b-spinner>
          <strong>Loading...</strong>
        </div>
      </template>
      <template #cell(actions)="row">
        <b-row>
          <b-col md="4">
            <Update :title="'Edit'" :modal_name="'modal-'+row.item.id" :content="row.item" :id="row.item.id"/>
          </b-col>
          <b-col  md="4">
            <b-button size="sm" variant="danger" @click="deleteFrog(row.item.id)" class="mr-1">
                  Delete
              </b-button>
          </b-col>
        </b-row>
      </template>
    </b-table>
    </b-card>
  </div>
</template>

<script>
import Form from './Form.vue'
import Update from './Update.vue'
export default {
  name: 'Home',
  props: {
    msg: String
  },
  components:{
    Form,
    Update
  },
  data() {
    return {
      isBusy: false,
      fields: ['number','color','weight','length','width','sex','live_cycle','description','actions'],
      data:[],
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

    async deleteFrog(id){            
        try {
          this.axios.post('frog/delete.php', {id: id})
          alert('Deleted successfully'); 
          this.getFrogs();
          setTimeout(() => {
            
          }, 5000);
        } catch (error) {
            alert("An error accured: Kindly try again")
        }
    }

  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
