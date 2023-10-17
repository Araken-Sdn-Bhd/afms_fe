<template>
<div id="layoutSidenav">
    <CommonSidebar />
    <div id="layoutSidenav_content">
        <CommonHeader />
        <main>
            <div class="container-fluid px-4">
                <div class="page-title">
                    <h1>
                        Add New Tender
                    </h1>
                </div>
                <div class="card mb-4">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-md-12 mb-4">
                                <label for="" class="form-label">Title<span style="color:red">*</span></label>
                                <input type="text" class="form-control" placeholder="Enter Title" v-model="titles" />
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-12 mb-4">
                                <label for="" class="form-label">Client<span style="color:red">*</span></label>
                                <select v-model="clientID" class="form-select" aria-label="Default select example">
                                    <option value="">Please Select</option>
                                    <option v-for="client in clientlist" v-bind:key="client.client_id" v-bind:value="client.client_id">
                                        {{ client.client_name }}
                                    </option>
                                </select>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-6 mb-4">
                                <label for="" class="form-label">Submission Date<span style="color:red">*</span></label>
                                <input type="date" class="form-control" v-model="sub_date" />
                            </div>
                            <div class="col-md-6 mb-4">
                                <label for="" class="form-label">Submission Price (RM)<span style="color:red">*</span></label>
                                <input type="text" class="form-control" placeholder="RM0.00" v-on:keypress="NumbersOnly" v-model="sub_price"  />
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-6 mb-4">
                                <label for="" class="form-label">Reference No.<span style="color:red">*</span></label>
                                <input type="text" class="form-control" placeholder="Enter Reference No." v-model="reference_no" />
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-12 mb-8">
                                <label for="" class="form-label">Remarks<span style="color:red">*</span></label>
                                <textarea class="form-control" placeholder="Enter Description" v-model="remarks"></textarea>
                            </div>
                        </div>
                        <br>
                        <div class="row">
                            <div class="col-md-12 mb-8">
                                <label for="" class="form-label">Tender Requirement<span style="color:red">*</span></label>
                                <input class="form-control" id="file-input" for="file-input" type="file" accept=".zip" v-on:change="selectFile">
                            </div>
                        </div>
                        <br>
                        <div class="row">
                            <div class="col-md-12 mb-6">
                                <label for="" class="form-label">Submission</label>
                            </div>
                            <div class="row mb-3">
                                <div>
                                    <div class="col-md-6">
                                        <label for="" class="form-label">Technical<span style="color:red">*</span></label>
                                        <input class="form-control" id="file-input" for="file-input" type="file" accept=".zip" v-on:change="selectFile">
                                    </div>
                                </div>
                            </div>
                            <div class="row mb-3">
                                <div>
                                    <div class="col-md-6">
                                        <label for="" class="form-label">Financial<span style="color:red">*</span></label>
                                        <input class="form-control" id="file-input" for="file-input" type="file" accept=".zip" v-on:change="selectFile">
                                    </div>
                                </div>
                            </div>
                            <div class="row mb-3">
                                <div>
                                    <div class="col-md-6">
                                        <label for="" class="form-label">Other<span style="color:red">*</span></label>
                                        <input class="form-control" id="file-input" for="file-input" type="file" accept=".zip" v-on:change="selectFile">
                                    </div>
                                </div>
                            </div>
                        </div>

                        <p v-if="errors">
                            <ul>
                                <li style="color:red" v-for='err in errors' :key='err'>{{ err }}</li>
                            </ul>
                        </p>

                        <br>
                        <div class="d-flex">
                            <button @click="GoBack" class="btn btn-primary btn-text"><i class="fa fa-arrow-alt-to-left"></i> Previous
                            </button>
                            <div class="btn-right">
                                <button type="submit" @click="onSubmit()" class="btn btn-success btn-text">
                                    <i class="fas fa-save"></i> Save Record
                                </button>
                            </div>
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
    components: {
        CommonSidebar,
        CommonHeader
    },
    name: "new-tender",

    data() {
        return {
            clientlist: [],
            errors: [],
            user_id: "",
            titles: "",
            clientID: "",
            sub_date: "",
            sub_price: "",
            reference_no: "",
            remarks: "",
            tender_requirement: "",
            technical_doc: "",
            financial_doc: "",
            other_doc: "",
            tr_file: ['tender_requirement', 'technical_doc_loc', 'financial_doc_loc', 'other_doc_loc'],
        };
    },

    mounted() {

    },
    beforeMount() {
        this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
        this.user_id = this.userdetails.user.id;
        this.GetClient();

    },
    methods: {
        async GetClient() {
            const headers = {
                Authorization: "Bearer " + this.userdetails.access_token,
                Accept: "application/json",
                "Content-Type": "application/json",
            };
            const response = await this.$axios.get("clients/getClient", {
                headers
            });
            this.clientlist = response.data.list;
        },

        async selectFile(event) {
            const files = event.target.files[0];
            if (files) {
                if (!this.isZip(files)) {
                    this.errors.push("Invalid file format. Please choose ZIP format only")
                    event.target.value = ''; // Clears the selected file
                } else {
                    this.tr_file = event.target.files[0];
                    this.errors = null; // Clear any previous error message
                }
            }
        },
        isZip(file) {
            // Check if the file type is a ZIP archive
            return file.name.endsWith('.zip');
        },

        async onSubmit() {
            this.$swal.fire({
                title: 'Are you sure to save this record?',
                showCancelButton: true,
                confirmButtonText: 'Yes',
                cancelButtonText: 'Cancel',
                reverseButtons: true
            }).then(async (result) => {
                if (result.isConfirmed) {
                    this.errors = [];
                    if (!this.titles) {
                        this.errors.push("Title is required.");
                    }
                    if (!this.clientID) {
                        this.errors.push("Client is required.");
                    }
                    if (!this.sub_date) {
                        this.errors.push("Submission Date is required.");
                    }
                    if (!this.sub_price) {
                        this.errors.push("Submission Price is required.");
                    }
                    if (!this.reference_no) {
                        this.errors.push("Reference No. is required.");
                    }
                    if (!this.remarks) {
                        this.errors.push("Remarks is required.");
                    }
                    // if (!this.tender_requirement) {
                    //     this.errors.push("Tender Requirement for Submission is required.");
                    // }
                    // if (!this.technical_doc) {
                    //     this.errors.push("Technical Document for Submission is required.");
                    // }
                    // if (!this.financial_doc) {
                    //     this.errors.push("Financial Document for Submission is required.");
                    // }
                    // if (!this.other_doc) {
                    //     this.errors.push("Other Document for Submission is required.");
                    // }

                    if ((this.titles && this.clientID && this.sub_date && this.sub_price && this.reference_no && this.remarks)) {

                        this.loader = true;
                        const headers = {
                            Authorization: "Bearer " + this.userdetails.access_token,
                            Accept: "application/json",
                            "Content-Type": "application/json",
                        };

                        let body = new FormData();
                        body.append("user_id", this.user_id);
                        body.append("title", this.titles);
                        body.append("client_id", this.clientID);
                        body.append("submission_date", this.sub_date);
                        body.append("submission_price", this.sub_price);
                        body.append("reference_no", this.reference_no);
                        body.append("remark", this.remarks);
                        body.append("tender_requirement", this.tr_file);
                        body.append("financial_doc_loc", this.tr_file);
                        body.append("technical_doc_loc", this.tr_file);
                        body.append("other_doc_loc", this.tr_file);

                        const response = await this.$axios.post("tender/newTender", body, {
                            headers
                        });

                        if (response.data.code == 200 || response.data.code == "200") {
                            this.loader = false;
                            await this.$swal.fire('Successfully Created', '', 'success');
                            this.$router.push("/Modules/Tender/list-tenders");

                        } else {
                            this.loader = false;
                            this.$swal.fire({
                                icon: 'error',
                                title: 'Oops... Something Went Wrong!',
                                text: 'the error is: ' + JSON.stringify(response.data.message)
                            });
                        }
                    }
                }
            })
        },

        NumbersOnly(evt) {
      evt = (evt) ? evt : window.event;
      var charCode = (evt.which) ? evt.which : evt.keyCode;
      if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
        evt.preventDefault();;
      } else {
        return true;
      }
    },

        async GoBack() {
            this.$router.push({
                path: "/modules/Tender/list-tenders",
                query: {
                    id: this.Id,
                },
            });
        }
    }
};
</script>

<style scoped>

  </style>
