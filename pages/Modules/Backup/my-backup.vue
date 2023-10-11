<template>
    <div id="layoutSidenav">
      <CommonSidebar />
      <div id="layoutSidenav_content">
        <CommonHeader />
        <main>
            <div class="container-fluid px-4">
            <div class="page-title">
              <h1>My Backup Files</h1>
              <div class="btn-group-a file-upload">
                <a class="add-btn" title="Upload File"><label for="file-input" class="fa fa-plus">
                </label></a>
                <input id="file-input" type="file" v-on:change="uploadFile">
              </div>
            </div>
  
            <div class="card mb-4">
                <div class="card-header icon-title">
                            <a href="#"><i class="fa fa-list"></i></a>
                            <h4>List of Backups</h4>
                        </div>
              <div class="card-body">
                <div class="search-table">
                  <div class="row mt-2">
                    <div class="col-lg-4 col-sm-6 mb-3">
                        <label for="date_from">FROM:</label>
                        <input type="date" class="form-control" v-model="date_from" @change="onchange($event)" id="date_from"/>
                    </div>
                    <div class="col-lg-4 col-sm-6 mb-3">
                        <label for="date_to">TO:</label>
                        <input type="date" class="form-control" v-model="date_to" @change="onchange($event)" id="date_to"/>
                    </div>
  
                    <div class="col-lg-4 col-sm-6 mb-3">
                        <label for="search">SEARCH:</label>
                      <div class="input-group search-table">
                        <span class="input-group-text bg-transparent br-0"><i class="fa fa-search"></i></span>
                        <input type="text" class="form-control" v-model="filterText" id="search" placeholder="Filter by file name"/>
                      </div>
                    </div>
                  </div>
                </div>
                <!-- search-table -->
                <div style="overflow-x:auto;">
                  <table class="table table-striped data-table display nowrap" style="width: 100%">
                  <thead>
                    <tr>
                      <th>No</th>
                      <th>Date Upload</th>
                      <th>File Name</th>
                      <th style="width:5%">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(file,index) in filelist" :key="index">
                      <td>{{ index+1}}</td>
                      <td>{{ getFormattedDate(file.created_at) }}</td>
                      <td style="color: #0645AD;"><a :href="envURL+file.file_path" target="_blank">{{ file.file_name }}</a></td>
                      <td>
                        <a @click="deleteRecord(file)" title="Delete Record" class="action-icon icon-danger"><i class="fa fa-trash-alt"></i></a>
                      </td>
                    </tr>
                  </tbody>
                </table>
                <p v-if="filelist.length == 0" style=" padding: 0px; margin: 10px;color: red;display: flex;justify-content: center;">No Record Found</p>
                </div>
              </div>
            </div>
          </div>
        </main>
      </div>
    </div>
  </template>
  <script>
  import moment from 'moment';
  import swal from 'sweetalert2';
  import CommonHeader from '../../../components/CommonHeader.vue';
  import CommonSidebar from '../../../components/CommonSidebar.vue';
  export default {
    components: { CommonSidebar, CommonHeader },
    name: "my-backup",
    
    
    data() {
      return {
       userdetails:null,
       userid: "",
       email: "",
       filterText: "",
       date_from: "",
       date_to: "",
       backupFile: [],
       file:true,
       envURL: "",
       alllist:[],
      };
    },
  
    beforeMount() {
        this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
        this.userid = this.userdetails.user.id;
        this.email = this.userdetails.user.email;
        this.GetFileList();
    },
    mounted() {
    },
    computed:{
        filelist() {
        return this.alllist.filter(item => {
            // Apply your filtering logic here
            return (
            item.file_name.toLowerCase().includes(this.filterText.toLowerCase())
            );
        });
        },
    },
    methods: {
        async GetFileList(){
            const headers = {
                Authorization: "Bearer " + this.userdetails.access_token,
                Accept: "application/json",
                "Content-Type": "application/json",
            };
            const response = await this.$axios.post(
                "files/fileList",
                {
                user_id: this.userid,
                date_from: this.date_from,
                date_to: this.date_to
                },{headers}
            );

            if(response.data.code == 200 || response.data.code == '200'){
                this.alllist = response.data.list;
                this.envURL = response.data.envURL;
            }
        },

        async uploadFile(file){
        try{
            this.loader = true;
            const headers = {
            Authorization: "Bearer " + this.userdetails.access_token,
            Accept: "application/json",
            "Content-Type": "application/json",
            };

            let body = new FormData();

            body.append("user_id", this.userid);
            body.append("file", file.target.files[0]);
            body.append("email", this.email);

            const response = await this.$axios.post(
            "files/fileStore",
            body,{headers}
            );
            
            if(response.data.code == 200 || response.data.code == '200'){
            this.loader = false;
            swal.fire('Record Successfully Saved', '', 'success')
            this.GetFileList();
            this.backupFile=[];
            this.file=true;
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
                                "files/deleteFile",{ 
                                backup_id: data.backup_id,
                                file_path: data.file_path
                                },
                                { headers }
                            );
                            if (response.data.code == 200 || response.data.code == '200') {
                                this.GetFileList();
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

        async onchange(data){
            this.GetFileList();
        },
        
        OnSearch() {
        if (this.search) {
            this.filelist=this.alllist.filter((notChunk) => {
            return (
                notChunk.name
                .toLowerCase()
                .indexOf(this.search.toLowerCase())
            );
            });
        } 
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
  