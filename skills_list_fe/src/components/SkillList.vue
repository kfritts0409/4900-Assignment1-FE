<template>
    <div class="container-fluid">
        <div class="row align-items-center justify-content-center">
            <div class="col-12 col-md-10 col-lg-10 col-12 align-items-center
    justify-content-center">
                <div class="alert alert-success" v-if="showMsg === 'new'" :value="true">
                    New skill added.
                </div>
                <div class="alert alert-success" v-if="showMsg === 'update'" :value="true">
                    Skill updated.
                </div>
                <div class="alert alert-success" v-if="showMsg === 'deleted'" :value="true">
                    Skill deleted.
                </div>
                <div class="alert alert-danger" v-if="showMsg === 'error'" :value="true">
                    Error connecting to server. Check server.
                </div>
            </div>
        </div>
        <div class="row align-items-center justify-content-center">
            <div class="col col-12 col-md-10 d-none d-xl-block d-lg-block d-md-
block">
                <table class="table table-hover" style="overflow-y: auto">
                    <thead>
                        <tr>
                            <th scope="col">ID</th>
                            <th scope="col">Skill</th>
                            <th scope="col">Description</th>
                            <th scope="col">Update</th>
                            <th scope="col">Delete</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="skill in skills" v-bind:key="skill">
                            <th scope="row">{{ skill.id }}</th>
                            <td>{{ skill.skill }}</td>
                            <td>{{ skill.description }}</td>
                            <td @click="updateSkill(skill)">
                                <button style="background-color: transparent;
padding: 0;">
                                    <font-awesome-icon icon="pencil" />
                                </button>
                            </td>
                            <td @click="deleteSkill(skill)">
                                <button style="background-color: transparent;
padding: 0;">
                                    <font-awesome-icon icon="trash" />
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
                <div>
                    <button type="button" class="btn btn-primary" @click="addNewSkill">Add New Skill</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { APIService } from "@/http/APIService";
const apiService = new APIService();
import router from "../router";
export default {
    data() {
        return {
            skills: {},
            skillSize: 0,
            showMsg: '',
        };
    },
    mounted() {
        this.getSkills();
        this.showMessages();
    },
    methods: {
        onResize() {
            if (window.innerWidth > 600)
                this.isMobile = true;
            else
                this.isMobile = false;
        },
        showMessages() {
            if (this.$route.params.msg) {
                this.showMsg = this.$route.params.msg;
            }
        },
        getSkills() {
            apiService
                .getSkillList()
                .then((response) => {
                    this.skills = response.data;
                    this.skillsSize = this.skills.length;
                })
                .catch((error) => {
                    this.showMsg = "error"
                });
        },
        addNewSkill() {
            router.push('/skill-create');
        },
        updateSkill(skill) {
            router.push("/skill-create/" + skill.id);
        },
        deleteSkill(skill) {
            if (confirm("Do you really want to delete?")) {
                apiService.deleteSkill(skill.id).then(response => {
                    if (response.status === 204) {
                        router.push('/skill-list/deleted/')
                        this.getSkills()
                        this.showMsg = "deleted"
                    }
                }).catch(error => {
                    this.showMsg = "error"
                });
            }
        }
    },
};
</script>
<style>
.table>thead>tr>th {
    background-color: rgb(190, 188, 190);
}
</style>