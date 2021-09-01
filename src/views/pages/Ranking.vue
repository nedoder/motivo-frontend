<template>
  <CRow>
    <CCol col="12" xl="12" sm="12">
      <CCard>
        <CCardBody>
           <CRow>
      <CCol lg="2" class="bg-danger py-3">
         {{collectedFilteredItems[0].user.username}}
      </CCol>
      <CCol lg="2" class="bg-danger py-3">
         1st Place
      </CCol>
      <CCol md="4" class="bg-danger py-3">
         Initial budget: {{collectedFilteredItems[0].initial_budget}}
      </CCol>
      <CCol sm="4" class="bg-danger py-3">
         Balance: {{collectedFilteredItems[0].collected_coins}}
      </CCol>
    </CRow>
          <CDataTable
            hover
            striped
            :items="collectedFilteredItems"
            :fields="fields"
            :items-per-page="20"
            clickable-rows
            :active-page="activePage"
            @row-clicked="rowClicked"
            :pagination="{ doubleArrows: false, align: 'center'}"
            @page-change="pageChange"
          >
          </CDataTable>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Ranking',
  data () {
    return {
      items: [],
      fields: [
        { key: 'userUsername', label: 'User', _classes: 'font-weight-bold' },
        { key: 'initial_budget', label: 'Initial budget' },
        { key: 'collected_coins', label: 'Collected Coins' },
      ],
      activePage: 1
    }
  },
  computed: {
    computedItems () {
      return this.items.map(item => {
        console.log(item);
        return { 
          ...item,
          userUsername: item.user.username, 
          userId: item.user.id
        }
      })
    },
    collectedFilteredItems() {
      return this.computedItems.sort(function(a,b ) {
        return b.collected_coins - a.collected_coins
      })
    }
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
    // getBadge (status) {
    //   switch (status) {
    //     case 'Active': return 'success'
    //     case 'Inactive': return 'secondary'
    //     case 'Pending': return 'warning'
    //     case 'Banned': return 'danger'
    //     default: 'primary'
    //   }
    // rowClicked (item, index) {
    //   this.$router.push({path: `users/${index + 1}`})
    // },
    pageChange (val) {
      this.$router.push({ query: { page: val }})
    }
  },
  mounted() {
    const token = JSON.parse(JSON.stringify(localStorage.getItem('user-token')))
    const bearer = 'Bearer ' + token
      axios({
        method:'get',
        url: '/api/ranking/',
      headers: { 'Authorization': bearer, }
    })
       .then(resp=>{
          this.items = resp.data.results
          // this.title = resp.data.results[0].title
          // this.price = resp.data.results[0].price
          // this.image = resp.data.results[0].image
       })
      .catch(error => console.log(error))
  },
}
</script>
