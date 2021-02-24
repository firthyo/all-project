<template>
  <v-card color="basil">
    <v-card-title class="py-6" style="flex-wrap: wrap">
      <div>
        <span class="font-weight-bold display-1 basil--text">
        {{ dataItem.ProjectName }}
      </span>
      </div>
      <div style="flex-basis: 100%"></div>
      <div>
        <h4 class="font-weight-light">
          Detail : {{ dataItem.description }}
        </h4>
      </div>
    </v-card-title>
    <v-container>
      <v-card
        v-model="dataItem"
      >
        <v-container>
          <h2>Git</h2>
          <div>
            <v-text-field
              v-if="isEditing"
              :value="dataItem.remoteName"
              label="Git Remotename"
            ></v-text-field>
            <strong v-if="!isEditing">Remote Name : {{ dataItem.remoteName }} </strong>
          </div>
          <div>
            <strong>Git url :</strong>
            <v-text-field
              v-if="isEditing"
              :value="dataItem.gitUrl"
            ></v-text-field>
            <v-chip
              class="ma-2"
              color="primary"
              outlined
              v-if="!isEditing"
            >
              <v-icon left>
                mdi-git
              </v-icon>

              <a href="#" v-if="!isEditing">{{ dataItem.gitUrl }}</a>

            </v-chip>

          </div>
          <div>
            <strong> Person in charge :</strong>
            <div>
              <v-autocomplete
                v-if="isEditing"
                v-model="dataItem.personInCharge"
                :items="dataItem.personInCharge"
                attach
                chips
                :menu-props="{ top: true, offsetY: true }"
                label="Person in charge"
                multiple
              ></v-autocomplete>
            </div>
            <v-chip
              v-if="!isEditing"
              class="ma-1"
              color="indigo"
              text-color="white"
              v-for="member in dataItem.personInCharge"
            >
              <v-avatar left>
                <v-icon>mdi-account-circle</v-icon>
              </v-avatar>
              <!--                  {{member}}-->
              {{ member }}
              <!--                  {{alldata.personInCharge}}-->


            </v-chip>
          </div>
          {{ dataItem }}
        </v-container>
      </v-card>
      <v-card v-for="env in dataItem.env" class="mt-2">
        {{ env }}
        <v-container>
          <h2>Environment</h2>
          <div>
            <strong v-if="!isEditing">Environment Name : {{ env.name }}</strong>
            <v-text-field
              v-if="isEditing"
              :value="env.name"
              label="Environment Name"
            ></v-text-field>
          </div>
<!--          <div>-->

<!--&lt;!&ndash;            <v-text-field v-if="isEditing"&ndash;&gt;-->
<!--&lt;!&ndash;                          :value="kv"&ndash;&gt;-->
<!--&lt;!&ndash;                          label="Key"&ndash;&gt;-->
<!--&lt;!&ndash;                          v-model="dataItem.key"></v-text-field>&ndash;&gt;-->
<!--&lt;!&ndash;            <strong v-if="!isEditing">Key : {{kv.key}} </strong> || <strong>Value : </strong> {{ kv.value }}&ndash;&gt;-->
<!--&lt;!&ndash;            {{kv}}&ndash;&gt;-->

<!--          </div>-->
              <span>xxxxxxxx</span>
          <span> This is KEY :{{ env.itemEnv.key}}</span>
          <span> This is VALUE :{{ env.itemEnv.value}}</span>

        </v-container>

      </v-card>

      <div align="center">
        <v-btn class="ma-4" color="primary" @click="editAllItem">แก้ไข</v-btn>
        <!--        save ควรอยู่ปุ่มเดียวกับแก้ไข-->
        <v-btn class="ma-4" color="primary" @click="save">บันทึก</v-btn>
      </div>
    </v-container>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      selectedCardId: null,
      tab: null,
      data: null,
      dataItem: [],
      isEditing: false,
      newItem: [],
    }
  },
  mounted() {
    this.selectedCardId = this.$route.query.id
    this.fetch()
    // console.log(this.selectedCard)
  },

  methods: {
    save() {
      // if(this.isEditing = true){
      //   this.$axios.patch(`http://localhost:80/api/edit/alldata/${this.selectedCardId}`)
      //     .then(({data}) => {
      //       this.dataItem = data
      //     })
      // }
    },
    editAllItem() {
      this.$axios.get(`http://localhost:80/api/alldata/`)
        .then(({data})=>{

        })
      this.isEditing = true

    },
    // editingData(editdata){
    //   if(editdata.editingdata){
    //     this.isEditing = false
    //   }else{
    //     this.isEditing =true
    //   }
    //
    // },
    fetch() {
      // this.$axios.$get('localhost/api/alldata').then((dataResult)=>{
      this.$axios.get(`http://localhost:80/api/alldata/${this.selectedCardId}`).then(({data}) => {
        this.dataItem = data
       // console.log(data)
      }).catch((err) => {
        (err)
      })
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
