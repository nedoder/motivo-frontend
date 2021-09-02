<template>
  <CNavbar expandable="md" type="light" color="light">
    <CToggler in-navbar @click="collapsed = !collapsed" class='bg-secondary'/>
    <CNavbarBrand class="text-info" href="#">MOTIVO</CNavbarBrand>
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
        collapsed: false
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
  }
</script>