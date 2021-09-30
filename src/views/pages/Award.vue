<template>
  <CRow class="d-flex justify-content-center">
    <CCol
      col="12"
      lg="6"
    >
      <!-- <form @submit.prevent="submitForm"> -->
      <CCard :style="{height: '550px', borderRadius:'18px', backgroundSize: 'cover', top:'20%'}">
        <!-- <CCardHeader v-bind:style="{backgroundColor: '#1CB0F6 !important'}">

        </CCardHeader>  -->

        <CCardBody
          class="card-title"
          :style="{
            color: 'black',
            padding: '20px 10px',
            margin: '0px',
            borderRadius: '18px',
            backgroundColor: '#fff'
          }"
        >
          <CRow
            class="m-3"
            :style="{color: '#F2C94C', fontWeight: 'bold', fontSize: '18x'}"
          >
            {{ awards.price_in_coins }} <img
              src="./img/Coin.png"
              class="mx-2"
            >
          </CRow>
          <CRow
            class="m-auto"
            :style="{
              display: 'flex',
              position: 'relative' ,
              width: '120px',
              height: '135px',
              clipPath: 'polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%)'
            }"
          >
            <img
              :src="awards.image"
              alt="Award image"
              :style="{ width: '100%', height: 'auto' , display: 'block', objectFit: 'cover'}"
            >
          </CRow>
          <CRow class="m-3">
            <h2>Title: <span>{{ awards.title }}</span></h2>
          </CRow>
          <CRow class="m-3">
            <h6>Description: <span>{{ awards.description }}</span></h6>
          </CRow>
          <CRow class="m-3">
            <CInput
              v-model="note"
              placeholder="User comment"
            />
          </CRow>

          <CButton
            class="col-3 m-3"
            color="info"
            @click="goBack"
          >
            Back
          </CButton>
          <CButton
            class="col-3 m-3"
            @click="submitAward"
          >
            Collect
          </CButton>
          <CRow class="m-3">
            <p>Message: <span>{{ message }}</span></p>
          </CRow>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
import { getRequest, postRequest } from '@/utils/http';

export default {
  name: 'Award',
  beforeRouteEnter(to, from, next) {
    next((vm) => {
      vm.awardsOpened = from.fullPath.includes('awards');
    });
  },
  data() {
    return {
      awardsOpened: null,
      awards: [],
      message: '',
      note: '',
      form: {
        terms: false,
      },
    };
  },
  computed: {
    awardsData() {
      const { id } = this.$route.params;
      const award = this.awards.find((a, index) => index + 1 === id);
      const awardDetails = award ? Object.entries(award) : [
        ['id', 'Not found'],
      ];
      return awardDetails.map(([key, value]) => ({
        key,
        value,
      }));
    },
  },

  mounted() {
    getRequest(`/awards/${this.$route.params.id}`)
      .then((response) => {
        this.awards = response.data;
      });

    // POST /api/attempt/
    // {
    //       "user": {
    //         "username": "nedoder1",
    //         "id": 1
    //       },
    //       "challenge": {
    //         "title": "test",
    //         "id": 1,
    //         "coins": 3,
    //         "description": "ddjjd"
    //       },
    //       "file": null,
    //       "confirmed_by_admin": false
    //     },
  },
  methods: {
    goBack() {
      this.$router.push({
        path: '/dashboard/awards',
      });
    },

    submitAward() {
      console.log('clicked');

      const data = {
        user: localStorage.getItem('user-id'),
        awards: this.$route.params.id,
        user_note: this.note,
      };

      postRequest('/collectedawards', data)
        .then((response) => {
          console.log(response);
          alert('You collected the award!');
          this.$router.push({
            path: '/dashboard/awards',
          });
        }).catch((error) => {
          console.log(error.response.data.message);
          this.message = error.response.data.message;
        });
    },
  },

};
</script>

<style scoped>

button {
  color: white;
  border: none;
  width: calc(100% - 30px);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  background: #ccc;
  cursor: pointer;
  box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.3);
  transition: 0.25s all ease;
}

.active {
  background: #5968d7;
}

pre-content {
  width: 500px;
}
</style>
