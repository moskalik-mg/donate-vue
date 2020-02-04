<template>
  <div id="app">
    <div class="container">
      <div class="app__list-comments">
        <transition-group name="kik">
        <appListComments v-for="(item, index) in userComment" 
        v-bind:key="index" 
        :commentItem="item" 
        :index="index"
       @deleteComment="deleteComment">
       </appListComments>
       </transition-group>
      </div>
      <div class="app__controls">
        <form action="#" @submit.prevent="sendMessage">
          <div>
            <input type="text" v-model="message" class="input-comment" placeholder="Enter comment">
          </div>
          <input type="button" value="Donate author" disabled class="btn btn-donate">
          <transition name="btn-send">
            <input type="button" value="Send" class="btn btn-send" v-if="message.length > 0" @click="sendMessage">
          </transition>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import listComments from './components/listComments.vue'

export default {
  name: 'app',
  data(){
    return{
      message: '',
      deleteEdit: false,
      userComment: [{
        name: 'Maks',
        comment: 'First comment',
        avatar: require('@/images/avatar.jpg')
      },
      {
        name: 'Maks',
        comment: 'Second comment',
        avatar: require('@/images/avatar.jpg')
      }
      ]
    }
  },
  methods:{
    sendMessage(){
      if(this.message.length > 0){
        let item = {
          name: 'Maks',
          comment: this.message,
          avatar: require('@/images/avatar.jpg')
        }

        this.userComment.push(item);

        this.message = '';
      }
      else{
        return false;
      }
      // /* eslint-disable no-console */
      // console.log(this.userComment);
      // /* eslint-enable no-console */
    
    },
    deleteComment(index){
      this.userComment.splice(index, 1);
    }

  },
  components:{
    appListComments: listComments 
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap');

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body{
  font-family: 'Roboto', sans-serif;
}
#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

.btn{
  padding: 10px 20px;
  border-radius: 5px;
  font-family: 'Roboto', sans-serif;
  border: none;
  background: none;
  cursor: pointer;
}

.btn:disabled{
  opacity: .7;
  cursor: auto;
}

.btn-donate[type=button]{
  background: #6772e5;
  border: none;
  color: #fff;
  margin-right: 20px;
}

.container{
  display: flex;
  max-width: 1200px;
  margin: 0 auto;
}

.app__list-comments{
  max-width: 65%;
  width: 100%;
  padding: 0 15px;

  overflow-y: scroll;
  max-height: 600px;
}
.app__controls{
  max-width: 35%;
  width: 100%;
  padding: 0 15px;
}



.btn-send{
  background-color: #25343b;
  color: #fff;
}

.input-comment{
  padding: 10px 20px;
  border: none;
  border-bottom: 1px solid #8e8e8e;
  margin-bottom: 30px;
  width: 100%;
}


.input-comment:focus{
  outline: none;
}


.btn-send-enter-active, .btn-send-leave-active{
  transition: all .4s ease;
}

.btn-send-enter, .btn-send-leave-to{
  transform: translateX(50px);
  opacity: 0;
}


.kik-enter-active, .kik-leave-active{
  transition: all .4s ease;
}

.kik-enter, .kik-leave-to{
  transform: translateY(50px);
  opacity: 0;
}

</style>
