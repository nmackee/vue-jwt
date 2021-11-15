<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-primary bg-gradient">
      <div class="container-fluid">
        
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0" v-if="getUserProfile.id == 0">
            <li class="nav-item">
              <router-link to="/login" class="nav-link">Login</router-link>
            </li>
            
          </ul>
          <ul class="navbar-nav me-auto mb-2 mb-lg-0" v-if="getUserProfile.id !== 0">
            <li class="nav-item">
              <span class="badge bg-primary">{{ getUserProfile.email }}</span>
            </li>
            <li>
              <router-link to="/dashboard" class="nav-link">Dashboard</router-link>
            </li>
              <li class="nav-item">
              <span @click="logout()" class="badge bg-primary">Logout</span>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <router-view></router-view>
  </div>
</template>

<script>
import { mapGetters, mapActions, mapMutations } from 'vuex';
export default {
  name: "App",
  components: {},
  computed: {
    ...mapGetters("auth", {
      getUserProfile: "getUserProfile",
      getLogout: "getLogout"
    })
  },
  methods: {
    ...mapActions("auth", {
      userLogout: "userLogout",
    }),
    ...mapMutations("auth", {
      setUserProfile: "setUserProfile",
      setLogout: "setLogout",
    }),
    async logout() {
      await this.userLogout();
      if(this.getLogout) {
        const resetUser = {
          id: 0,
          lastName: "",
          firstName: "",
          email: "",
          phone: "",
        };
        this.setUserProfile(resetUser);
        this.setLogout(false);
        this.$router.push("/login");
      }
    },
  },

};
</script>