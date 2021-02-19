<template>
  <v-card color="basil">
    <v-card-title class="text-center justify-center py-6">
      <h1 class="font-weight-bold display-1 basil--text">
        System True Wallets
      </h1>
    </v-card-title>
    <v-container>
      <v-tabs
        v-model="tab"
        background-color="transparent"
        color="basil"
        slider-color="primary"
        grow
      >
        <v-tab>
          <h2>Git</h2>
        </v-tab>
        <v-tab>
          <h2>Environment</h2>
        </v-tab>
      </v-tabs>
      <v-tabs-items v-model="tab">
        <v-tab-item>
          <span v-if="data">{{ data.data }}</span>
<!--          <v-card-->
<!--            class="ma-3"-->
<!--            color="basil"-->
<!--            flat-->

<!--            v-for="alldata in dataItems"-->

<!--          >-->
<!--            <div>-->
<!--              <strong>Remote Name :</strong>{{ alldata.remoteName }}-->
<!--            </div>-->
<!--            <div>-->
<!--              <strong>Git url :</strong>{{ alldata.gitUrl }}-->
<!--            </div>-->
<!--            <div>-->
<!--              <strong> Person in charge :</strong>-->
<!--              <v-chip-->
<!--                class="ma-1"-->
<!--                color="indigo"-->
<!--                text-color="white"-->
<!--                v-for="member in alldata.personInCharge"-->
<!--              >-->
<!--                &lt;!&ndash;                  {{member}}&ndash;&gt;-->
<!--                {{ member }}-->
<!--                &lt;!&ndash;                  {{alldata.personInCharge}}&ndash;&gt;-->
<!--              </v-chip>-->
<!--            </div>-->
<!--          </v-card>-->
        </v-tab-item>
        <v-tab-item >

        </v-tab-item>

<!--        <v-tab-item>-->
<!--          <v-card v-for="envData in dataItems">-->
<!--            dsfsdfds-->
<!--            <v-card-title v-for="item in envData.env">-->
<!--              <v-card-text>-->
<!--                <p> Name : {{ item.envname }}</p>-->
<!--                <span v-for="x in item.envItem">-->
<!--               <p>Key: {{ x.key }}, Value: {{ x.value }}</p>-->
<!--                 </span>-->
<!--                <v-divider></v-divider>-->
<!--              </v-card-text>-->

<!--            </v-card-title>-->
<!--            &lt;!&ndash;            this.$router.push({ path: '/apps/system/banner/groupDetail', query: { groupId: item.id } })&ndash;&gt;-->
<!--            &lt;!&ndash;            <div v-for="env in envData.env">&ndash;&gt;-->
<!--            &lt;!&ndash;              <strong>Environment Name</strong> {{ env.envname }}&ndash;&gt;-->
<!--            &lt;!&ndash;            </div>&ndash;&gt;-->
<!--            &lt;!&ndash;            <div v-for="env in envData.env">&ndash;&gt;-->
<!--            &lt;!&ndash;              <div v-for="envItem in env.envItem">&ndash;&gt;-->
<!--            &lt;!&ndash;                <strong>Key : </strong> {{ envItem.key }}&ndash;&gt;-->
<!--            &lt;!&ndash;              </div>&ndash;&gt;-->
<!--            &lt;!&ndash;            </div>&ndash;&gt;-->
<!--            &lt;!&ndash;            <div v-for="env in envData.env">&ndash;&gt;-->
<!--            &lt;!&ndash;              <div v-for="envValue  in env.envItem">&ndash;&gt;-->
<!--            &lt;!&ndash;                <strong>Value</strong> {{ envValue.value }}&ndash;&gt;-->
<!--            &lt;!&ndash;              </div>&ndash;&gt;-->
<!--            &lt;!&ndash;            </div>&ndash;&gt;-->
<!--          </v-card>-->
<!--        </v-tab-item>-->
<!--        <v-tabs-items v-for="dataProject in data">-->
<!--          {{dataProject.remoteName}}-->
<!--          <h3>hi</h3>-->
<!--        </v-tabs-items>-->
      </v-tabs-items>
    </v-container>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      selectedCardId:null,
      tab: null,
      data: null,
      tabHeader: [
        'Git', 'Environment','test','yyy'
      ],
      text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.',
      hey: 'projectsssssssssss',
      dataItems: [
        {
          id: '1',
          remoteName: " zipzoft / mepo",
          gitUrl: 'https://github.com/zipzoft/mepo',
          personInCharge: ["firth", "Elon", "Bill", "Steve"],
          detail: "A pilot from Grand Rapids is delighted when she gets the chance to take part in the final of a dancing competition.",
          env: [
            {
              id: '1',
              envname: "Staging",
              envItem:
                [{
                  id: '1',
                  key: "YYY",
                  value: "three NYC",
                },
                  {
                    id: '2',
                    key: "AAA",
                    value: "One Rolla",
                  }],
            },
            {
              id: '2',
              envname: "Dev",
              envItem: [{
                key: "A51",
                value: "1234 B NYC 51",
              }]
            }
          ]
        },
        {
          id: '2',
          remoteName: " zipzoft / mepo",
          gitUrl: 'https://github.com/zipzoft/mepo',
          personInCharge: ["firth", "Elon", "Bill", "Steve"],
          detail: "A pilot from Grand Rapids is delighted when she gets the chance to take part in the final of a dancing competition.",
          env: [
            {
              id: '1',
              envname: "Staging",
              envItem:
                [{
                  id: '1',
                  key: "YYY",
                  value: "three NYC",
                },
                  {
                    id: '2',
                    key: "AAA",
                    value: "One Rolla",
                  }],
            },
            {
              id: '2',
              envname: "Dev",
              envItem: [{
                key: "A51",
                value: "1234 B NYC 51",
              }]
            }
          ]
        },
      ]
    }
  },
  mounted() {
    console.log('1', this.data)
    this.selectedCardId = this.$route.query.id
    console.log('2',this.data)
    // console.log(this.$route.query.id)
    console.log('3',this.data)
    this.fetch()
    console.log('4',this.data)
    // console.log(this.selectedCard)
  },
  methods:{
    fetch(){
      console.log('6',this.data)
      // this.$axios.$get('localhost/api/alldata').then((dataResult)=>{
      this.$axios.get(`http://localhost:80/api/alldata/${this.selectedCardId}`).then((dataResult)=>{
        this.data = dataResult
        console.log('7',this.data)
      }).catch((err)=>{

      })
      console.log('8', this.data)
    }
  }
}
</script>
<style>
/* Helper classes */
/*.basil {*/
/*  background-color: #FFFBE6 !important;*/
/*}*/
/*.basil--text {*/
/*  color: #356859 !important;*/
/*}*/
</style>
