<template>
    <div class='homePage'>
      <h1>Workers</h1>
    <div class="topContainer">
      <div class="addSection">
    <input v-model="fName"  type="text" placeholder="First Name"/>
    
    <input v-model="lName"  type="text" placeholder="Last Name"/>
    <select v-model="dept"  id="dept">
      <option value="IT">IT</option>
      <option value="Marketing">Marketing</option>
      <option value="Sales">Sales</option>
      <option value="Administration">Administration</option>
    </select>
    <input  v-model="sal" type="number" placeholder="Salary in USD"/>
    <button @click="pushData">+ Add </button>
  </div>
  <div class="error">{{errorMsg}}</div>
    <div class='SearchForm'>
    <input v-model="sName"  type="text" placeholder="Search By Name"/>
    <select v-model="sDept"  id="dept" style="width:145px">
      <option value="All">All</option>
      <option value="IT">IT</option>
      <option value="Marketing">Marketing</option>
      <option value="Sales">Sales</option>
      <option value="Administration">Administration</option>
    </select>
    <input  v-model="min" type="number" placeholder="Salary in USD" style="margin-left: 0;margin-right: 0;"/>
    <span> - </span>
    <input  v-model="max" type="number" placeholder="Salary in USD"/>
    
    <button @click="filter">Search </button>
    <button @click="clearFilter">Clear Filters </button>
    </div>
    </div>
     <div class="tableFixHead">
          <table class="containerTable" border="1">
            <thead>
              <tr>
                <th>First Name</th>
                <th>Last Name</th>
                <th>Department</th>
                <th>Salary(in USD)</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(emp,i) in employees" :key="i">
                <td>{{emp.firstName}}</td>
                <td>{{emp.lastName}}</td>
                <td>{{emp.dept}}</td>
                <td>{{emp.salary}} USD</td>
              </tr>
              <tr>
                <td> &nbsp; </td>
                <td> </td>
                <td> </td>
                <td> </td>
              </tr>
              <tr>
             <td> </td>
             <td> </td>
             <td style="fontWeight:bold;"> Grand Total </td>
             <td style="fontWeight:bold;">{{total}} USD </td>
              </tr>
            </tbody>
          </table>
        </div>
    </div>
    </template>
    
    <script>
    export default {
      name: 'HomePage',
     data(){
       return {
         errorMsg:'',
         dept:'IT',
         fName:'',
         lName:'',
         sal:0,
         sName:'',
         sDept:'All',
         max:0,
         min:0,
         total:0,
         tmpEmp:[],
         employees: [{
           firstName:"John",
           lastName:"Smith",
           dept:"IT",
           salary:3000
         },
         {
           firstName:"Jane",
           lastName:"Doe",
           dept:"Marketing",
           salary:3000.5
         }
         ]
       }
     },
     mounted(){
     this.tmpEmp = this.employees
     this.calculateTotal()
     },
     methods:{
       clearFilter(){
         this.sName= ''
         this.sDept= 'All'
         this.max= 0
         this.min= 0
         this.filter()
       },
       pushData(){
         if(this.dept && this.fName && this.lName && this.sal) {
           this.employees.push({
            firstName:this.fName,
           lastName:this.lName,
           dept:this.dept,
           salary:this.sal
           })
           this.errorMsg = ''
         } else {
           this.errorMsg = 'All fields are mendatory'
         }
         this.tmpEmp =  this.employees
         this.fName = ''
         this.lName = ''
         this.dept = 'IT'
         this.sal = 0
         this.calculateTotal()
       },
       filter() {
         this.employees = this.tmpEmp
         let tmp = []
    
          this.employees.map(emp =>{
            //For Name
             if(emp.firstName?.toLowerCase().includes(this.sName.toLowerCase()) || emp.lastName.toLowerCase().includes(this.sName.toLowerCase()) || !this.sName) { 
              //For Salary Range 
               if((emp.salary > this.min && emp.salary < this.max) || !this.max) {
                //For Dept
                if(emp.dept === this.sDept || this.sDept === 'All') { 
                   tmp.push(emp)
                }
             }
             }
    
            })
            this.employees = tmp
    
            this.calculateTotal()
       },
       calculateTotal(){
           this.total= 0
          this.employees.map(emp =>{
           this.total =  this.total + emp.salary
          })
       }
     }
    }
    
    </script>
    <style scoped>
   .homePage{
    display: flex;
    flex-direction: column;
    
   }
   .topContainer{
    text-align: left;
   }
    .topContainer input, .topContainer select{
      padding: 5px;
      margin: 0 10px 10px 0;
      width: 150px;
    }
    .topContainer button{
      background-color: skyblue;
      width: 100px;
      height: 30px;
      border: 0;
      margin-left: 10px;
      cursor: pointer;
    }
          .tableFixHead {
           margin-top: 2%;
            overflow-y: auto;
            height: 60%;
            width: 80%;
          }
          .tableFixHead thead th {
            position: sticky;
            top: 0;
          }
          table.containerTable {
            width: 100%;
            border: 1px solid #ccc;
            text-align: center;
          }
          th,
          td {
            padding: 8px 16px;
          }
          th {
            background: #eee;
          }
        .error{
          color: red;
          font-size: 12px;
          margin-bottom: 10px;
        }
    </style>
    