<template>
  <div>
    <v-container fluid>
      <v-row justify="center">
        <v-col cols="8">
          <v-card flat color="accent" dark>
            <v-card-title class="headline">You can use the toolbar above to navigate around, or click one of the buttons below...</v-card-title>
            <v-card-text>
              <v-row justify="space-around" align="center">
                <v-col cols="12" v-for="(item, index) in menuOptions" :key="index">
                  <v-btn block color="primary" height="40px" :disabled="item.requiresManager && !isManager" @click="routeChoice(index)">{{item.title}}</v-btn>
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  components: {},
  data: () => ({
    menuOptions: [
      {
        title: "View Product List",
        requiresManager: false
      },
      {
        title: "Create New Employee",
        requiresManager: true
      },
      {
        title: "Start Transaction",
        requiresManager: true
      },
      {
        title: "Employee Details",
        requiresManager: true
      },
    ],
    isManager: false
  }),
  mounted() {
    if (this.$store.state.employeeCount <= 0) {
      this.$router.push({ name: 'employee' })
    } else if (localStorage.getItem('employee') === null) {
      this.$router.push({name: 'login'})
    }

    this.$store.dispatch("RETRIEVE_LOCAL")
    if (this.$store.state.employee.classification === 1 || this.$store.state.employee.classification === 2) {
      this.isManager = true
    }
  },
  methods: {
    routeChoice(choice) {
      if (choice === 0) {
        this.$router.push({ name: "products" })
      } else if (choice === 1 || choice === 3) {
          this.$router.push({ name: "employee"})
      } else if (choice === 2) {
        this.$router.push({ name: "products" })
      } else {
        alert("This function is not implemented yet")
      }
    }
  }
}
</script>
