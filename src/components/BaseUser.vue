<template>
    <transition-group tag="ul" name="user-list">
        <li v-for="user in Users" :key="user" @click="removeUser(user)">{{user}}</li>
    </transition-group>
    <div>
        <input type="text" v-model="userName">
        <button @click="addUser">Add User</button>
    </div>
</template>
<script>
export default {
    data(){
        return{
            Users:['Max','Manu','Julie','Angla','Michael'],
            userName:'',
        }
    },
    methods:{
        removeUser(user){
           this.Users=this.Users.filter(usr=>usr!==user);
        },
        addUser(){
            this.Users.unshift(this.userName);
        }
    }
}
</script>
<style>
ul{
    list-style: none;
    padding:1rem 0;
    margin:0;
}
li{
border:1px solid gray;
padding:1rem;
text-align: center;
cursor: pointer;
}

.user-list-enter-from{
    opacity: 0;
    transform: translateX(-30px);
}
.user-list-enter-active{
    transition: all 1s ease-out;
}
.user-list-enter-to,
.user-list-leave-from{
    opacity: 1;
    transform: translateX(0);
}
.user-list-leave-to{
    opacity: 0;
    transform: translateX(30px);
}
.user-list-leave-active{
    transition: all 1s ease-in;
    position: absolute;
}
.user-list-move{
    transition: transform 0.8s ease;
}
</style>