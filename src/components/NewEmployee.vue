<template>
    <div id="new-employee">
        <h3>New Employee</h3>
        <form @submit.prevent="saveEmployee">
            <div class="row">
                <div class="col-md-12">
                    <label for="employee_id">Employee ID:</label>
                    <input type="text" class="form-control" id="employee_id" name="employee_id" v-model="employee_id">   
                </div>
            </div>
            
            <div class="row">
                <div class="col-md-12">
                    <label for="name">Name</label>
                    <input type="text" class="form-control" id="name" name="name" v-model="name" required>
                </div>
            </div>
            
            <div class="row">
                <div class="col-md-12">
                    <label for="name">Department</label>
                    <input type="text" class="form-control" id="dept" name="dept" v-model="dept" required>
                </div>
            </div>

            <div class="row">
                <div class="col-md-12">
                    <label for="name">Position</label>
                    <input type="text" class="form-control" id="name" name="position" v-model="position" required>
                </div>
            </div>
            
            <div class="row py-2">
                <div class="col-md-12">
                    <button type="submit" class="btn btn-primary">Save</button>
                    <router-link to="/" class="btn btn-secondary">Back</router-link>
                </div>
            </div>
            
        </form>  
    </div>    
</template>

<script>
import db from './firebaseInit'

export default {
    name:'new-employee',
    data(){
        return{
            employee_id:null,
            name: null,
            dept:null,
            position: null
        }
    },
    methods:{
        saveEmployee(){
            db.collection("employees").add({
                employee_id:    this.employee_id,
                name:           this.name,
                dept:           this.dept,
                position:       this.position
            }).then(doc=>{
                this.$router.push("/") 
            })
        }
    }
}
</script>

