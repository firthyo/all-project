<template>
  <v-card color="basil">
    <v-card-title class="py-6" style="flex-wrap: wrap">
      <div v-if="dataItem">
        <span v-model="dataItem.ProjectName" class="font-weight-light display-1 basil--text">
<!--        {{ dataItem ? dataItem.ProjectName : '' }}-->
          Project Name : {{dataItem.ProjectName}}
<!--          {{dataItem.projectName}}-->
<!--           <v-text-field-->
<!--             v-if="isEditing"-->
<!--             v-model="dataItem.ProjectName"-->
<!--             label="Project Name"-->
<!--           ></v-text-field>-->
      </span>
      </div>
      <div style="flex-basis: 100%"></div>
      <div>
        <h4 class="font-weight-light">
          Detail : {{ dataItem ? dataItem.description : '' }}
        </h4>
      </div>
    </v-card-title>
    <v-container v-if="dataItem">
      <v-card>
        <v-container>
          <h2>Git</h2>
          <div>
            <!--              :value="dataItem.remoteName"-->
            <v-text-field
              v-if="isEditing"
              v-model="dataItem.remoteName"
              label="Git Remotename"
            ></v-text-field>
            <strong v-if="!isEditing">Remote Name : {{ dataItem.remoteName }} </strong>
          </div>
          <div>
            <strong>Git url :</strong>
            <v-text-field
              v-if="isEditing"
              v-model="dataItem.gitUrl"
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
                :items="teamMember"
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
        </v-container>
      </v-card>
      <v-card v-for="env in dataItem.env" class="mt-2">
        <v-container>
          <h2>Environment</h2>
          <div>
            <strong class="pt-1" v-if="!isEditing">Env Name : {{ env.name }}</strong>
            <v-text-field
              v-if="isEditing"
              v-model="env.name"
              label="Name"
            ></v-text-field>

          </div>
          <strong v-if="!isEditing"> Key :{{ env.itemEnv.key }}</strong>
          <v-spacer></v-spacer>
          <v-text-field
            v-if="isEditing"
            v-model="env.itemEnv.key"
            label="key"
          ></v-text-field>
          <strong v-if="!isEditing"> Value :{{ env.itemEnv.value }}</strong>
          <v-text-field
            v-if="isEditing"
            :value="env.itemEnv.value"
            label="value"
          ></v-text-field>
        </v-container>
      </v-card>
      <div align="center">
        <v-btn class="ma-4 " color="primary" @click="editAllItem" v-if="!cancleEditing">แก้ไข</v-btn>
        <v-btn class="ma-4" color="error" v-if="cancleEditing" @click="cancleEdit">ยกเลิก</v-btn>

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
      teamMember: ["firth", "Elon", "Bill", "Jeff", "Steve"],
      selectedCardId: null,
      tab: null,
      data: null,
      dataItem: null,
      isEditing: false,
      cancleEditing: false,
      //not cancle
      newItem: [],
    }
  },
  mounted() {
    this.selectedCardId = this.$route.query.id
    this.fetch()
    // console.log(this.selectedCard)
  },

  methods: {
    cancleEdit(){
      this.cancleEditing = false
      this.isEditing = false
    },
    save() {
      // if(this.isEditing = true){

      this.$axios.patch(`http://localhost:80/api/edit/alldata/${this.selectedCardId}`,
        {
          ProjectName: this.dataItem.projectName,
          gitUrl: this.dataItem.gitUrl,
          description: this.dataItem.description,
          remoteName: this.dataItem.remoteName,


          personInCharge: this.dataItem.personInCharge,
          env :this.env,
          // env:{
          //   name:this.dataItem.env.name,
          //   itemEnv: {
          //     key : this.dataItem.env.key,
          //     value : this.dataItem.env.value,
          //   }
          // }
        }
      ).then(({data}) => {
        console.log(data)
        this.dataItem = data
        this.isEditing = false
        this.cancleEditing = false
        // this.isCancle = true
      }).finally((data) => {
        console.log(data)
        this.fetch()
      })
    },
    editAllItem() {
      this.cancleEditing =true
      this.$axios.get(`http://localhost:80/api/alldata/`)
        .then(({data}) => {
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
