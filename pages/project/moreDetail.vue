<template>
  <v-card color="basil">
    <v-card-title class="py-6" style="flex-wrap: wrap">
      <div v-if="dataItem">
        <span v-model="dataItem.ProjectName" class="font-weight-light display-1 basil--text">
          Project Name : {{ dataItem.ProjectName }}
      </span>
        <div v-model="dataItem.description">
          <h4 class="font-weight-light">
            Detail : {{ dataItem ? dataItem.description : '' }}
          </h4>
        </div>
      </div>
      <div style="flex-basis: 100%"></div>
    </v-card-title>
    <v-container v-if="dataItem">
      <v-row >
        <v-col cols="6">
          <h2>Git</h2>
        </v-col>
        <v-col cols="6"  class="d-flex justify-end">
          <v-btn class="mb-2 " small color="warning" @click="editAllItem" v-if="!cancleEditing">
            <v-icon>mdi-pencil</v-icon>
          </v-btn>
        </v-col>
      </v-row>

      <v-card>
        <v-container>
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
              color="blue-grey darken-3"
              outlined
              v-if="!isEditing"
            >
              <v-icon left>
                mdi-git
              </v-icon>
              <a href="#" v-if="!isEditing" style="color: #35495e">{{ dataItem.gitUrl }}</a>
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
              color="blue-grey darken-3"
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
      <h2 class="mt-3">Environment</h2>
      <v-card v-for="env in dataItem.env" class="mt-2">
        <v-container>
          <div>
            <strong>
              Env Name : {{ env.name }}
            </strong>
            <v-btn color="error" small class="ma-1" @click="openDeleteWholeEnv">
              <v-icon>
                mdi-delete
              </v-icon>
            </v-btn>
            <v-dialog
              v-for="wholeEnv in dataItem.env"
              max-width="290"
              v-model="isOpenDeleteEnv"
            >
              <v-card>
                <v-container>
                  <p>ยืนยันที่จะลบ <strong>{{ wholeEnv.name }}</strong> นี้</p>
                  <v-btn @click="deleteWholeEnv(env)" color="success">
                    ตกลง
                  </v-btn>
                  <v-btn color="error" @click="closeDeleteWholeEnv">
                    ยกเลิก
                  </v-btn>
                </v-container>

              </v-card>

            </v-dialog>

            <!--            <strong class="pt-1" v-if="!isEditing">Env Name : {{ env.name }}</strong>-->
            <!--            {{env}}-->
            <v-text-field
              v-if="isEditing"
              v-model="env.name"
              label="Name"
            ></v-text-field>
            <div>

              <v-dialog
                max-width="490"
                v-model="isOpenAddkvDialog"
              >
                <v-card>
                  <v-container>
                    <p>

                       Key และ Value
                    </p>
                    <v-row>
                      <v-col cols="6">
                        <v-text-field
                          v-model="addMoreEnvKey"
                          label="key"
                        >

                        </v-text-field>
                      </v-col>
                      <v-col cols="6">
                        <v-text-field
                          v-model="addMoreEnvVal"
                          label="value"
                        >
                        </v-text-field>
                      </v-col>
                    </v-row>
                  </v-container>
                  <v-btn @click="AddEnvKv(env)">ตกลงเพิ่ม</v-btn>
                </v-card>
              </v-dialog>
            </div>
          </div>
          <!--          {{env.itemEnv}}-->
          <!--          {{env.itemEnv.key}}-->
          <v-divider class="mb-2"></v-divider>

          <div class="d-flex justify-start align-center">
            <v-row>
              <v-col cols="6">
                <strong>
                  Key / Value
                </strong>
              </v-col>
            </v-row>
            <v-col cols="6" class="d-flex justify-end">
              <v-btn @click="openAddKvDialog" small color="primary ">เพิ่ม Key/Value</v-btn>
            </v-col>

          </div>
          <div v-for="kv in env.itemEnv" class="d-flex mt-2">
            <div style="flex: 1 1;">
              <span v-if="!isEditing">Key : <strong>{{ kv.key }}</strong></span>
              <v-text-field
                v-if="isEditing"
                v-model="kv.key"
                outlined
                label="key"
              ></v-text-field>
            </div>
            <div style="flex: 2 1;" class="mx-2">
              <span v-if="!isEditing">Value : <strong>{{ kv.value }}</strong></span>
              <v-text-field
                v-if="isEditing"
                v-model="kv.value"
                outlined
                label="val"
              ></v-text-field>
            </div>
            <div class="d-flex">
              <v-btn class="mb-3" small color="error" @click="openDeleteKvdialog">
                <v-icon>
                  mdi-delete
                </v-icon>
              </v-btn>
              <v-dialog
                max-width="390"
                v-model="isOpenDeleteKvdialog"
                v-for="envElement in dataItem.env"
              >
                <v-card>
                  <v-container>
                    <p>ยืนยันที่จะลบ Key และ Value ของ <strong>{{ envElement.name }}</strong></p>

                    <v-btn
                      @click="deleteEnvKv(env, kv)"
                      color="success">
                      ยืนยัน
                    </v-btn>
                    <v-btn
                      color="error"
                      @click="closeDeleteKvdialog"
                    >
                      ยกเลิก
                    </v-btn>
                  </v-container>
                </v-card>
              </v-dialog>
            </div>
          </div>
          <v-spacer></v-spacer>
        </v-container>
      </v-card>
      <div align="center">
        <v-btn v-if="!cancleEditing" color="primary" class="ma-4 " @click="openEnvDialog">
          <v-icon left>mdi-server-plus</v-icon>
          เพิ่ม Env
        </v-btn>


        <v-btn class="ma-2" color="error" v-if="cancleEditing" @click="cancleEdit">ยกเลิก</v-btn>
        <v-btn v-if="isEditing" class="ma-4" color="primary" @click="save">
          <v-icon left>mdi-content-save</v-icon>
          บันทึก
        </v-btn>
        <v-dialog
          v-model="dialogAddEnv"
          persistent
          max-width="400"
        >
          <v-card>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    v-model="addMoreEnvName"
                    label="Env Name"
                  ></v-text-field>

                </v-col>
                <v-col cols="6">
                  <v-text-field
                    v-model="addMoreEnvKey"
                    label="Env Key"
                  ></v-text-field>
                </v-col>
                <v-col cols="6">
                  <v-text-field
                    v-model="addMoreEnvVal"
                    label="Env value"
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="success"
                @click="addNewEnv"
              >
                ตกลง
                <!--                call patch API-->
              </v-btn>
              <v-btn
                color="error"
                @click="cancelAddEnvDialog"
              >
                ยกเลิก
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
    </v-container>
  </v-card>

</template>
<script>
import datadetail from "@/pages/datadetail";

export default {
  data() {
    return {
      testDelete: false,
      addMoreEnvName: '',
      addMoreEnvKey: '',
      addMoreEnvVal: '',
      teamMember: ["firth", "Elon", "Bill", "Jeff", "Steve"],
      selectedCardId: null,
      envId: null,
      isOpenAddkvDialog: false,
      tab: null,
      data: null,
      dataItem: null,
      isEditing: false,
      cancleEditing: false,
      //not cancle
      dialogAddEnv: false,
      newItem: [],
      newEnvItem: [],
      isOpenDeleteKvdialog: false,
      isOpenDeleteEnv: false,

      // newEnv :[{
      //   env : [{
      //     name : '',
      //     itemEnv : [{
      //       key : '',
      //       value : '',
      //     }]
      //   }]
      // }]
    }
  },
  mounted() {

    this.selectedCardId = this.$route.query.id
    this.fetch()
    // this.envID = this.$route.query.id
    // console.log(this.selectedCard)
  },

  methods: {

    closeDeleteWholeEnv() {
      this.isOpenDeleteEnv = false
    },
    openDeleteWholeEnv() {
      this.isOpenDeleteEnv = true
    },
    openDeleteKvdialog() {
      this.isOpenDeleteKvdialog = true

    },
    closeDeleteKvdialog() {
      this.isOpenDeleteKvdialog = false
    },

    deleteWholeEnv(wholeEnv) {
      let envData = this.dataItem.env;

      let index = envData.findIndex(({_id}) => {
        return _id === wholeEnv._id
      })
      envData.splice(index, 1)

      this.$axios.patch(`http://localhost:80/api/edit/alldata/${this.selectedCardId}`,
        {
          env: envData,
        }
      ).then(({data}) => {
        this.dataItem = data
        // this.isCancle = true
        this.isOpenDeleteEnv = true
      }).finally((data) => {
        this.fetch()
      })
    },
    openAddKvDialog() {
      this.isOpenAddkvDialog = true
    },
    AddEnvKv(env) {
      this.$axios.patch(`http://localhost:80/api/env/keyval/${env._id}`,
        {
          key: this.addMoreEnvKey,
          value: this.addMoreEnvVal
        })
        .then((data) => {
          // this.dataItem = data
          this.isOpenAddkvDialog = false
          console.log('this is (success) ', data)
        }).finally(() => {
        this.fetch()
      })
    },
    deleteEnvKv(enviroment, itemEnv) {
      let envData = this.dataItem.env.map(env => {
        if (env._id === enviroment._id) {
          let index = env.itemEnv.findIndex(({_id}) => {
            return _id === itemEnv._id
          })
          env.itemEnv.splice(index, 1)
        }
        return env
      })
      this.$axios.patch(`http://localhost:80/api/edit/alldata/${this.selectedCardId}`,
        {
          env: envData,
        }
      ).then(({data}) => {
        this.dataItem = data
        // this.isCancle = true
      }).finally((data) => {
        this.fetch()
      })
    }
    ,

    clearDialogEnvData() {
      this.addMoreEnvName = []
      this.addMoreEnvKey = []
      this.addMoreEnvVal = []
    }
    ,
    cancelAddEnvDialog() {
      this.dialogAddEnv = false
      this.clearDialogEnvData()
    }
    ,
    addNewEnv() {
      //มันเปลี่ยน ค่า Key and Value เลย  ไม่ได้ Add
      // console.log('--->', this.dataItem.env,this.addMoreEnvName,this.addMoreEnvKey,this.addMoreEnvVal)
      this.$axios.post(`http://localhost:80/api/add/env/${this.selectedCardId}`,
        {
          env: [{
            name: this.addMoreEnvName,
            itemEnv: [{
              key: this.addMoreEnvKey,
              value: this.addMoreEnvVal
            }]
          }]
          // env: [this.addMoreEnvName,this.addMoreEnvKey,this.addMoreEnvVal]
        }
      ).then(({data}) => {
        this.dataItem = data
        this.clearDialogEnvData()
        // console.log(this.dataItem.env)
        // console.log(data)
        this.dialogAddEnv = false

      }).finally((data) => {
        console.log(this.dataItem)
        this.fetch()
      })
    }
    ,
    openEnvDialog() {
      this.dialogAddEnv = true
      //  open dialog
    }
    ,
    cancleEdit() {
      this.cancleEditing = false
      this.isEditing = false

    }
    ,
    save() {
      // if(this.isEditing = true){
      // console.log(this.dataItem)
      console.log('สวัสดี', this.dataItem.env[0].itemEnv)
      this.$axios.patch(`http://localhost:80/api/edit/alldata/${this.selectedCardId}`,
        {
          ProjectName: this.dataItem.projectName,
          gitUrl: this.dataItem.gitUrl,
          description: this.dataItem.description,
          remoteName: this.dataItem.remoteName,
          personInCharge: this.dataItem.personInCharge,
          env: this.dataItem.env,
        }
      ).then(({data}) => {
        this.dataItem = data
        this.isEditing = false
        this.cancleEditing = false
        // this.isCancle = true
      }).finally((data) => {
        this.fetch()
      })
    }
    ,
    editAllItem() {
      this.cancleEditing = true
      this.$axios.get(`http://localhost:80/api/alldata/`)
        .then(({data}) => {
          // this.dataItem = data
        })
      this.isEditing = true

    }
    ,
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
