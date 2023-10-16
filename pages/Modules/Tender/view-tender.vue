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
                                <input disabled type="text" class="form-control" placeholder="Enter Title" v-model="title" />
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-12 mb-4">
                                <label for="" class="form-label">Client<span style="color:red">*</span></label>
                                <select disabled v-model="clientID" class="form-select" aria-label="Default select example">
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
                                <input disabled type="date" class="form-control" v-model="sub_date" />
                            </div>
                            <div class="col-md-6 mb-4">
                                <label for="" class="form-label">Submission Price<span style="color:red">*</span></label>
                                <input disabled type="text" class="form-control" placeholder="RM0.00" v-model="sub_price" />
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-6 mb-4">
                                <label for="" class="form-label">Reference No.<span style="color:red">*</span></label>
                                <input disabled type="text" class="form-control" placeholder="Enter Reference No." v-model="reference_no" />
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-12 mb-8">
                                <label for="" class="form-label">Remarks<span style="color:red">*</span></label>
                                <textarea disabled class="form-control" placeholder="Enter Description" v-model="remarks"></textarea>
                            </div>
                        </div>
                        <br>
                        <div class="row">
                            <div class="col-md-12 mb-8">
                                <label for="" class="form-label">Tender Requirement<span style="color:red">*</span></label>
                                <input disabled class="form-control" id="file-input" for="file-input" type="file" v-on:change="selectFile">
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
                                        <input disabled class="form-control" id="file-input" for="file-input" type="file" v-on:change="selectFile">
                                    </div>
                                </div>
                            </div>
                            <div class="row mb-3">
                                <div>
                                    <div class="col-md-6">
                                        <label for="" class="form-label">Financial<span style="color:red">*</span></label>
                                        <input disabled class="form-control" id="file-input" for="file-input" type="file" v-on:change="selectFile">
                                    </div>
                                </div>
                            </div>
                            <div class="row mb-3">
                                <div>
                                    <div class="col-md-6">
                                        <label for="" class="form-label">Other<span style="color:red">*</span></label>
                                        <input disabled class="form-control" id="file-input" for="file-input" type="file" v-on:change="selectFile">
                                    </div>
                                </div>
                            </div>
                        </div>

                        <p v-if="errors.length">
                            <ul>
                                <li style="color:red" v-for='err in errors' :key='err'>{{ err }}</li>
                            </ul>
                        </p>

                        <br>
                        <div class="d-flex">
                            <a href="modules/Tender/list-tenders" class="btn btn-primary btn-text"><i class="fa fa-arrow-alt-to-left"></i>Previous</a>
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
    name: "view-tender",

    data() {
        return {
            clientlist: [],
            Id: 0,
            errors: [],
            user_id: "",
            title: "",
            clientID: "",
            sub_date: "",
            sub_price: "",
            reference_no: "",
            remarks: "",
            tender_req: "",
            technical_doc: "",
        };
    },

    mounted() {

    },
    beforeMount() {
        this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
        let urlParams = new URLSearchParams(window.location.search);
        this.Id = urlParams.get("id");
        this.user_id = this.userdetails.user.id;

        this.GetTenderbyId();
        this.GetClient();

    },
    methods: {
        async GetTenderbyId() {
            const headers = {
                Authorization: "Bearer " + this.userdetails.access_token,
                Accept: "application/json",
                "Content-Type": "application/json",
            };
            const response = await this.$axios.post(
                "tender/getTender", {
                    tender_id: this.Id
                }, {
                    headers
                }
            );

            if (response.data.code == 200) {

                this.Id = response.data.tender[0].tender_id;
                this.user_id = response.data.tender[0].user_id;
                this.clientID = response.data.tender[0].client_id;
                this.title = response.data.tender[0].title;
                this.sub_date = response.data.tender[0].submission_date;
                this.sub_price = response.data.tender[0].submission_price;
                this.reference_no = response.data.tender[0].reference_no;
                this.remarks = response.data.tender[0].remark;
                // alert('sini ke')

            }
        },
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
        }
    }
};
</script>


<style scoped>

    </style>
