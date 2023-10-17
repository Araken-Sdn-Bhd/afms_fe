<template>
    <div id="layoutSidenav">
      <CommonSidebar />
      <div id="layoutSidenav_content">
        <CommonHeader />
        <main>
          <div class="container-fluid px-4">
            <div class="page-title">
              <h1>Curiculum Vitae: My CV</h1>
            </div>
          </div>
          <!-- Academic Qualification -->
          <div class="card mb-4">
            <div class="card-header icon-title">
              <a href="#"><i class="fa fa-graduation-cap"></i></a>
                <h4>Academic Qualification(s)</h4>
            </div>
            <div class="table-responsive">
              <div style="overflow-x:auto;">
                  <table class="table table-striped data-table display nowrap" style="width: 100%">
                  <thead>
                    <tr>
                      <th>No</th>
                      <th>Qualification Level</th>
                      <th>Field of Study</th>
                      <th>Specialization</th>
                      <th>Year Awarded</th>
                      <th>Institution Name</th>
                      <th style="width:5%">Action</th>
                    </tr>
                  </thead>
                  <tbody class="optionBox">
                    <tr class="block">
                      <td></td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Qualification Level" v-model="newItem.qualification_level"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Field of Study" v-model="newItem.field_study"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Specialization" v-model="newItem.specialization"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Year Awarded" v-model="newItem.year_awarded"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Institution Name" v-model="newItem.institution_name"/>
                      </td>
                      <td @click="addItem"><a class="add-row"><i class="fa fa-plus"></i></a></td>
                    </tr>
                    <tr class="block" v-for="(item, index) in newList.items" :key="index">

                      <td>{{ index + 1 }}</td>
                      <td>{{ item.qualification_level }}</td>
                      <td>{{ item.field_study }}</td>
                      <td>{{ item.specialization }}</td>
                      <td>{{ item.year_awarded }}</td>
                      <td>{{ item.institution_name }}</td>

                      <td> <span class="remove remove-ui" @click="removeItem(index)"><i class="fal fa-times"></i></span></td>
                    </tr>
                  </tbody>
                </table>
                </div>
            </div>
            <!-- Academic Qualification -->
            <br>
            <br>
            <!-- Professional Body -->
            <div class="card-header icon-title">
              <a href="#"><i class="fa fa-users"></i></a>
                <h4>Professional Body</h4>
            </div>
            <div class="card-body">
              <div style="overflow-x:auto;">
                  <table class="table table-striped data-table display nowrap" style="width: 100%">
                  <thead>
                    <tr>
                      <th>No</th>
                      <th>Name of Professional Body</th>
                      <th>Membership Number</th>
                      <th>Membership Expiry Date</th>
                      <th style="width:5%">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>1</td>
                      <td>
                        <input type="test" class="form-control quals" name="" placeholder="Enter Qualification Level"/>
                      </td>
                      <td>
                        <input class="form-control study" name="" placeholder="Enter Field of Study"/>
                      </td>
                      <td>
                        <input class="form-control special" name="" placeholder="Enter Specialization" />
                      </td>
                      <td>
                        <span class="add-td"><i class="fa fa-plus"></i></span>
                      </td>
                    </tr>
                  </tbody>
                </table>
                </div>
            </div>
            <!-- Professional Body -->
            <br>
            <br>
            <!-- Professional Certificate -->
            <div class="card-header icon-title">
              <a href="#"><i class="fa fa-certificate"></i></a>
                <h4>Professional Certificate(s)</h4>
            </div>
            <div class="card-body">
              <div style="overflow-x:auto;">
                  <table class="table table-striped data-table display nowrap" style="width: 100%">
                  <thead>
                    <tr>
                      <th>No</th>
                      <th>Certificate Name</th>
                      <th>Certificate</th>
                      <th>Date</th>
                      <th style="width:5%">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>1</td>
                      <td>
                        <input type="test" class="form-control quals" name="" placeholder="Enter Qualification Level"/>
                      </td>
                      <td>
                        <input class="form-control study" name="" placeholder="Enter Field of Study"/>
                      </td>
                      <td>
                        <input class="form-control special" name="" placeholder="Enter Specialization" />
                      </td>
                      <td>
                        <button class="add-td"><i class="fa fa-plus"></i></button>
                      </td>
                    </tr>
                  </tbody>
                </table>
                </div>
            </div>
            <!-- Professional Certificate -->
            <br>
            <br>
            <!-- List Of Projects -->
            <div class="card-header icon-title">
              <a href="#"><i class="fa fa-list"></i></a>
                <h4>List Of Projects</h4>
            </div>
            <div class="card-body">
              <div style="overflow-x:auto;">
                  <table class="table table-striped data-table display nowrap" style="width: 100%">
                  <thead>
                    <tr>
                      <th>No</th>
                      <th>Project Title</th>
                      <th>Sector</th>
                      <th>Client Name</th>
                      <th>Year of the Project</th>
                      <th>Position in the Project</th>
                      <th style="width:5%">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>1</td>
                      <td>
                        <input type="test" class="form-control quals" name="" placeholder="Enter Qualification Level"/>
                      </td>
                      <td>
                        <input class="form-control study" name="" placeholder="Enter Field of Study"/>
                      </td>
                      <td>
                        <input class="form-control special" name="" placeholder="Enter Specialization" />
                      </td>
                      <td>
                        <input class="form-control yearawarded" name="" placeholder="Enter Year Awarded" />
                      </td>
                      <td>
                        <input class="form-control institution" name="" placeholder="Enter Institution Name" />
                      </td>
                      <td>
                        <span class="add-td"><i class="fa fa-plus"></i></span>
                      </td>
                    </tr>
                  </tbody>
                </table>
                </div>
            </div>
            <!-- List Of Projects -->
            <br>
            <br>
            <!-- Work Experience -->
            <div class="card-header icon-title">
              <a href="#"><i class="fa fa-briefcase"></i></a>
                <h4>Work Experience</h4>
            </div>
            <div class="card-body">
              <div style="overflow-x:auto;">
                  <table class="table table-striped data-table display nowrap" style="width: 100%">
                  <thead>
                    <tr>
                      <th>No</th>
                      <th>Company Name</th>
                      <th>Position</th>
                      <th>Start Date</th>
                      <th>End Date</th>
                      <th>Year of Experience</th>
                      <th style="width:5%">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>1</td>
                      <td>
                        <input type="test" class="form-control quals" name="" placeholder="Enter Qualification Level"/>
                      </td>
                      <td>
                        <input class="form-control study" name="" placeholder="Enter Field of Study"/>
                      </td>
                      <td>
                        <input class="form-control special" name="" placeholder="Enter Specialization" />
                      </td>
                      <td>
                        <input class="form-control yearawarded" name="" placeholder="Enter Year Awarded" />
                      </td>
                      <td>
                        <input class="form-control institution" name="" placeholder="Enter Institution Name" />
                      </td>
                      <td>
                        <span class="add-td"><i class="fa fa-plus"></i></span>
                      </td>
                    </tr>
                  </tbody>
                </table>
                </div>
            </div>
            <!-- Work Experience -->
            <br>
            <br>
            <div class="form-foter mt-3">
                          <!-- <a href="/app/modules/staff-management/staff-record" class="btn btn-primary btn-text"><i class="fa fa-arrow-alt-to-left"></i> Back</a> -->
                          <div class="ml-auto" id="hidebutton" ref="hidebutton">
                            <button type="submit" class="btn btn-success btn-text"> <i class="fa fa-save"></i> Save</button>
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
    name: "my-cv",


    data() {
      return {

        newList: {items: []},
        newItem: {
                qualification_level:'',
                field_study: '',
                specialization:'',
                year_awarded: '',
                institution_name: '',
                },

      };
    },

    mounted() {

    },
    beforeMount() {


    },
    methods: {

      addItem() {
        this.newList.items.push({
          qualification_level:this.newItem.qualification_level,
          field_study:this.newItem.field_study,
          specialization:this.newItem.specialization,
          year_awarded:this.newItem.year_awarded,
          institution_name:this.newItem.institution_name,
         });


       // Clear the input fields
       this.newItem.qualification_level = '';
       this.newItem.field_study = '';
       this.newItem.specialization = '';
       this.newItem.year_awarded = '';
       this.newItem.institution_name='';
      },

      removeItem(index) {
            
            this.newList.items.splice(index, 1);
      },
    },


  };
  </script>
  <style scoped>


  </style>
