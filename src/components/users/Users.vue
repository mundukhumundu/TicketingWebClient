<template>
  <v-container grid-list-md fluid>
    <v-layout row wrap v-if="loading">
      <v-flex xs12 class="text-xs-center">
        <v-progress-circular indeterminate class="primary--text"
                             :width="7"
                             :size="70"
        ></v-progress-circular>
      </v-flex>
    </v-layout>
  <v-layout row wrap v-else>
    <v-flex xs12 sm6>
      <h2 class="secondary--text">Our Customers</h2>
    </v-flex>
    <v-flex xs12>
      <v-card>
        <v-card-title>
          <add-user></add-user>
          <v-spacer></v-spacer>
          <v-text-field
            name="search"
            label="Search User"
            v-model="search"

            append-icon="search"></v-text-field>
        </v-card-title>
      <v-card-text>
        <v-layout row wrap>
          <v-flex xs12>
            <v-data-table
            :headers="headers"
            :items="customers"
            :search="search">
              <template slot="items" slot-scope="props">
                <td>{{props.item.uid}}</td>
                <td class="text-xs-center">{{props.item.email!==undefined ? props.item.email : props.item.phoneNumber}}</td>
                <td class="text-xs-center">{{props.item.metadata.creationTime!==undefined ? props.item.metadata.creationTime: null}}</td>
                <td class="text-xs-center">{{props.item.metadata.lastSignInTime ? props.item.metadata.lastSignInTime :null}}</td>
                <td class="justify-center layout px-0">
                  <v-icon
                    style="cursor: pointer;"
                    small
                    class="mr-2"

                  >
                    edit
                  </v-icon>
                  <v-icon
                    small
                  >
                    delete
                  </v-icon>
                </td>
              </template>
            </v-data-table>
          </v-flex>
        </v-layout>
      </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
  </v-container>
</template>
<script>
  export default{
    data(){
      return{
        search:'',
        headers:[
          {
            text:'uid',
            align:'left',
            sortable:true,
            value:'uid'
          },
          {text:'Email',value:'email'},
          {text:'Created On',value:'creationTime'},
          {text:'Last Signed In',value:'lastSignInTime'},
          {text:'Action',value:'Action'}
        ],
        customers:[
          {
            uid:'',
            emailAddress:'',
            phoneNumber:'',
            creationTime:'',
            lastSignInTime:''
          }
        ]
      }
    },
    async  mounted(){
     await this.$store.dispatch('onFetchCustomerList');
      this.customers = await this.$store.state.customers;
      console.log(this.$store.state.customers);
    },
    computed:{
      loading(){
        return this.$store.getters.loading
      }
    }
  }
</script>
