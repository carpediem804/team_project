<template>
  <div id="app">
      <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/all.css">
    <div id="nav">
      <div>
        <div class="navbar-menu">
          <div class="navbar-end">
            <button class="button is-warning is-normal" v-if="!loggedIn" @click="signUpPop=true">sign up</button>
            <button class="button is-primary is-normal" v-if="!loggedIn" @click="loginPop=true">login</button>
            <b-modal :active.sync="loginPop" has-modal-card>
              <login></login>
            </b-modal>

            <b-modal :active.sync="signUpPop" has-modal-card>
              <signUp></signUp>
            </b-modal>

            <b-modal :active.sync="profilePop" has-modal-card>
              <addProfile></addProfile>
            </b-modal>

            <b-modal :active.sync="writePop" has-modal-card>
              <showContent></showContent>
            </b-modal>
            <button class="button is-warning" v-if="loggedIn" @click="profilePop=true">프로필 수정</button>
            <button class="button is-light" v-if="loggedIn" @click="writePop=true">글쓰기</button>
            <button class="button is-link" v-if="loggedIn" v-on:click="logout()">logout</button>
          </div>
        </div>
        <div v-if="loggedIn">
          아이디:{{this.$store.state.user.email}}
        </div>
          <div class="card" style="width:70%; margin: auto">
              <app-header></app-header>
              <router-view></router-view>
          </div>

      </div>
    </div>
  </div>
</template>
<script>
    // Imports
    import header from './components/header.vue';
    import { mapGetters } from 'vuex'
    import firebase from 'firebase'
    import BModal from "buefy/src/components/modal/Modal";
    import login from './components/Login.vue'
    import signUp from './views/SignUp.vue'
    import addProfile from './views/addProfile.vue'
    import showContent from './components/showContent'
    import BDropdown from "buefy/src/components/dropdown/Dropdown";
    import BDropdownItem from "buefy/src/components/dropdown/DropdownItem";
    export default {
        components: {
            BDropdownItem,
            BDropdown,
            BModal,
            'app-header': header,
            firebase,
            login,
            signUp,
            addProfile,
            showContent
        },
        data () {
            return {
                loginPop:false,
                signUpPop:false,
                profilePop:false,
                writePop:false
            }
        },
        methods: {
            logout:function(){
                firebase.auth().signOut().catch(function(err){
                    alert(err)
                }).then(()=>{
                    localStorage.clear()
                    this.$store.state.user={}
                    this.$store.state.loggedIn=false
                    alert("로그아웃")
                })
            }
        },
        computed:{
            ...mapGetters({
                user:'user',
                loggedIn:'loggedIn'
            })
        },
        created(){
            let getUser=JSON.parse(localStorage.getItem('newUser'))
            let logState=JSON.parse(localStorage.getItem('logState'))
            if(logState !==false)
            {
                console.log(getUser)
                this.$store.state.loggedIn=logState
                this.$store.state.user=getUser
            }
            console.log(this.$store.state.user)


        },
    }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
    background-color: #EEE;
    background-size: cover;
    height: 1500px;

}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: black;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
