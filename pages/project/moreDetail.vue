<template>
  <v-card color="basil">
    <v-card-title class="py-6" style="flex-wrap: wrap">
      <div v-if="dataItem">
        <span v-model="dataItem.ProjectName" class="font-weight-light display-1 basil--text">
<!--        {{ dataItem ? dataItem.ProjectName : '' }}-->
          Project Name : {{ dataItem.ProjectName }}
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
      <h2>Git</h2>
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
      <h2 class="mt-3">Environment</h2>
      <v-card v-for="env in dataItem.env" class="mt-2">
        <v-container>

          <div>
            <strong class="pt-1" v-if="!isEditing">Env Name : {{ env.name }}</strong>
            <v-text-field
              v-if="isEditing"
              v-model="env.name"
              label="Name"
            ></v-text-field>
          </div>
          <strong v-if="!isEditing"> Key :{{ env.itemEnv ? env.itemEnv.key : 'none' }}</strong>
<!--          <div v-for="n in dataItem.env">-->
<!--            <p>testKey :{{n.key}}</p>-->
<!--&lt;!&ndash;            <p>{{env.itemEnv.key}}</p>&ndash;&gt;-->
<!--          </div>-->
<!--          env.itemEnv.key-->

          <v-spacer></v-spacer>
<!--          <v-text-field-->
<!--            v-if="isEditing && env.itemEnv"-->
<!--            v-model="env.itemEnv.key"-->
<!--            label="key"-->
<!--          ></v-text-field>-->
<!--          <strong v-if="!isEditing"> Value :{{ env.itemEnv ? env.itemEnv.value : '' }}</strong>-->
<!--          <v-text-field-->
<!--            v-if="isEditing && env.itemEnv"-->
<!--            v-model="env.itemEnv.value"-->
<!--            label="value"-->
<!--          ></v-text-field>-->
          <div v-for="kv in env.itemEnv">
<!--            <p>{{kv}}</p>-->
            <strong v-if="!isEditing">key : {{kv.key}}</strong>
            <v-text-field
              v-if="isEditing"
              v-model="kv.key"
              label="value"
            ></v-text-field>
            <strong v-if="!isEditing">value : {{kv.value}}</strong>
            <v-text-field
              v-if="isEditing"
              v-model="kv.value"
              label="value"
            ></v-text-field>
          </div>
        </v-container>
      </v-card>
      <div align="center">
        <v-btn class="ma-4 " color="primary" @click="editAllItem" v-if="!cancleEditing">
          <v-icon left>mdi-pencil</v-icon>
          แก้ไข
        </v-btn>
        <v-btn class="ma-4" color="error" v-if="cancleEditing" @click="cancleEdit">ยกเลิก</v-btn>

        <!--        save ควรอยู่ปุ่มเดียวกับแก้ไข-->
        <v-btn class="ma-4" color="primary" @click="save">
          <v-icon left>mdi-content-save</v-icon>
          บันทึก
        </v-btn>
        <v-btn color="primary" class="ma-4 " @click="openEnvDialog">
          <v-icon left>mdi-server-plus</v-icon>
          เพิ่ม Env
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
                    label="Env Name"
                  ></v-text-field>
                </v-col>
                <v-col cols="6">
                  <v-text-field
                    label="Env Key"
                  ></v-text-field>
                </v-col>
                <v-col cols="6">
                  <v-text-field
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
                @click="dialogAddEnv = false"
              >
                ยกเลิก
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
<!--      <div>-->
<!--        <v-btn @click="addBlock">Add block</v-btn>-->
<!--      </div>-->
<!--      <v-card>-->
<!--        <v-container>-->
<!--          <v-row>-->
<!--            <v-col cols="12">-->
<!--              <v-text-field-->
<!--                label="Env Name"-->
<!--              ></v-text-field>-->
<!--            </v-col>-->
<!--            <v-col cols="6">-->
<!--              <v-text-field-->
<!--                label="Env Key"-->
<!--              ></v-text-field>-->
<!--            </v-col>-->
<!--            <v-col cols="6">-->
<!--              <v-text-field-->
<!--                label="Env value"-->
<!--              ></v-text-field>-->
<!--            </v-col>-->
<!--          </v-row>-->
<!--        </v-container>-->

<!--        <v-card-actions>-->
<!--          <v-spacer></v-spacer>-->
<!--          <v-btn-->
<!--            color="success"-->
<!--            @click="addNewEnv"-->
<!--          >-->
<!--            ตกลง-->
<!--            &lt;!&ndash;                call patch API&ndash;&gt;-->
<!--          </v-btn>-->
<!--          <v-btn-->
<!--            color="error"-->
<!--            @click="dialogAddEnv = false"-->
<!--          >-->
<!--            ยกเลิก-->
<!--          </v-btn>-->
<!--        </v-card-actions>-->
<!--      </v-card>-->
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
      dialogAddEnv: false,
      newItem: [],
      newEnv :[{
        env : [{
          name : '',
          itemEnv : [{
            key : '',
            value : '',
          }]
        }]
      }]
    }
  },
  mounted() {
    this.selectedCardId = this.$route.query.id
    this.fetch()
    // console.log(this.selectedCard)
  },

  methods: {
    // addListCredit() {
    //   this.tempModelCredit = {
    //     ...this.tempModelCredit,
    //     id: this.modelCredit.length + 1,
    //   }
    //   this.modelCredit.push(this.tempModelCredit)
    //
    //   this.tempModelCredit = {
    //     id: null,
    //     imageId: null,
    //     imageUrl: null,
    //     url: null,
    //   }
    // },
    // addBlock() {
    //   this.addData.push(haveData)
    // },
    addNewEnv() {
      //มันเปลี่ยน ค่า Key and Value เลย  ไม่ได้ Add
      this.$axios.patch(`http://localhost:80/api/edit/alldata/${this.selectedCardId}`,
        {env: this.dataItem.env,})
        .then(({data}) => {
          if(this.newEnv){
            Object.assign(this.dataItem,this.newEnv)
            //ถ้ามี array อยุ่แล้ว ให้เพื่มเข้าไป
            this.dataItem = data
          }else{

            this.dataItem.push(this.newEnv)
          }

          console.log(data)
          console.log(this.dataItem.env)
          console.log(this.dataItem)

          //มี Env อยู่แล้ว อยากจะเพิ่มเข้าไป
          //env : []
          //inside env : [name,itemEnv]
          //inside itemEnv : [{key,value}]

          //Env ใน Database = []
          //API ที่รับค่า Env = รับเหมือนการรับ Data ทั่วไป
          // this.dataItem.push(this.dataItem.env)
          // console.log('then')
          this.dialogAddEnv = false
        })
        .finally((data) => {
          console.log(this.dataItem)
          this.fetch()
        })
    },
    openEnvDialog() {
      this.dialogAddEnv = true
      //  open dialog
    },
    cancleEdit() {
      this.cancleEditing = false
      this.isEditing = false
    },
    save() {
      // if(this.isEditing = true){
      console.log(this.dataItem)
      this.$axios.patch(`http://localhost:80/api/edit/alldata/${this.selectedCardId}`,
        {
          ProjectName: this.dataItem.projectName,
          gitUrl: this.dataItem.gitUrl,
          description: this.dataItem.description,
          remoteName: this.dataItem.remoteName,
          personInCharge: this.dataItem.personInCharge,
          env: this.dataItem.env,

          // env:{
          //   name:this.dataItem.env.name,
          //   itemEnv: {
          //     key : this.dataItem.env[0].itemEnv.key,
          //     value : this.dataItem.env.itemEnv.value,
          //   }
          // },
        }
      ).then(({data}) => {
        this.dataItem = data
        this.isEditing = false
        this.cancleEditing = false
        // this.isCancle = true
      }).finally((data) => {
        this.fetch()
      })
    },
    editAllItem() {
      this.cancleEditing = true
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
