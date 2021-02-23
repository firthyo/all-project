<template>
  <v-card color="basil">
    <v-card-title class="py-6" style="flex-wrap: wrap">
      <div>
        <span class="font-weight-bold display-1 basil--text">
        {{dataItem.ProjectName}}
      </span>
      </div>
      <div style="flex-basis: 100%"></div>
      <div>
        <h4 class="font-weight-light">
          Detail : {{dataItem.description}}
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
                  label="Environment Name"
                ></v-text-field>
                <strong v-if="!isEditing">Remote Name : {{ dataItem.remoteName }} </strong>
              </div>
              <div>
                <strong>Git url :</strong>
                <v-text-field
                  v-if="isEditing"
                  :value="dataItem.gitUrl"
                  label="Environment Name"
                ></v-text-field>
                <v-chip
                  class="ma-2"
                  color="primary"
                  outlined
                  v-if="!isEditing"
                >
                  <v-icon left >
                    mdi-git
                  </v-icon>

                  <a href="#"  v-if="!isEditing">{{ dataItem.gitUrl }}</a>

                </v-chip>

              </div>
              <div>
                <strong> Person in charge :</strong>
                <v-chip
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
              {{dataItem}}
            </v-container>
          </v-card>
      <v-card class="mt-2">
        <v-container>
          <h2>Environment</h2>
        </v-container>

      </v-card>
          <v-card  v-for="env in dataItem.env"  class="mt-2">
            ppppp
            {{env}}
            <v-container>
              <h2>Environment</h2>
              <div>
                <strong  v-if="!isEditing">Environment Name :  {{env.name}}</strong>
                <v-text-field
                  v-if="isEditing"
                  :value="env.name"
                  label="Environment Name"
                ></v-text-field>
              </div>
              <div v-for="kv in env.itemEnv">
<!--                <v-text-field label="Key" v-model="dataItem.key" v-if="dataItem.isEditing"></v-text-field>-->
<!--                <strong v-if="dataItem.isEditing">Hello Editing</strong>-->
                <strong >Key : </strong> {{kv.key}} || <strong>Value : </strong> {{kv.value}}

              </div>

            </v-container>

          </v-card>

      <div align="center" >
        <v-btn class="ma-4" color="primary" @click="editAllItem">แก้ไข</v-btn>
<!--        save ควรอยู่ปุ่มเดียวกับแก้ไข-->
        <v-btn class="ma-4" color="primary" >Save</v-btn>
      </div>
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
      dataItem : [],
      isEditing : false,
    }
  },
  mounted() {
    this.selectedCardId = this.$route.query.id
    this.fetch()
    // console.log(this.selectedCard)
  },

  methods:{
    editAllItem(){
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
    fetch(){
      // this.$axios.$get('localhost/api/alldata').then((dataResult)=>{
      this.$axios.get(`http://localhost:80/api/alldata/${this.selectedCardId}`).then(({ data })=>{
        this.dataItem = data
        console.log(data)
      }).catch((err)=>{
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
