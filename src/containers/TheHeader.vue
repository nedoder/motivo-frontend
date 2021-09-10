<template>
  <CNavbar expandable="md" type="light" color="light" class="bg-white" v-bind:style="{borderBottom: '2px solid #EBEDF0'}"> 
    <CToggler in-navbar @click="collapsed = !collapsed" class='bg-secondary'/>
    <img class="text-info" src="./motivo.png"/>
    <CCollapse :show="collapsed" navbar>
      <CNavbarNav >
        <CHeaderNavItem class="px-5">
          <CHeaderNavLink class="text-info" to="/dashboard/challenges" v-bind:style="{textDecoration: 'none', color: '#99A2AD !important'}">
            <img src="./Tasks (1).png"/> Challenges
          </CHeaderNavLink>
        </CHeaderNavItem>
        <CHeaderNavItem class="px-5">
          <CHeaderNavLink class="text-info" to="/dashboard/ranking" v-bind:style="{textDecoration: 'none', color: '#99A2AD  !important'}">
            <img src="./Ranking.png"/> Ranking
          </CHeaderNavLink>
        </CHeaderNavItem>
        <CHeaderNavItem class="px-5">
          <CHeaderNavLink class="text-info" to="/dashboard/awards" v-bind:style="{textDecoration: 'none', color: '#99A2AD  !important'}">
            <img src="./Coupon.png"/> Awards
          </CHeaderNavLink>
        </CHeaderNavItem>
      </CNavbarNav>
      <CNavbarNav
      >

      </CNavbarNav>
      <CNavbarNav class="ml-auto" >
         <CHeaderNavItem class='p-2' v-bind:style="{color: '#F2C94C', fontWeight: 'bold'}">
          <img class="text-info" src="./Coin.png"/> {{this.coins[0].collected_coins}}
          <img class="text-info" src="./Ellipse 23.png"/>
      </CHeaderNavItem>
    
      <CDropdown nav placement="bottom-end">
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

import axios from 'axios'

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
      axios({
          method: 'get',
          url: 'https://api.motivo.localhost/profile/',
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
      axios({
          method: 'get',
          url: 'https://api.motivo.localhost/profile/',
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