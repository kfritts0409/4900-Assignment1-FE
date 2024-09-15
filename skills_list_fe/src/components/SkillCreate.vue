<template>
    <div class="container-fluid">
        <div class="row align-items-center justify-content-center">
            <div class=" col align-items-center">
                <div class="row align-items-center justify-content-center">
                    <div class="col col-12 col-sm-10 col-md-10 col-lg-6">
                        <div class="alert alert-danger shadow" role="alert" v-if="showMsg === 'error'">
                            Please verify Skill Information
                        </div>
                        <div class="alert alert-danger shadow" role="alert" v-if="showMsg === 'requestError'">
                            Please verify Skill Information - data formatted
                            incorrectly
                        </div>
                    </div>
                </div>
                <div class="row align-items-center justify-content-center">
                    <div class="col col-12 col-sm-10 col-md-10 col-lg-6 align-
    items-center">
                        <div class="card">
                            <div class="card-header">{{ pageTitle }}</div>
                            <div class="card-body">
                                <form ref="form">
                                    <div class="container-fluid">
                                        <div class="form-group row justify-content-
    around py-2">
                                            <label class="col-5">Skill</label>
                                            <div class="col col-7">
                                                <input v-model="skill.skill" type="text"
                                                    class="form-control-sm form-control">
                                            </div>
                                        </div>
                                        <div class="form-group row justify-content-
around py-2">
                                            <label class="col-5">Description</label>
                                            <div class="col col-7">
                                                <input v-model="skill.description" type="text"
                                                    class="form-control-sm form-control">
                                            </div>
                                        </div>
                                        <div class="row justify-content-around">
                                            <div v-if="!isUpdate" type="button" class="btn
btn-primary col-4" @click="createSkill">Save</div>
                                            <div v-if="isUpdate" type="button" class="btn
btn-primary col-4" @click="updateSkill">Update</div>
                                            <div type="button" class="btn btn-secondary
col-4" @click="cancelOperation">Cancel</div>
                                        </div>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import router from '../router';
import { APIService } from '../http/APIService';
const apiService = new APIService();
export default {
    name: 'SkillCreate',
    data() {
        return {
            showError: false,
            skill: {},
            pageTitle: "Add New Skill",
            isUpdate: false,
            showMsg: '',
        };
    },
    methods: {
        createSkill() {
            apiService.addNewSkill(this.skill).then(response => {
                if (response.status === 201) {
                    this.skill = response.data;
                    this.showMsg = "";
                    router.push('/skill-list/new');
                } else {
                    this.showMsg = "error";
                }
            }).catch(error => {
                this.showMsg = "error";
            });
        },
        cancelOperation() {
            router.push("/skill-list");
        },
        updateSkill() {
            apiService.updateSkill(this.skill).then(response => {
                if (response.status === 200) {
                    this.skill = response.data;
                    router.push('/skill-list/update');
                } else {
                    this.showMsg = "error";
                }
            }).catch(error => {
                this.showMsg = "error";
            });
        }
    },
    mounted() {
        // if a primary key is provided, set title and get the skill
        if (this.$route.params.pk) {
            this.pageTitle = "Edit Skill";
            this.isUpdate = true;
            apiService.getSkill(this.$route.params.pk).then(response => {
                this.skill = response.data;
            }).catch(error => {
                this.showMsg = "error";
            });
        }
    }
};
</script>
<style></style>
