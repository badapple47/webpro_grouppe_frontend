/* eslint-disable */
<template>
  <div class="welcome">




    <img class="text-left" id="bg" v-bind:src='"../../static/img/wallpaper.png"' height="600" width="1200" />
    <img class="text-left" id="welcome-logo" v-bind:src='"../../static/img/grouppe2.png"' height="600" width="1200" />

       <h1 id="welcome-brand"> Grouppe </h1>

       <h4 id="welcome-quote" > Grouppe คือสารานุกรมชาว EGMU ที่คุณจะไม่พลาดทุกการติดต่อ กับเพื่อนๆ พี่ๆ น้องๆ ชาววิศวะมหิดล </h4>

    <button id="login-modal"class="btn btn-default" data-toggle="modal" data-target=".login-modal" >login</button>
    <button id="register-modal"class="btn btn-primary" data-toggle="modal" data-target=".register-modal" >register</button>

       <div class="modal fade login-modal" id="myModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title" id="exampleModalLabel">Login</h1>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
          <div class="container-fluid">
               <form>
    <div class="form-group">
      <label for="usr">Username</label>
      <input type="text" class="form-control"  v-model="User.username">
    </div>
    <div class="form-group">
      <label for="pwd">Password</label>
      <input type="password" class="form-control"  v-model="User.password">
    </div>

    <button data-dismiss="modal" class="btn btn-primary" tag="button" type="button" @click="login">
              <span >login</span>
            </button>

  </form>
          </div>
            
            
            
            </div>

             
            
         
        </div>
      </div>
    </div>

    <div class="modal fade register-modal" id="myModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title" id="exampleModalLabel">Register</h1>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
                        <form>
    <div class="form-group">
      <label for="usr">Username</label>
      <input type="text" class="form-control" id="usr" v-model="User.username">
    </div>
    <div class="form-group">
      <label for="pwd">Password</label>
      <input type="password" class="form-control" id="pwd" v-model="User.password">
    </div>
    <div class="form-group">
      <label for="pwd">Confirm Password</label>
      <input type="password" class="form-control" id="conpwd" v-model="User.confirmPassword">
    </div>

      <div class="modal-body">
             <div class="panel panel-danger" v-if="checkConfirmPasswordPass == false">
          <div class="panel-heading">
            <h3 class="panel-title">ผิดพลาด</h3>
          </div>
          <div class="panel-body">
            กรุณากรอก password ให้ตรงกันทั้งสองช่อง
          </div>
        </div>
        </div>

        <div class="modal-body">
             <div class="panel panel-success" v-if="registerSuccess == true">
          <div class="panel-heading">
            <h3 class="panel-title">ยินดีด้วย</h3>
          </div>
          <div class="panel-body">
            คุณสามารใช้ username และ password นี้ login ได้ทันที
          </div>
        </div>
        </div>

    <button  @click="register" class="btn btn-primary" tag="button" type="button" disabled v-if="registerSuccess == true">
              <span >Register</span>
            </button>
            <button  @click="register" class="btn btn-primary" tag="button" type="button" v-else>
              <span >Register</span>
            </button>

  </form>
  
          </div>
         
        </div>
      </div>
    </div>






</div>

  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'home',
  data () {
    return {
      msg: 'EGCO427',
       User: {
        username: '',
        password: '',
      },
      checkConfirmPasswordPass: true,
      registerSuccess: false
    }
  },
  methods: {

    register () {

      if(this.User.password == this.User.confirmPassword){

        

        let newUser = {
        username: this.User.username,
        password: this.User.password,
      }

      
      console.log(newUser)
      axios.post('http://localhost:8082/register', newUser)
        .then((response) => {
          console.log(response)
          this.registerSuccess = true
          this.checkConfirmPasswordPass = true
          
        })
        .catch((error) => {
          console.log(error)
        })

      }else{

        
        this.checkConfirmPasswordPass = false

        

      }

      
    },

    login(){
      let newUser = {
        username: this.User.username,
        password: this.User.password,
     
      }
      console.log(newUser)
      axios.post('http://localhost:8082/authen', newUser)
        .then((response) => {
          console.log(response.data)
          if(response.data.note == "success"){
            console.log("authen success")
            localStorage.setItem('Token', 'asdasdasdasd');
            localStorage.setItem('userID', response.data.userID);
            localStorage.setItem('username', this.User.username);
           window.location.href = "http://localhost:8080/#/home"

          }
        })
        .catch((error) => {
          console.log(error)
        })
    }

  },
  mounted(){

     if (localStorage.getItem('reloaded')) {
        // The page was just reloaded. Clear the value from local storage
        // so that it will reload the next time this page is visited.
        localStorage.removeItem('reloaded');
    } else {
        // Set a flag so that we know not to reload the page twice.
        localStorage.setItem('reloaded', '1');
        location.reload();
    }
    localStorage.removeItem("Header");

    
    
  }
}

</script>

<style >


  #welcome-logo {
      /* height: 300px;
    width: 300px; */

    
  position: fixed;
  top: 38%;
  left: 50%;
  /* bring your own prefixes */
  transform: translate(-50%, -50%);

  height: 30%;
  width: 18%;
  }
  #welcome-brand {
      /* height: 300px;
    width: 300px; */

    
  position: fixed;
  top: 50%;
  left: 50%;
  /* bring your own prefixes */
  transform: translate(-50%, -50%);
  color: white;


  }

    #welcome-quote {
      /* height: 300px;
    width: 300px; */

    
  /* position: fixed; */
  /* top: 75%;
  left: 50%; */
  /* bring your own prefixes */

  margin-top: 30%;
  /* transform: translate(-50%, -50%); */
  color: white;
  /* display: inline; */


  }

  
  #login-modal {
      /* height: 300px;
    width: 300px; */

    
  position: fixed;
  top: 73%;
  left: 50%;
  width: 150px;
  height: 35px;
  /* bring your own prefixes */
  transform: translate(-50%, -50%);
  /* color: white; */

  }

    #register-modal {
      /* height: 300px;
    width: 300px; */

    
  position: fixed;
  top: 80%;
  left: 50%;
  width: 150px;
  height: 35px;
  /* bring your own prefixes */
  transform: translate(-50%, -50%);
  /* color: white; */


  }

  

  #bg {
 position: fixed; 
  top: 0; 
  left: 0; 

  /* Preserve aspet ratio */
  min-width: 100%;
  min-height: 100%;
  /* filter: contrast(50%); */
z-index:-5;
  /* filter: grayscale(20%); */


}


</style>

