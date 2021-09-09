<template>
  <CNavbar expandable="md" type="light" color="light" class="bg-white" v-bind:style="{borderBottom: '2px solid #EBEDF0'}"> 
    <CToggler in-navbar @click="collapsed = !collapsed" class='bg-secondary'/>
    <img class="text-info" src="./motivo.png"/>
    <CCollapse :show="collapsed" navbar>
      <CNavbarNav >
        <CHeaderNavItem class="px-5">
          <CHeaderNavLink class="text-info" to="/dashboard/challenges" v-bind:style="{textDecoration: 'none'}">
            Challenges
          </CHeaderNavLink>
        </CHeaderNavItem>
        <CHeaderNavItem class="px-5">
          <CHeaderNavLink class="text-info" to="/dashboard/ranking" v-bind:style="{textDecoration: 'none'}">
            Ranking
          </CHeaderNavLink>
        </CHeaderNavItem>
        <CHeaderNavItem class="px-5">
          <CHeaderNavLink class="text-info" to="/dashboard/coupons" v-bind:style="{textDecoration: 'none'}">
            Coupons
          </CHeaderNavLink>
        </CHeaderNavItem>
      </CNavbarNav>
      <CNavbarNav
      >

      </CNavbarNav>
      <CNavbarNav class="ml-auto" >
         <CHeaderNavItem class='p-2' v-bind:style="{color: '#F2C94C', fontWeight: 'bold'}">
          <img class="text-info" src="./Coin.png"/> {{this.coins[0].collected_coins}}
      </CHeaderNavItem>
    
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

      update() {
      const token = localStorage.getItem('user-token')
      const bearer = 'Bearer ' + token
      const id = localStorage.getItem('user-id')
      console.log(id)
      console.log(bearer)
      axios({
          method: 'get',
          url: '/api/profile/',
          headers: {
            'Authorization': bearer,
          }
        })
        .then(resp => {
          this.coins = resp.data.results
          let userid = parseInt(localStorage.getItem('user-id'))
           this.coins = this.coins.filter(result => result.user.id === userid)
        })
        .catch(error => console.log(error))
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
          url: '/api/profile/',
          headers: {
            'Authorization': bearer,
          }
        })
        .then(resp => {
          this.coins = resp.data.results
          let userid = parseInt(localStorage.getItem('user-id'))
           this.coins = this.coins.filter(result => result.user.id === userid)
        })
        .catch(error => console.log(error))


      window.addEventListener('click', (event) => { 
        this.update();
    })

    },
  }
</script>