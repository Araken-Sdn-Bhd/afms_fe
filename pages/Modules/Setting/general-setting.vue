
<template>
  <div id="layoutSidenav">
    <CommonSidebar />
    <div id="layoutSidenav_content">
      <CommonHeader />
      <main>
        <div class="container-fluid px-4">
          <div class="page-title">
            <h1>General Setting</h1>
          </div>
          <div class="card mb-4">
            <div class="card-body">
              <div class="row mb-4 align-items-center">
                <div class="col-md-6">
                  <label for="" class="form-label">Type</label>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Enter Type"
                    v-model="settingType"
                    list="settingTypeList"
                  />
                  <datalist id="settingTypeList">
                      <option
                        v-for="typ in typelist"
                        v-bind:key="typ.type"
                        v-bind:value="typ.type"
                      >
                        {{ typ.type }}
                    </option>
                  </datalist>
                </div>

                <div class="col-md-6">
                  <label for="" class="form-label">Parameter</label>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Enter Parameter"
                    v-model="settingParam"
                  />
                </div>

                <div class="col-md-2">
                  <label for="" class="form-label">Value</label>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Enter Value"
                    v-model="settingVal"
                  />
                </div>

                <div class="col-md-2">
                  <label for="" class="form-label">Code</label>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Enter Code"
                    v-model="settingCode"
                  />
                </div>

                <div class="col-md-2">
                  <label for="" class="form-label">Index</label>
                  <input
                    type="number"
                    min=0
                    class="form-control"
                    placeholder="Enter Index"
                    v-model="settingIndex"
                  />
                </div>

                <div class="col-md-6">
                  <label for="" class="form-label">Description</label>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Enter Description"
                    v-model="settingDesc"
                  />
                </div>
                <!-- <div class="col-md-6">
                  <label for="" class="form-label">Parameter</label>
                  <select 
                    v-model="clientStatus"
                    class="form-select"
                    aria-label="Default select example"
                  >
                    <option value="1">Active</option>
                    <option value="0">Inactive</option>
                  </select>
                </div> -->
            </div>
            <div class="d-flex justify-content-center">
                  <button type="submit" class="btn btn-success btn-text" v-if="editID" @click="addRecord">
                    <i class="fa fa-save"></i> Update
                  </button>
                  <button type="submit" class="btn btn-success btn-text" v-if="!editID" @click="addRecord">
                    <i class="fa fa-plus"></i> Add New
                  </button>
                </div>
            </div>
          </div>

          <div class="card mb-4">
              <div class="card-header icon-title">
                          <a href="#"><i class="fa fa-list"></i></a>
                          <h4>List of Settings</h4>
              </div>
            <div class="card-body">
              <!-- search-table -->
                <div class="col-md-4 mb-3">
                  <select 
                    v-model="typeSearch"
                    class="form-select"
                    aria-label="Default select example"
                    @change="onChange($event)"
                  >
                    <option value="ALL">All Types</option>
                      <option
                        v-for="typ in typelist"
                        v-bind:key="typ.type"
                        v-bind:value="typ.type"
                      >
                        {{ typ.type }}
                    </option>
                  </select>
                </div>
              <div style="overflow-x:auto;">
                <table class="table table-striped data-table display nowrap" style="width: 100%">
                <thead>
                  <tr>
                    <th>No</th>
                    <th>Type</th>
                    <th>Parameter</th>
                    <th>Value</th>
                    <th>Code</th>
                    <th>Index</th>
                    <th>Description</th>
                    <th style="width:5%">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(setting,index) in generallist" :key="index">
                    <td>{{ index+1}}</td>
                    <td>{{ setting.type }}</td>
                    <td>{{ setting.parameter }}</td>  
                    <td>{{ setting.value }}</td>
                    <td>{{ setting.code }}</td>
                    <td>{{ setting.index }}</td>
                    <td>{{ setting.description }}</td>
                    <td>
                      <a @click="editRecord(setting)" class="action-icon icon-success" title="Edit Record"><i class="fa fa-edit"></i></a>
                      <a @click="deleteRecord(setting)" title="Delete Record" class="action-icon icon-danger"><i class="fa fa-trash-alt"></i></a>
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
  name: "general-setting",
  
  
  data() {
    return {
      alllist: [],
      generallist: [],
      typelist: [],
      userdetails:null,
      userID: 0,
      editID: 0,
      settingType: "",
      settingParam: "",
      settingVal: "",
      settingCode: "",
      settingIndex: "",
      settingDesc: "",
      typeSearch: "",



     
    };
  },
  beforeMount() {
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.GetGeneralSettingList();
    this.userID = this.userdetails.user.id;
    this.getTypeList();
  },

  mounted() {
  },

  methods: {
    async GetGeneralSettingList(){
      const headers = {
        Authorization: "Bearer " + this.userdetails.access_token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "general-setting/lists",
        {headers}
      );

      if(response.data.code == 200){
        this.generallist = response.data.list;
      }
    },

    async addRecord(){
      try{
          this.loader = true;
          const headers = {
          Authorization: "Bearer " + this.userdetails.access_token,
          Accept: "application/json",
          "Content-Type": "application/json",
          };

          let body = new FormData();

          body.append("editId", this.editID);
          body.append("added_by", this.userID);
          body.append("type", this.settingType);
          body.append("parameter", this.settingParam);
          body.append("value", this.settingVal);
          body.append("code", this.settingCode);
          body.append("index", this.settingIndex);
          body.append("description", this.settingDesc);


          const response = await this.$axios.post(
          "general-setting/add",
          body,{headers}
          );
          
          if(response.data.code == 200 || response.data.code == '200'){
          this.loader = false;
          swal.fire('Record Successfully Saved', '', 'success')
          this.resetModel();
          this.GetGeneralSettingList();
          this.getTypeList();
          this.typeSearch="ALL";
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
                              "general-setting/deleteSetting",{ 
                              id: data.id,
                              },
                              { headers }
                          );
                          if (response.data.code == 200 || response.data.code == '200') {
                              this.GetGeneralSettingList();
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
      this.editID='',
      this.settingType='',
      this.settingParam='',
      this.settingVal='',
      this.settingCode='',
      this.settingIndex='',
      this.settingDesc=''
    },

    async editRecord(data){
      this.editID=data.id;
      this.settingType=data.type;
      this.settingParam=data.parameter;
      this.settingVal=data.value;
      this.settingCode=data.code;
      this.settingIndex=data.index;
      this.settingDesc=data.description;
    },

    async getTypeList(){
      const headers = {
        Authorization: "Bearer " + this.userdetails.access_token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "general-setting/typeList",
        {headers}
      );

      if(response.data.code == 200){
        this.typelist = response.data.list;
      }
    },

    async onChange(event) {
      if(event.target.value!='ALL'){
      const headers = {
        Authorization: "Bearer " + this.userdetails.access_token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.post(
        "general-setting/typeSearchList",
        {type: event.target.value},
        {headers}
      );

      if(response.data.code == 200){
        this.generallist = response.data.list;
      }
    }else{
      this.GetGeneralSettingList();
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
