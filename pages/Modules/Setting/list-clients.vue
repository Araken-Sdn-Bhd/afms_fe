
<template>
    <div id="layoutSidenav">
      <CommonSidebar />
      <div id="layoutSidenav_content">
        <CommonHeader />
        <main>
          <div class="container-fluid px-4">
            <div class="page-title">
              <h1>Client Management</h1>
            </div>
            <div class="card mb-4">
              <div class="card-body">
                <div class="row mb-4 align-items-center">
                  <div class="col-md-6">
                    <label for="" class="form-label">Client Name</label>
                    <input
                      type="text"
                      class="form-control"
                      placeholder="Enter Client Name"
                      v-model="clientName"
                    />
                  </div>

                  <div class="col-md-6">
                    <label for="" class="form-label">Status</label>
                    <select 
                      v-model="clientStatus"
                      class="form-select"
                      aria-label="Default select example"
                    >
                      <option value="1">Active</option>
                      <option value="0">Inactive</option>
                    </select>
                  </div>
              </div>
              <div class="d-flex justify-content-center">
                    <button type="submit" class="btn btn-success btn-text" v-if="editID" @click="addClient">
                      <i class="fa fa-save"></i> Update
                    </button>
                    <button type="submit" class="btn btn-success btn-text" v-if="!editID" @click="addClient">
                      <i class="fa fa-plus"></i> Add New
                    </button>
                  </div>
              </div>
            </div>
  
            <div class="card mb-4">
                <div class="card-header icon-title">
                            <a href="#"><i class="fa fa-list"></i></a>
                            <h4>List of Clients</h4>
                </div>
              <div class="card-body">
                <!-- search-table -->
                <div style="overflow-x:auto;">
                  <table class="table table-striped data-table display nowrap" style="width: 100%">
                  <thead>
                    <tr>
                      <th>No</th>
                      <th>Client</th>
                      <th>Status</th>
                      <th style="width:5%">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(clt,index) in clientlist" :key="index">
                      <td>{{ index+1}}</td>
                      <td>{{ clt.client_name }}</td>
                      <td v-if="clt.client_status == 1">Active</td>  
                      <td v-if="clt.client_status == 0">Inactive</td>
                      <td>
                        <a @click="editRecord(clt)" class="action-icon icon-success" title="Edit Record"><i class="fa fa-edit"></i></a>
                        <a @click="deleteRecord(clt)" title="Delete Record" class="action-icon icon-danger"><i class="fa fa-trash-alt"></i></a>
                      </td>
                    </tr>
                  </tbody>
                </table>
                <!-- <p v-if="filelist.length == 0" style=" padding: 0px; margin: 10px;color: red;display: flex;justify-content: center;">No Record Found</p> -->
                </div>
              </div>
            </div>
          </div>
        </main>
      </div>
    </div>
  </template>
  <script>
  import swal from 'sweetalert2';
  import moment from 'moment';
  import CommonHeader from '../../../components/CommonHeader.vue';
  import CommonSidebar from '../../../components/CommonSidebar.vue';
  export default {
    components: { CommonSidebar, CommonHeader },
    name: "list-client",
    
    
    data() {
      return {
        alllist: [],
        clientlist: [],
        userdetails:null,
        clientName: "",
        clientStatus: "",
        editID: 0,
       
      };
    },
    beforeMount() {
      this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
      this.GetClientList();
    },

    mounted() {
    },

    methods: {
      async GetClientList(){
        const headers = {
          Authorization: "Bearer " + this.userdetails.access_token,
          Accept: "application/json",
          "Content-Type": "application/json",
        };
        const response = await this.$axios.post(
          "clients/clientList",
          {headers}
        );

        if(response.data.code == 200){
          this.clientlist = response.data.list;
        }
      },

      async addClient(){
        try{
            this.loader = true;
            const headers = {
            Authorization: "Bearer " + this.userdetails.access_token,
            Accept: "application/json",
            "Content-Type": "application/json",
            };

            let body = new FormData();

            body.append("editId", this.editID);
            body.append("client_name", this.clientName);
            body.append("client_status", this.clientStatus);

            const response = await this.$axios.post(
            "clients/clientStore",
            body,{headers}
            );
            
            if(response.data.code == 200 || response.data.code == '200'){
            this.loader = false;
            swal.fire('Record Successfully Saved', '', 'success')
            this.resetModel();
            this.GetClientList();
            }
            else {
                    this.loader = false;
                    swal.fire({
                        icon: 'error',
                        title: 'Oops... Something Went Wrong!',
                        text: 'the error is: ' + JSON.stringify(response.data.message),
                    })
            
                }
        }catch(e){
            swal.fire({
                        icon: 'error',
                        title: 'Oops... Something Went Wrong!',
                        text: 'the error is: ' + e,
                    })
        }
        },
      
      async deleteRecord(data){
        swal.fire({
                    title: 'Are you sure to delete this record?',
                    showCancelButton: true,
                    confirmButtonText: 'Delete',
                    }).then(async (result) => {
                    if (result.isConfirmed) {
                    try {
                        this.loader = true;
                        const headers = {
                                Authorization: "Bearer " + this.userdetails.access_token,
                                Accept: "application/json",
                                "Content-Type": "application/json",
                            };
                            const response = await this.$axios.post(
                                "clients/deleteClient",{ 
                                client_id: data.client_id,
                                },
                                { headers }
                            );
                            if (response.data.code == 200 || response.data.code == '200') {
                                this.GetClientList();
                                this.loader = false;
                                swal.fire('Record Successfully Deleted', '', 'success')
                            } else {
                                this.loader = false;
                                swal.fire({
                                    icon: 'error',
                                    title: 'Oops... Something Went Wrong!',
                                    text: 'the error is: ' + JSON.stringify(response.data.message),
                                })
                        
                            }
                    } catch (e) {
                        this.loader = false;
                            swal.fire({
                                icon: 'error',
                                title: 'Oops... Something Went Wrong!',
                                text: 'the error is: ' + e,
                            })
                        }
                    } else if (result.isDismissed) {
                        swal.fire('Record are not saved', '', 'info')
                    }
                })

      },

      resetModel(){
        this.clientName='',
        this.clientStatus='',
        this.editID=''
      },

      async editRecord(data){
        this.editID=data.client_id;
        this.clientName=data.client_name;
        this.clientStatus=data.client_status;
      },

      getFormattedDate(date) {
            return moment(date).format("DD/MM/YYYY")
      },
    
    }
  };
  </script>
  <style scoped>
  .file-upload>input {
    display: none;
    visibility:hidden;
    width:0;
    height:0
    }
  </style>
  