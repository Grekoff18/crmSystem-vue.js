<template>
    <div>
        <Loader v-if="loading"/>
        <div class="app-main-layout" v-else>
            <NavBar @click="isOpen = !isOpen" />
            <SideBar v-model="isOpen"/>

            <main
              class="app-content"
              :class="isOpen ? '' : 'full'"
            >
                <div class="app-page">
                    <router-view/>
                </div>
            </main>

            <div class="fixed-action-btn">
                <router-link
								 	class="btn-floating btn-large blue"
								 	to="/record"
									v-tooltip="'Create a new entry'"
								>
                    <i class="large material-icons">add</i>
                </router-link>
            </div>
        </div>
  </div>
</template>

<script>
import NavBar from "../components/app/NavBar"
import SideBar from "../components/app/SideBar"

export default {
  name: "main-layout",

  components: {
    NavBar, SideBar
  },

  data() {
    return {
      isOpen: true,
      loading: true,
    }
  },

  async mounted() {
    if (!Object.keys(this.$store.getters.info).length) {
      await this.$store.dispatch("fetchInfo")
    }
    this.loading = false
	},
	
	computed: {
		error() {
			return this.$store.getters.error
		}
	},

	watch: {
		error(firebaseError) {
			this.$error(firebaseError.message)	
		}
	},

}

</script>