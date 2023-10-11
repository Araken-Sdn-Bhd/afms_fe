<template>
    <div id="layoutSidenav">
      <CommonSidebar />
      <div id="layoutSidenav_content">
        <CommonHeader />
        <main>
            <div class="container-fluid px-4">
            <div class="page-title">
              <h1>Tender Management</h1>
              <div class="btn-group-a">
                <a @click="addstaff()" class="add-btn"><em class="fa fa-plus"></em></a>
              </div>
            </div>
  
            <div class="card mb-4">
                <div class="card-header icon-title">
                            <a href="#"><i class="fa fa-list"></i></a>
                            <h4>List of Tenders</h4>
                        </div>
              <div class="card-body">
                <div class="search-table">
                  <div class="row mt-2">
  
                    <div class="col-lg-4 col-sm-6 mb-3">
                      <div class="input-group">
                        <span class="input-group-text bg-transparent br-0"><i class="fa fa-search"></i></span>
                        <input type="text" class="form-control" v-model="filterText" placeholder="Filter by name or nric number"/>
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
                      <th>Submission Date</th>
                      <th>Title</th>
                      <th>Reference No.</th>
                      <th>Submission Price (RM)</th>
                      <th style="width:5%">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(staff,index) in stafflist" :key="index">
                      <td>#{{ index+1}}</td>
                      <td>{{ staff.name }}</td>
                      <td>{{ staff.nric_no }}</td>
                      <td>
                        <a @click="edit(staff)" class="view" title="edit staff profile"><em class="fa fa-edit"></em></a>
                        <!--<a class="view" @click="Onview(staff)" title="view user matrix"><em class="fa fa-bars"></em></a>-->
                      </td>
                    </tr>
                  </tbody>
                </table>
                </div>
  
              </div>
            </div>
          </div>
        </main>
      </div>
    </div>
  </template>
  <script>
  import CommonHeader from '../../../components/CommonHeader.vue';
  import CommonSidebar from '../../../components/CommonSidebar.vue';
  export default {
    components: { CommonSidebar, CommonHeader },
    name: "list-tenders",
    
    
    data() {
      return {
       
      };
    },
  
    mounted() {
     
    },
    beforeMount() {
     
  
    },
    methods: {
    
    }
  };
  </script>
  <style scoped>
  
  
  </style>
  