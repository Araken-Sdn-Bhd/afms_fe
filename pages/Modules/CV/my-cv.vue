<template>
    <div id="layoutSidenav">
      <CommonSidebar />
      <div id="layoutSidenav_content">
        <CommonHeader />
        <main>
          <div class="container-fluid px-4">
            <div class="page-title">
              <h1>CV Management: My CV</h1>
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
                <p v-if="acErrors.length">
                            <ul>
                                <li style="color:red" v-for='err in acErrors' :key='err'>{{ err }}</li>
                            </ul>
                </p>
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
                  <tbody class="optionBox">
                    <tr class="block">
                      <td></td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Name of Professional Body" v-model="proBodiesitems.pb_name"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Membership Number" v-model="proBodiesitems.pb_membership"/>
                      </td>
                      <td>
                        <input type="date" class="form-control" placeholder="Enter Membership Expiry Date" v-model="proBodiesitems.pb_expiry"/>
                      </td>
                      <td @click="addPBItem"><a class="add-row"><i class="fa fa-plus"></i></a></td>
                    </tr>
                    <tr class="block" v-for="(item, index) in newpbList.items" :key="index">

                      <td>{{ index + 1 }}</td>
                      <td>{{ item.pb_name }}</td>
                      <td>{{ item.pb_membership }}</td>
                      <td>{{ item.pb_expiry }}</td>

                      <td> <span class="remove remove-ui" @click="removePBItem(index)"><i class="fal fa-times"></i></span></td>
                    </tr>
                  </tbody>
                </table>
                <p v-if="pbErrors.length">
                            <ul>
                                <li style="color:red" v-for='err in pbErrors' :key='err'>{{ err }}</li>
                            </ul>
                </p>
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
                  <tbody class="optionBox">
                    <tr class="block">
                      <td></td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Certificate Name" v-model="pcItem.certificate_name"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter certificate" v-model="pcItem.certificate"/>
                      </td>
                      <td>
                        <input type="date" class="form-control" placeholder="Enter Date" v-model="pcItem.date"/>
                      </td>
                      <td @click="addPCItem"><a class="add-row"><i class="fa fa-plus"></i></a></td>
                    </tr>
                    <tr class="block" v-for="(item, index) in pcList.items" :key="index">

                      <td>{{ index + 1 }}</td>
                      <td>{{ item.certificate_name }}</td>
                      <td>{{ item.certificate }}</td>
                      <td>{{ item.date }}</td>

                      <td> <span class="remove remove-ui" @click="removePCItem(index)"><i class="fal fa-times"></i></span></td>
                    </tr>
                  </tbody>
                </table>
                <p v-if="pcErrors.length">
                            <ul>
                                <li style="color:red" v-for='err in pcErrors' :key='err'>{{ err }}</li>
                            </ul>
                </p>
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
                  <tbody class="optionBox">
                    <tr class="block">
                      <td></td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Project Title" v-model="lpItem.project_title"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Sector" v-model="lpItem.sector"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Client Name" v-model="lpItem.client_name"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Year of the Project" v-model="lpItem.year_project"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Position in the Project" v-model="lpItem.position_project"/>
                      </td>
                      <td @click="addLPItem"><a class="add-row"><i class="fa fa-plus"></i></a></td>
                    </tr>
                    <tr class="block" v-for="(item, index) in lplist.items" :key="index">

                      <td>{{ index + 1 }}</td>
                      <td>{{ item.project_title }}</td>
                      <td>{{ item.sector }}</td>
                      <td>{{ item.client_name }}</td>
                      <td>{{ item.year_project }}</td>
                      <td>{{ item.position_project }}</td>

                      <td> <span class="remove remove-ui" @click="removeLPItem(index)"><i class="fal fa-times"></i></span></td>
                    </tr>
                  </tbody>
                </table>
                <p v-if="lpErrors.length">
                            <ul>
                                <li style="color:red" v-for='err in lpErrors' :key='err'>{{ err }}</li>
                            </ul>
                </p>
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
                  <tbody class="optionBox">
                    <tr class="block">
                      <td></td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Company Name" v-model="weItem.company_name"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Position" v-model="weItem.position"/>
                      </td>
                      <td>
                        <input type="date" class="form-control" placeholder="Enter Start Date" v-model="weItem.start_date"/>
                      </td>
                      <td>
                        <input type="date" class="form-control" placeholder="Enter End Date" v-model="weItem.end_date"/>
                      </td>
                      <td>
                        <input type="text" class="form-control" placeholder="Enter Year of Experience" v-model="weItem.year_experience"/>
                      </td>
                      <td @click="addWEItem"><a class="add-row"><i class="fa fa-plus"></i></a></td>
                    </tr>
                    <tr class="block" v-for="(item, index) in welist.items" :key="index">

                      <td>{{ index + 1 }}</td>
                      <td>{{ item.company_name }}</td>
                      <td>{{ item.position }}</td>
                      <td>{{ item.start_date }}</td>
                      <td>{{ item.end_date }}</td>
                      <td>{{ item.year_experience }}</td>

                      <td> <span class="remove remove-ui" @click="removeWEItem(index)"><i class="fal fa-times"></i></span></td>
                    </tr>
                  </tbody>
                </table>
                <p v-if="weErrors.length">
                            <ul>
                                <li style="color:red" v-for='err in weErrors' :key='err'>{{ err }}</li>
                            </ul>
                </p>
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

        acErrors: [],
        pbErrors: [],
        pcErrors: [],
        lpErrors: [],
        weErrors: [],
        newList: {items: []},
        newItem: {
                qualification_level:'',
                field_study: '',
                specialization:'',
                year_awarded: '',
                institution_name: '',
                },
        
        newpbList: {items: []},
        proBodiesitems: {
                          pb_name:'',
                          pb_membership: '',
                          pb_expiry: '',
                        },
        pcList:{items: []},
        pcItem: {
                certificate_name:'',
                certificate:'',
                date:'',
                },
        
        lplist:{items: []},
        lpItem: {
                project_title:'',
                sector:'',
                client_name:'',
                year_project:'',
                position_project:'',
                },
        welist:{items: []},
        weItem: {
                company_name:'',
                position:'',
                start_date:'',
                end_date:'',
                year_experience:'',
                },
        
      };
    },

    mounted() {

    },
    beforeMount() {


    },
    methods: {

      addItem() {
        this.acErrors = [];
        if(!this.newItem.qualification_level){
          this.acErrors.push("Qualification Level is required.");
        }
        if(!this.newItem.field_study){
          this.acErrors.push("Field Study is required.");
        }
        if(!this.newItem.specialization){
          this.acErrors.push("Specialization is required.");
        }
        if(!this.newItem.year_awarded){
          this.acErrors.push("Year Awarded is required.");
        }
        if(!this.newItem.institution_name){
          this.acErrors.push("Institution Name is required");
        }
        if(this.newItem.qualification_level && this.newItem.field_study && this.newItem.specialization && this.newItem.year_awarded && this.newItem.institution_name){
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
        }
      },

      addPBItem(){
        this.pbErrors = [];
        if(!this.proBodiesitems.pb_name){
          this.pbErrors.push("Name of Professional Body is required.");
        }
        if(!this.proBodiesitems.pb_membership){
          this.pbErrors.push("Membership Number is required.");
        }
        if(!this.proBodiesitems.pb_expiry){
          this.pbErrors.push("Membership Expiry Date is required.");
        }
        if(this.proBodiesitems.pb_name && this.proBodiesitems.pb_membership && this.proBodiesitems.pb_expiry){
          this.newpbList.items.push({
            pb_name:this.proBodiesitems.pb_name,
            pb_membership:this.proBodiesitems.pb_membership,
            pb_expiry:this.proBodiesitems.pb_expiry,
          });

          this.proBodiesitems.pb_name = '';
          this.proBodiesitems.pb_membership = '';
          this.proBodiesitems.pb_expiry = '';
        }
      },

      addPCItem(){
        this.pcErrors = [];
        if(!this.pcItem.certificate_name){
          this.pcErrors.push("Certificate Name is required.");
        }
        if(!this.pcItem.certificate){
          this.pcErrors.push("Certificate is required.");
        }
        if(!this.pcItem.date){
          this.pcErrors.push("Date is required.");
        }
        if(this.pcItem.certificate_name && this.pcItem.certificate && this.pcItem.date){
            this.pcList.items.push({
              certificate_name:this.pcItem.certificate_name,
              certificate:this.pcItem.certificate,
              date:this.pcItem.date,
            });

            this.pcItem.certificate_name = '';
            this.pcItem.certificate = '';
            this.pcItem.date = '';
        }
      },

      addLPItem(){
        this.lpErrors = [];
        if(!this.lpItem.project_title){
          this.lpErrors.push("Project Title is required.");
        }
        if(!this.lpItem.sector){
          this.lpErrors.push("Sector is required.");
        }
        if(!this.lpItem.client_name){
          this.lpErrors.push("Client Name is required.");
        }
        if(!this.lpItem.year_project){
          this.lpErrors.push("Year of the Project is required.");
        }
        if(!this.lpItem.position_project){
          this.lpErrors.push("Position in the Project is required.");
        }
        if(this.lpItem.project_title && this.lpItem.sector && this.lpItem.client_name && this.lpItem.year_project && this.lpItem.position_project){
          this.lplist.items.push({
            project_title:this.lpItem.project_title,
            sector:this.lpItem.sector,
            client_name:this.lpItem.client_name,
            year_project:this.lpItem.year_project,
            position_project:this.lpItem.position_project,
          });

          this.lpItem.project_title = '';
          this.lpItem.sector = '';
          this.lpItem.client_name = '';
          this.lpItem.year_project = '';
          this.lpItem.position_project = '';
        }
      },

      addWEItem(){
        this.weErrors = [];
        if(!this.weItem.company_name){
          this.weErrors.push("Company Name is required.");
        }
        if(!this.weItem.position){
          this.weErrors.push("Position is required.");
        }
        if(!this.weItem.start_date){
          this.weErrors.push("Start Date is required.");
        }
        if(!this.weItem.end_date){
          this.weErrors.push("End Date is required.");
        }
        if(!this.weItem.year_experience){
          this.weErrors.push("Year of Experience is required.");
        }
        if(this.weItem.company_name && this.weItem.position && this.weItem.start_date && this.weItem.end_date && weItem.year_experience){
          this.welist.items.push({
            company_name: this.weItem.company_name,
            position: this.weItem.position,
            start_date: this.weItem.start_date,
            end_date: this.weItem.end_date,
            year_experience: this.weItem.year_experience,
          });

          this.weItem.company_name = '';
          this.weItem.position = '';
          this.weItem.start_date = '';
          this.weItem.end_date = '';
          this.weItem.year_experience = '';
        }
      },

      removeItem(index) {
            
            this.newList.items.splice(index, 1);
      },

      removePBItem(index) {
            
            this.newpbList.items.splice(index, 1);
      },

      removePCItem(index){
            this.pcList.items.splice(index, 1);
      },

      removeLPItem(index){
            this.lplist.items.splice(index, 1);
      },

      removeWEItem(index){
            this.welist.items.splice(index, 1);
      },
    },


  };
  </script>
  <style scoped>


  </style>
