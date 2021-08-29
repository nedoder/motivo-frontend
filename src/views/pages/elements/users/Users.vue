<template>
  <CRow>
    <CCol col="12" xl="12" sm="12">
      <CCard>
        <CCardBody>
          
          <CDataTable
            hover
            striped
            :items="computedItems"
            :fields="fields"
            :items-per-page="5"
            clickable-rows
            :active-page="activePage"
            @row-clicked="rowClicked"
            :pagination="{ doubleArrows: false, align: 'center'}"
            @page-change="pageChange"
          >
           <!-- <template #userUsername="{item}"> -->
        <td>
          <!-- {{ item }} -->
        </td>
        
           <!-- </template> -->
            <template #status="data">
              <td>
                <CBadge :color="getBadge(data.item.status)">
                  {{data.item.status}}
                </CBadge>
              </td>
            </template>
          </CDataTable>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
import usersData from './UsersData'
export default {
  name: 'Users',
  data () {
    return {
      items: usersData,
      fields: [
        { key: 'userUsername', label: '', _classes: 'font-weight-bold' },
        { key: 'initial_budget' },
        { key: 'annual_budget' },
        { key: 'collected_coins' }
      ],
      activePage: 1
    }
  },
  computed: {
    computedItems () {
      return usersData.map(item => {
        return { 
          ...item,
          userUsername: item.user.username, 
          userId: item.user.id
        }
      })
    },
  },
  watch: {
    $route: {
      immediate: true,
      handler (route) {
        if (route.query && route.query.page) {
          this.activePage = Number(route.query.page)
        }
      }
    }
  },
  methods: {
    getBadge (status) {
      switch (status) {
        case 'Active': return 'success'
        case 'Inactive': return 'secondary'
        case 'Pending': return 'warning'
        case 'Banned': return 'danger'
        default: 'primary'
      }
    },
    rowClicked (item, index) {
      this.$router.push({path: `users/${index + 1}`})
    },
    pageChange (val) {
      this.$router.push({ query: { page: val }})
    }
  }
}
</script>
