<template>
  <CDropdown
    inNav
    class="c-header-nav-items"
    placement="bottom-end"
    add-menu-classes="pt-0"
  >
    <template #toggler>
      <CHeaderNavLink>
        <div class="c-avatar">
          <img
            src="img/avatars/6.jpg"
            class="c-avatar-img "
          />
        </div>
      </CHeaderNavLink>
    </template>
    <CDropdownItem>
      <CHeaderNavLink to="/userinfo"> 
          <CIcon name="cil-user"/> Profile
        </CHeaderNavLink>
    </CDropdownItem>
    <CDropdownItem>
      <CIcon name="cil-settings" /> Settings
    </CDropdownItem>
    <CDropdownItem  v-on:click.prevent="logoutCustom" >
      <CIcon name="cil-lock-locked"/> Logout
    </CDropdownItem>
  </CDropdown>
</template>

<script>
export default {
  name: 'TheHeaderDropdownAccnt',
  data () {
    return { 
      itemsCount: 42
    }
  },

  methods:{
    logoutCustom() {
      localStorage.removeItem('user-token')
      this.$router.push('/')
    }
  },
}
</script>

<style scoped>
  .c-icon {
    margin-right: 0.3rem;
  }
</style>