<template>
    <div id="dashboard">

        <ul class="list-group py-3">
            <li class="list-group-item active"><strong>Employees</strong></li>

            <li class="list-group-item " v-for="employee in employees" v-bind:key="employee.id">
                {{employee.employee_id}}: {{ employee.name}}
                <router-link v-bind:to="{ name:'view-employee', params:{employee_id:employee.employee_id}}" class="float-right"> <i class="fa fa-eye"></i> </router-link>
            </li>
        </ul>

        <router-link v-bind:to="{name:'new-employee'}" class="btn btn-primary">
            <i class="fa fa-plus"></i>
        </router-link>
           
    </div>    
</template>

<script>
import db from './firebaseInit';

export default {
    name:'dashboard',
    data(){
        return{ 
            employees:[]
        }
    },
    created(){
        db.collection("employees").orderBy("dept").get().then(snaptshot=>{
            snaptshot.forEach(doc=>{
                const data = {
                    'id':           doc.id,
                    'employee_id':  doc.data().employee_id,
                    'name':         doc.data().name,
                    'dept':         doc.data().dept,
                    'position' :    doc.data().position
                }

                this.employees.push(data);
            })
        });
    }
}
</script>

