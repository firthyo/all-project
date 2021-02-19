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
        <v-tab
          v-for="item in tabHeader"
        >
          <h2 class="">{{ item }}</h2>
        </v-tab>
      </v-tabs>
      <v-tabs-items v-model="tab">
        <v-tab-item
          v-for="alldata in dataItems"
        >
          <v-card
            class="ma-3"
            color="basil"
            flat
          >
            <div>
              <strong>Remote Name :</strong>{{ alldata.remoteName }}
            </div>
            <div>
              <strong>Git url :</strong>{{ alldata.gitUrl }}
            </div>
            <div>
              <strong> Person in charge :</strong>
              <v-chip
                class="ma-1"
                color="indigo"
                text-color="white"
                v-for="member in alldata.personInCharge"
              >
                <v-avatar left>
                  <v-icon>mdi-account-circle</v-icon>
                </v-avatar>
                <!--                  {{member}}-->
                {{ member }}
                <!--                  {{alldata.personInCharge}}-->
              </v-chip>
            </div>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card v-for="envData in dataItems">
            <v-card-title v-for="item in envData.env">
              <v-card-text>
                <p> Name : {{ item.envname }}</p>
                <span v-for="x in item.envItem">
               <p>Key: {{ x.key }}, Value: {{ x.value }}</p>
                 </span>
                <v-divider></v-divider>
              </v-card-text>

            </v-card-title>
<!--            this.$router.push({ path: '/apps/system/banner/groupDetail', query: { groupId: item.id } })-->
            <!--            <div v-for="env in envData.env">-->
            <!--              <strong>Environment Name</strong> {{ env.envname }}-->
            <!--            </div>-->
            <!--            <div v-for="env in envData.env">-->
            <!--              <div v-for="envItem in env.envItem">-->
            <!--                <strong>Key : </strong> {{ envItem.key }}-->
            <!--              </div>-->
            <!--            </div>-->
            <!--            <div v-for="env in envData.env">-->
            <!--              <div v-for="envValue  in env.envItem">-->
            <!--                <strong>Value</strong> {{ envValue.value }}-->
            <!--              </div>-->
            <!--            </div>-->
          </v-card>
        </v-tab-item>
        <v-tabs-items v-for="(sss,index) in data" :key="index">
<!--          <h3>{{sss.remoteName}}</h3>-->
<!--          {{data}}-->
        </v-tabs-items>
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
      data:null,
      tabHeader: [
        'Git', 'Environment','test'
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
        //   {
        //   id:'2',
        //   remoteName: " zipzoft / system",
        //   gitUrl: 'https://github.com/zipzoft/system',
        //   personInCharge: ["mozart", "beethoven", "chopin", "maler"],
        //   detail: "A musician ",
        //   env: [
        //     {
        //       envname: "Staging",
        //       envItem : [{
        //         key: "III",
        //         value: "AWS",
        //       },
        //         {
        //           key: "BBB",
        //           value: "AAPL",
        //         }
        //       ],
        //     },
        //     {
        //       envname: "Project 51",
        //       key: "AAA",
        //       value: "1234 B NYC",
        //     }
        //   ]
        // },
        //   {
        //   id:'3',
        //   remoteName: " zipzoft / menu",
        //   gitUrl: 'https://github.com/zipzoft/menu',
        //   personInCharge: ["liza", "jenny", "jisoo", "rose"],
        //   detail: "BlackPink in your areaaaa",
        //   env: [
        //     {
        //       envname: "YG",
        //       envItem : [{
        //         key: "KR POP",
        //         value: "Kill this love",
        //       },
        //         {
        //           key: "KR POP2",
        //           value: "Du Duu Du",
        //         }
        //
        //       ],
        //
        //     },
        //     {
        //       envname: "Project 51",
        //       key: "AAA",
        //       value: "1234 B NYC",
        //     }
        //   ]
        // }
      ]
    }
  },
  mounted() {
    this.selectedCardId = this.$route.query.id
    console.log(this.$route.query.id)
    this.fetch()
    // console.log(this.selectedCard)
  },
  methods:{
    fetch(data){
      this.selectedCardId =  this.$route.query.id
      //call api (path +  this.selectedCard).then(receivedFromApi)
      const receivedFromApi = {
          id:'2',
          remoteName: " zipzoft / system",
          gitUrl: 'https://github.com/zipzoft/system',
          personInCharge: ["mozart", "beethoven", "chopin", "maler"],
          detail: "A musician ",
          env: [
            {
              envname: "Staging",
              envItem : [{
                key: "III",
                value: "AWS",
              },
                {
                  key: "BBB",
                  value: "AAPL",
                }
              ],
            },
            {
              envname: "Project 51",
              key: "AAA",
              value: "1234 B NYC",
            }
          ]
      }
      this.data = receivedFromApi

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
