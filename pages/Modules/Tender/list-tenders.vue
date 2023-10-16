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
                <a href="/app/modules/Tender/new-tender" class="add-btn" title="Add Tender"><em class="fa fa-plus"></em></a>
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
                    <div class="col-lg-3 col-sm-6 mb-3">
                        <label for="date_from">FROM:</label>
                        <input type="date" class="form-control" v-model="date_from" :max="this.date_to" @change="onchange($event)" id="date_from"/>
                    </div>
                    <div class="col-lg-3 col-sm-6 mb-3">
                        <label for="date_to">TO:</label>
                        <input type="date" class="form-control" v-model="date_to" :min="this.date_from" @change="onchange($event)" id="date_to"/>
                    </div>
  
                    <div class="col-lg-6 col-sm-6 mb-3">
                        <label for="search">SEARCH:</label>
                      <div class="input-group search-table">
                        <span class="input-group-text bg-transparent br-0"><i class="fa fa-search"></i></span>
                        <input type="text" class="form-control" v-model="filterText" id="search" placeholder="Filter by Title, Reference No., or Submission Price"/>
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
                    <tr v-for="(tender,index) in tenderlist" :key="index">
                      <td>{{ index+1}}</td>
                      <td>{{ getFormattedDate(tender.submission_date)}}</td>
                      <td>{{ tender.title }}</td>
                      <td>{{ tender.reference_no }}</td>
                      <td>{{ tender.submission_price }}</td>
                      <td>
                        <a class="view" @click="Onview(tender)" title="View tender details"><em class="fa fa-eye"></em></a>
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
  import moment from 'moment';
  import swal from 'sweetalert2';
  import CommonHeader from '../../../components/CommonHeader.vue';
  import CommonSidebar from '../../../components/CommonSidebar.vue';
  export default {
    components: { CommonSidebar, CommonHeader },
    name: "list-tenders",


    data() {
      return {
        alllist: [],
        userid: [],
        email: [],
        filterText: "",
        date_from: "",
        date_to: "",
        
      };
    },
    computed:{
      tenderlist() {
        return this.alllist.filter(item => {
            // Apply your filtering logic here
            return (
            item.title.toLowerCase().includes(this.filterText.toLowerCase())||
            item.reference_no.toLowerCase().includes(this.filterText) ||
            item.submission_price.toString().includes(this.filterText)
            );
        });
      },
    },

    beforeMount() {
      this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
      this.GetTenderList();
    },
    mounted() {

    },
    methods: {
      async Onview(data) {
        this.$router.push({
                    path: "/modules/Tender/view-tender",
                    query: {
                        id: data.tender_id,
                    },
                });
      },

      async GetTenderList(){
            const headers = {
                Authorization: "Bearer " + this.userdetails.access_token,
                Accept: "application/json",
                "Content-Type": "application/json",
            };
            const response = await this.$axios.post(
                "tender/tenderList",
                {
                date_from: this.date_from,
                date_to: this.date_to
                },{headers}
            );

            if(response.data.code == 200 || response.data.code == '200'){
                this.alllist = response.data.list;
            }
      },
      
      getFormattedDate(date) {
          return moment(date).format("DD/MM/YYYY")
      },

      async onchange(data){
            this.GetTenderList();
      },
    },
  };
  </script>
  <style scoped>


  </style>
