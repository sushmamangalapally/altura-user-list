<template>
  <div class="user-container">
    <ul class="users">
      <li
        v-for="(user) in users"
        :key="user.id"
        @click="handleUserSelect(user.id)"
        class="user">
        <label>
          <div class="user-info">
            <div class="img-wrap">
              <img src="../assets/user.svg" class="user" alt="User Symbol" />
            </div>
            <div class="category">{{user.firstName}} {{user.lastName}}</div>
          </div>
          <div class="delete" @click="deleteUser(user.id)">
            <span>
              <img src="../assets/trash.svg" class="trash" alt="Delete Symbol" />
            </span>
          </div>
        </label>
      </li>	
    </ul>

    <Dialog v-if="showDialog" :user="selectedUser" @close="showDialog = false" />
  </div>
</template>

<script>
import {defineComponent, ref, onMounted} from 'vue';
import { getData } from "../api";
import Dialog from '../components/Dialog.vue'

export default defineComponent({
  name: 'ListNames',
  components: {
    Dialog,
  },
  setup() {
    const users = ref([]);
    const showDialog = ref(false);
    const selectedUser = ref(null);

    async function getUsers() {
      const response = await getData();
      users.value = await response.users;
    }

    function handleUserSelect(userId) {
      showDialog.value = false;
      const foundUser = users.value.filter((user) => user.id == userId);
      selectedUser.value = foundUser && foundUser.length ? foundUser[0] : null;
      showDialog.value = true;
    }

    function deleteUser(userId) {
      users.value = users.value.filter((user) => user.id !== userId);
    }

    onMounted(getUsers);

    return {
      users,
      showDialog,
      selectedUser,
      handleUserSelect,
      deleteUser
    }
  },
});
</script>

<style scoped>
 ul.list{
  display: table;
  width: 100%;
  padding: 0;
}
.list li{
  display: table-row;
  border: 1px solid black;
  box-sizing: border-box;
}
.list li span{
  display: table-cell;
  text-align: left;
  padding: 6px 9px;
  border: 1px solid black;
}
.list li.header {
  font-weight: bold;
  text-align: center;
  text-transform: uppercase;
}
.users {
  list-style: none;
  overflow: auto;
  max-height: 500px;
  margin: 20px auto 50px auto;
  width: 400px;
  border: 1px solid rgba(37, 40, 46, 0.3);
  padding: 0px;
}
li.user:hover {
  background: rgb(241, 241, 241);
}
.users .user label {
  gap: 50px;
  display: grid;
  grid-template-columns: 200px 20px 20px;
}
.user {
  padding: 10px 20px;
  border-bottom: 1px solid rgba(37, 40, 46, 0.3);
}
.user-info {
  display: flex;
  align-items: center;
  position: relative;
  cursor: pointer;
}
img.user, img.trash {
  width: 40px;
  height: 40px;
  border: none;
  overflow: hidden;
  margin-right: 8px;
}
.delete {
  display: flex;
  align-items: center;
}
img.trash {
  width: 20px;
  height: 20px;
  cursor: pointer;
}
</style>
