<template>
  <CNavbar expandable="md" type="light" color="light">
    <CToggler in-navbar @click="collapsed = !collapsed" class='bg-secondary'/>
    <img class="text-info" src="./motivo.png"/>
    <CCollapse :show="collapsed" navbar>
      <CNavbarNav >
        <CHeaderNavItem class="px-3">
          <CHeaderNavLink class="text-info" to="/dashboard/tasks">
            Tasks
          </CHeaderNavLink>
        </CHeaderNavItem>
        <CHeaderNavItem class="px-3">
          <CHeaderNavLink class="text-info" to="/dashboard/ranking" exact>
            Ranking
          </CHeaderNavLink>
        </CHeaderNavItem>
        <CHeaderNavItem class="px-3">
          <CHeaderNavLink class="text-info" to="/dashboard/coupons">
            Coupons
          </CHeaderNavLink>
        </CHeaderNavItem>
      </CNavbarNav>
      <CNavbarNav
      >
      <CHeaderNavItem class="d-md-down-none mx-2">
         <CHeaderNavLink class="text-info" to="/dashboard/">
          Score : {{coins[0].collected_coins}}
          </CHeaderNavLink>
      </CHeaderNavItem>
      </CNavbarNav>
      <CNavbarNav class="ml-auto">
        <CDropdown nav togglerText="User" placement="bottom-end">
          <CDropdownItem v-on:click.prevent="editUser">
            <CIcon name="cil-settings" /> Settings
          </CDropdownItem>
          <CDropdownItem v-on:click.prevent="logoutCustom">
            <CIcon name="cil-lock-locked" /> Logout
          </CDropdownItem>
        </CDropdown>
      </CNavbarNav>
    </CCollapse>
  </CNavbar>
</template>

<script>
  export default {
    data() {
      return {
        collapsed: false,
        coins: [],
      }
    },
    methods: {
      logoutCustom() {
        localStorage.removeItem('user-token')
        this.$router.push('/')
      },

      editUser() {
        this.$router.push('/edit')
      },
    },

    mounted() {
      const token = localStorage.getItem('user-token')
      const bearer = 'Bearer ' + token
      const id = localStorage.getItem('user-id')
      console.log(id)
      console.log(bearer)
      axios({
          method: 'get',
          url: 'https://api.motivo.localhost/profile/',
          headers: {
            'Authorization': bearer,
          }
        })
        .then(resp => {
          this.coins = resp.data.results
          console.log(resp.data.results)
          this.coins = this.coins.filter(coin => coin.user.id=id)
          console.log(this.coins[0])
          // this.title = resp.data.results[0].title
          // this.price = resp.data.results[0].price
          // this.image = resp.data.results[0].image
        })
        .catch(error => console.log(error))
    },
  }
</script>