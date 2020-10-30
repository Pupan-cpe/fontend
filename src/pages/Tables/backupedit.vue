<template>
  <v-data-table
    :headers="headers"
    :items="users"
   
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Manage User</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
              New Item
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="email1"
                      label="email"
                    ></v-text-field>
                  </v-col>
                   <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="fullname1"
                      label="Fullname"
                    ></v-text-field>
                  </v-col>
                
               
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">
                Cancel
              </v-btn>
              <v-btn color="blue darken-1" text @click="save">
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline"
              >Are you sure you want to delete this item?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete"
                >Cancel</v-btn
              >
              <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                >OK</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>

    <template v-slot:[`item.actions`]="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)">
        mdi-pencil
      </v-icon>
      <v-icon small @click="deleteItem(item)">
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
     
    </template>
  </v-data-table>
</template>

<script>

import axios from "axios";
import swal from 'sweetalert';

export default {
  data: () => ({
    users: [],
    doh:{
      fullname1:"",
      email1:""

    },
    loading: false,
    setIntervalId: null,
    dialog: false,
    dialogDelete: false,
    editedItem: {
      email: "",
      fullname: ""
    },
    defaultItem: {
      email: "",
      fullname: ""
    },
    headers: [
       {
            
            
            align: 'start',
            filterable: false,
            value: 'id',
           
          },
      { text: "Email", value: "email" },
      { text: "Fullname", value: "fullname" },
     

      { text: "Actions", value: "actions", sortable: false },
    ],
    desserts: [],
    editedIndex: -1,
    
    
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
    
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
     this.getUsers();
    this.setIntervalId = setInterval(() => {
      this.getUsers();
    }, 1000);
 
  },
   beforeDestroy() {
    clearInterval(this.setIntervalId);
  },

  methods: {
 
  deleteData: function() {
        axios.delete('http://192.168.1.138:3000/api/authen/delete/'+ this.editedItem.id)
        .then(res => {
          
         if(res.data.status ===  "true"){
           swal("ลบสำเร็จ" ,"", "success");

         }
         
          
        });
      },

      editdata: function() {
        axios.post('http://192.168.1.138:3000/api/authen/update/'+ this.editedItem.id ,this.doh )
        .then(res => {
          console.log(this.doh);
          var myJSON = JSON.stringify(this.doh);
          console.log(myJSON);
          console.log(typeof(this.editedItem));
         if(res.data.status ===  "true"){
           swal("อัพเดทสำเร็จ" ,"", "success");
           

         }
         else{
           console.log("error 404");
         }
        
          
        });
      },
    
    async getUsers() {
      // this.loading = true;
      try {
        const users = await axios.get(
          "http://192.168.1.132:3000/api/authen/table-all"
        );
        this.users = users.data.data;
        
        this.loading = false;
        console.log();
      } catch (err) {
        console.log(err); // handle errors here...
      }
    },
   
    editItem(item) {
      
      this.editedIndex = this.users.indexOf(item);
      this.editedItem = Object.assign([], item);
      this.dialog = true;
     
    },

    deleteItem(item) {
      this.editedIndex = this.users.indexOf(item);
      this.editedItem = Object.assign([], item);
      this.ids = this.editedItem.id
      console.log(this.ids);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.deleteData();
      this.users.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
        
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      this.editdata(); 
      if (this.editedIndex > -1) {
        
        Object.assign(this.users[this.editedIndex], this.defaultItem);
         
      } else {
        this.users.push(this.editedItem);
        
      }
      this.close();
    },
  },
};
</script>
