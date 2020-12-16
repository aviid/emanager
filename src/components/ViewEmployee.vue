<template>
    <div id="view-employee">
        <ul class="list-group py-2">
            <li class="list-group-item active"><h2>{{name}}</h2></li>
            <li class="list-group-item">Employee ID: {{employee_id}}</li>
            <li class="list-group-item">Department: {{dept}}</li>
            <li class="list-group-item">Position: {{position}}</li>
        </ul>  
        <router-link to="/" class="btn btn-secondary">Back</router-link>  
        <button @click="deleteEmployee" class="btn btn-danger">Delete</button>

        <router-link v-bind:to="{name:'edit-employee', params:{employee_id:employee_id}}" class="btn btn-secondary float-right"><i class="fa fa-pencil"></i>Edit</router-link>
    </div>    
</template>

<script>
import db from './firebaseInit'

export default {
    name:'view-employee',
    data(){
        return {
            employee_id : null,
            name : null,
            dept : null,
            position : null
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
        deleteEmployee(){
            if(confirm('Are you sure')){
                db.collection('employees').where('employee_id','==',this.$route.params.employee_id).get()
                .then(snapShot=>{
                    snapShot.forEach(doc=>{
                        doc.ref.delete()
                        this.$router.push("/")
                    })
                })
            }
            
        }
    }
}
</script>

