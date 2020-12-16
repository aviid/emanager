<template>
    <div id="edit-employee">
        <h3>New Employee</h3>
        <form @submit.prevent="updateEmployee">
            <div class="row">
                <div class="col-md-12">
                    <label for="employee_id">Employee ID:</label>
                    <input type="text" class="form-control" id="employee_id" name="employee_id" v-model="employee_id" disabled>   
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
                    <button type="submit" class="btn btn-primary">Update</button>
                    <router-link to="/" class="btn btn-secondary">Back</router-link>
                </div>
            </div>
            
        </form>     
    </div>    
</template>

<script>
import db from './firebaseInit'

export default {
    name:'edit-employee',
    data(){
        return{
            employee_id: null,
            name: null,
            dept: null,
            position: null
        }
    },
    beforeRouteEnter(to, from, next){
        db.collection('employees').where('employee_id','==',to.params.employee_id).get()
        .then(snapShot=>{
            snapShot.forEach(doc=>{
                next(vm => {
                    vm.employee_id  = doc.data().employee_id
                    vm.name         = doc.data().name
                    vm.dept         = doc.data().dept
                    vm.position     = doc.data().position
                })
            })
        })
    },
    watch:{
        '$route':'fetchData'
    },
    methods:{
        fetchData(){
            db.collection('employees').where('employee_id','==', this.$route.params.employee_id).get()
            .then(snapShot=>{
                snapShot.forEach(doc=>{
                    this.employee_id = doc.data().employee_id
                    this.name        = doc.data().name
                    this.dept        = doc.data().dept
                    this.position    = doc.data().position
                })
            })
        },
        updateEmployee(){
            db.collection("employees").where('employee_id','==',this.$route.params.employee_id).get()
            .then(snapShot=>{
                snapShot.forEach(doc=>{
                    doc.ref.update({
                        employee_id: this.employee_id,
                        name: this.name,
                        dept: this.dept,
                        position: this.position
                    }).then(()=>{
                        this.$router.push({name:'view-employee', params:{employee_id:this.employee_id}})
                    })
                });
            })
        }
    }
}
</script>

