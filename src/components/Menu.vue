<template>
  <div class="ui secondary menu">
    <div class="ui container">
      <div class="left menu">
        <router-link class="item" to="/">
          <img src="@/assets/logo.png" alt="Ecommerce" class="ui small image" />
        </router-link>
      </div>
      <template v-for="category in categorias" :key="category.id">
        <router-link class="item" to="category.slug">{{
          category.title
        }}</router-link>
      </template>

      <div class="right menu">
        <router-link class="item" to="/login" v-if="!token">
          Login
        </router-link>
        <template v-if="token">
          <router-link to="/orders" class="item">Pedidos</router-link>
          <span class="ui item cart"><i class="shopping cart icon"></i></span>
          <span class="ui item logout" @click="logout">
            <i class="power off icon"></i>
          </span>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import { getTokenApi, deleteTokenApi } from "../api/token";
import { getCategoriesApi } from "../api/category";
export default {
  name: "Menu",
  setup() {
    let categorias = ref(null);
    const token = getTokenApi();

    onMounted(async () => {
      const response = await getCategoriesApi();
      categorias.value = response;
      console.log(categorias.value);
    });

    const logout = () => {
      deleteTokenApi();
      location.replace("/");
    };
    return {
      token,
      logout,
      categorias,
    };
  },
};
</script>

<style lang="scss" scoped>
.ui.menu.secondary {
  background-color: #16202b;
  .item {
    color: #fff;
    &:hover {
      color: #1fa1f1;
    }
  }
  .menu.left {
    width: 50%;
    .ui.image {
      width: 40px;
    }
  }

  .menu.right {
    width: 50%;
    justify-content: flex-end;

    .logout,
    .cart {
      &:hover {
        cursor: pointer;
      }
    }
  }
}
</style>
