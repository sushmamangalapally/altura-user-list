<template>
  <div class="modal-mask">
    <div class="modal-container">
      <div class="user-info-container">
          <div class="profile-picture">
            <img src="../assets/user.svg" class="user" alt="User Symbol" />
          </div>
          <h2>{{userInfo.firstName}} {{userInfo.lastName}}</h2>
          <span class="username">{{userInfo.username}}</span>
          <div class="user-information">
            <div class="header">ID:</div>
            <div class="header-info">{{userInfo.id}}</div>
          </div>
          <div class="user-information">
            <div class="header">Phone Number:</div>
            <div class="header-info">{{userInfo.phoneNumber}}</div>
          </div>
      </div>
      <div class="dialog">
        <button @click="$emit('close')">Close</button>
      </div>
    </div>
  </div>
</template>

<script>
import {computed, defineComponent, defineProps, ref} from 'vue';
import { getData } from "../api";


export default defineComponent({
  name: 'Dialog',
  props: {
    user: {
      type: Object,
      required: true
    }
  },

  setup(props) {
    console.log(props.user)

    const userInfo = props.user;


    return {
      userInfo
    }
    

  },
});
</script>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
.modal-container {
  width: 300px;
  margin: 150px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
}

.user-info-container {
  display: flex;
  /* align-items: center; */
    flex-direction: column;

}

/* .profile-picture {
  width: 60px;
    height: 60px;
    overflow: hidden;
    margin-right: 8px;
} */

.profile-picture img{
    width: 60px;
    height: 60px;
    border: none;
    overflow: hidden;
    margin-right: 8px;
}
.user-information {
      display: flex;
    align-items: start;
    flex-direction: column;
    margin-bottom: 20px;
}

.header {
      font-size: 0.875rem;
    line-height: 1.25rem;
}

.header-info, .username {
      font-size: 1em;
    line-height: 1.25rem;
    font-weight: 600;
}
</style>
