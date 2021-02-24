<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <template>
        <v-container fluid>
          <v-data-iterator
            :items="items"
            :items-per-page.sync="itemsPerPage"
            :page.sync="page"
            :search="search"
            :sort-by="sortBy.toLowerCase()"
            :sort-desc="sortDesc"
            hide-default-footer
          >
            <template v-slot:header>
              <v-toolbar
                dark
                color="black"
                class="mb-1"
              >
                <v-text-field
                  v-model="search"
                  clearable
                  flat
                  solo-inverted
                  hide-details
                  prepend-inner-icon="mdi-magnify"
                  label="Search"
                ></v-text-field>
                <template v-if="$vuetify.breakpoint.mdAndUp">
                  <v-spacer></v-spacer>
                  <v-select
                    v-model="sortBy"
                    flat
                    solo-inverted
                    hide-details
                    :items="keys"
                    prepend-inner-icon="mdi-magnify"
                    label="Sort by"
                  ></v-select>
                  <v-spacer></v-spacer>
                  <v-select
                    v-model="sortBy"
                    flat
                    solo-inverted
                    hide-details
                    :items="keys"
                    prepend-inner-icon="mdi-magnify"
                    label="Sort by"
                  ></v-select>

                  <v-spacer></v-spacer>
                  <v-btn-toggle
                    v-model="sortDesc"
                    mandatory
                  >
                    <v-btn
                      large
                      depressed
                      color="blue"
                      :value="false"
                    >
                      <v-icon>mdi-arrow-up</v-icon>
                    </v-btn>
                    <v-btn
                      large
                      depressed
                      color="blue"
                      :value="true"
                    >
                      <v-icon>mdi-arrow-down</v-icon>
                    </v-btn>
                  </v-btn-toggle>
                </template>
              </v-toolbar>
            </template>

            <template v-slot:default="props">
              <v-row>
                <v-col
                  style="justify-items: center; display: flex; justify-content: center"
                  cols="12"
                  sm="6"
                  md="4"
                  lg="3">
                  <v-card-title class="subheading font-weight-bold">
                    <template>
                      <v-row justify="center">
                        <!--*********************-->
                        <!-- ADD-->
                        <!--*********************-->
                        <v-btn
                          color="success"
                          class="white--text ma-1"
                          @click.stop="dialogAdd = true"
                        >
                          เพิ่ม
                        </v-btn>
                        <v-dialog
                          v-model="dialogAdd"
                          max-width="600"
                        >
                          <v-card>
                            <v-card-title>
                              <span class="headline">New Project</span>
                            </v-card-title>
                            <v-card-text>
                              <v-container>
                                <v-row>
                                  <v-col
                                    cols="12"
                                  >
                                    <v-text-field
                                      v-model="projectName"
                                      label="Project Name"
                                      prepend-icon="mdi-folder"
                                      required
                                    ></v-text-field>
                                  </v-col>
                                  <v-col
                                    cols="12"
                                  >
                                    <v-text-field
                                      v-model="description"
                                      label="Description"
                                      prepend-icon="mdi-note-text"
                                      required
                                    ></v-text-field>
                                  </v-col>
                                  <v-col cols="12">
                                    <v-text-field
                                      v-model="remoteName"
                                      label="Git Repository Name"
                                      prepend-icon="mdi-git"
                                      required
                                    ></v-text-field>
                                  </v-col>
                                  <v-col cols="12">
                                    <v-text-field
                                      v-model="gitUrl"
                                      label="Git Url"
                                      prepend-icon="mdi-link-variant"
                                      required
                                    ></v-text-field>
                                  </v-col>
                                  <v-col cols="12">
                                    <v-autocomplete
                                      v-model="personInCharge"
                                      :items="teamMember"
                                      prepend-icon="mdi-account"
                                      attach
                                      chips
                                      :menu-props="{ top: true, offsetY: true }"
                                      label="Person in charge"
                                      multiple
                                    ></v-autocomplete>
                                  </v-col>
                                  <v-col
                                    cols="12"
                                  >
                                    <v-text-field
                                      v-model="envName"
                                      label="Environment Type"
                                      prepend-icon="mdi-nature"
                                      required
                                    ></v-text-field>
                                  </v-col>
                                  <v-col
                                    cols="12"
                                    sm="6"
                                  >
                                    <v-text-field
                                      v-model="envKey"
                                      label="Key"
                                      prepend-icon="mdi-key"
                                      required
                                    ></v-text-field>
                                  </v-col>
                                  <v-col
                                    cols="12"
                                    sm="6"
                                  >
                                    <v-text-field
                                      v-model="envValue"
                                      label="Value"
                                      prepend-icon="mdi-server-security"
                                      required
                                    ></v-text-field>
                                  </v-col>
                                </v-row>
                              </v-container>
                            </v-card-text>
                            <v-card-actions>
                              <v-spacer></v-spacer>
                              <v-btn
                                color="green darken-1"
                                text
                                @click="addNewProject()"
                              >
                                ตกลง
                              </v-btn>
                              <v-btn
                                color="red darken-2"
                                text

                                @click="dialogAdd = false"
                              >
                                ยกเลิก
                              </v-btn>
                            </v-card-actions>
                          </v-card>
                        </v-dialog>
                      </v-row>
                    </template>


                  </v-card-title>

                </v-col>

                <v-col
                  v-for="data in allData"
                  cols="12"
                  sm="6"
                  md="4"
                  lg="3"
                >
                  <v-card>
                    <v-card-title class="subheading font-weight-bold">
                      {{ data.ProjectName }}
                      <!--                      {{data}}-->
                    </v-card-title>

                    <v-divider></v-divider>

                    <v-list dense>
                      <v-list-item
                        v-for="(key, index) in filteredKeys"
                        :key="card"
                      >
                        <v-list-item-content :class="{ 'blue--text': sortBy === key }">
                          {{ key }}:
                        </v-list-item-content>
                        <v-list-item-content
                          class="align-end"
                          :class="{ 'blue--text': sortBy === key }"
                        >
                          <v-row>
                            <v-col cols="6">
                              <v-btn
                                depressed
                                color="primary"
                                @click="openDetail(data)"
                              >
                                รายละเอียด
                                <!--                                {{allData}}-->
                              </v-btn>
                            </v-col>
                            <v-col cols="6">
                              <v-btn
                                depressed
                                color="red"
                                class="white--text"
                                @click="deleteItem(data._id)"
                              >
                                ลบ
                                <!--                                {{allData}}-->
                              </v-btn>
                            </v-col>
                          </v-row>


                          <!--                          this.$router.push-->
                          <!--                          {{ items.createdBy }}-->
                          <!--                          {{ items[key.toLowerCase()] }}-->
                        </v-list-item-content>
                      </v-list-item>
                    </v-list>
                  </v-card>
                </v-col>
              </v-row>
            </template>

            <template v-slot:footer>
              <v-container>
                <v-row
                  class="mt-2"
                  align="center"
                  justify="center"
                >
                  <span class="grey--text">Items per page</span>
                  <v-menu offset-y>
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn
                        dark
                        text
                        color="primary"
                        class="ml-2"
                        v-bind="attrs"
                        v-on="on"
                      >
                        {{ itemsPerPage }}
                        <v-icon>mdi-chevron-down</v-icon>
                      </v-btn>
                    </template>
                    <v-list>
                      <v-list-item
                        v-for="(number, index) in itemsPerPageArray"
                        :key="card"
                        @click="updateItemsPerPage(number)"
                      >
                        <v-list-item-title>{{ number }}</v-list-item-title>
                      </v-list-item>
                    </v-list>
                  </v-menu>

                  <v-spacer></v-spacer>

                  <span
                    class="mr-4
            grey--text"
                  >
            Page {{ page }} of {{ numberOfPages }}
          </span>
                  <v-btn
                    fab
                    dark
                    color="blue darken-3"
                    class="mr-1"
                    @click="formerPage"
                  >
                    <v-icon>mdi-chevron-left</v-icon>
                  </v-btn>
                  <v-btn
                    fab
                    dark
                    color="blue darken-3"
                    class="ml-1"
                    @click="nextPage"
                  >
                    <v-icon>mdi-chevron-right</v-icon>
                  </v-btn>
                </v-row>
              </v-container>

            </template>
          </v-data-iterator>
        </v-container>
      </template>

    </v-col>
  </v-row>
</template>

<script>
import AddProject from "@/components/AddProject";

export default {
  data() {
    return {
      card: '',
      itemsPerPageArray: [10, 15, 20],
      search: '',
      filter: {},
      sortDesc: false,
      page: 1,
      itemsPerPage: 10,
      sortBy: 'name',
      keys: [
        'Name',
        'CreatedBy',
      ],
      allData: [],
      items: [
        {
          id: "1",
          name: 'System',
          createdBy: '123',
        },
        {
          id: "2",
          name: '123',
          CreatedBy: '123',

        },
        {
          id: "3",
          name: 'Syst333',

        },
        {
          id: "4",
          name: 'S4',
        },
        {
          id: "5",
          name: 'Sys5',


        },
        {
          id: "6",
          name: 'System True-Wallet6',

        },
        {
          id: "7",
          name: 'mepo2',

        },
        {
          id: "8",
          name: 'mepo1',

        },
        {
          id: "9",
          name: 'menu',

        },
        {
          id: "10",
          name: 'System True Wallets',
        },
        {
          id: "11",
          name: 'System True-Wallet2',

        },
        {
          id: "12",
          name: 'System True-Wallet3',
        },
        {
          id: "13",
          name: 'System True-Wallet4',

        },
      ],
      dialogAdd: false,
      dialogEdit: false,
      dialogCancleAdd: false,
      teamMember: ["firth", "Elon", "Bill", "Jeff", "Steve"],
      projectName: null,
      gitUrl: null,
      remoteName: null,
      personInCharge: [],
      description: null,
      envName : null,
      envKey : null,
      envValue:null,
      env: [{
        name:'',
        itemEnv:[
          {
            key:'',
            value:''
          }
        ]
      }],
    }
  },

  computed: {
    numberOfPages() {
      return Math.ceil(this.items.length / this.itemsPerPage)
    },
    filteredKeys() {
      return this.keys.filter(key => key !== 'Name')
    },

  },
  mounted() {
    this.fetchAllData()
  },
  methods: {
    addNewProject() {
      console.log('hi')
      this.$axios.post(`http://localhost/api/alldata`, {
        ProjectName: this.projectName,
        gitUrl: this.gitUrl,
        remoteName: this.remoteName,
        personInCharge: this.personInCharge,
        env : {
          name : this.envName,
          itemEnv: [{
            key: this.envKey,
            value: this.envValue,
          }]
        }
      })
        .then(({data}) => {
          // this.allData = data
          this.fetchAllData()
          console.log(data)
        })
        .catch((error) => console.error(error))
        .finally(() => this.dialogAdd = false)
    },
    // fetchAllData(){
    //   this.$axios.get(`http://localhost:80/api/alldata/${this.selectedCardId}`).then((dataResult)=>{
    //     this.data = dataResult
    //   }).catch((err)=>{
    //     (err)
    //   })
    // },
    fetchAllData() {
      this.$axios.get(`http://localhost/api/alldata`)
        .then(({data}) => {
          this.allData = data
        })
    },

    deleteItem(projectId) {
      this.$axios.delete(`http://localhost/api/delete/${projectId}`)
        .then(({data}) => {
          this.fetchAllData()
        })

      // app.delete('/api/delete/:id', (req, res) => {
      //   const projectId = req.params.id
      //   Alldata.remove({_id: projectId}, (err, deleteResult) => {
      //     if (err) {
      //       res.send(err.message)
      //     } else {
      //       res.json(deleteResult)
      //     }
      //   })
      // })
    },

    openDetail(data) {
      this.$router.push({path: `/project/moreDetail?id=${data._id}`})
    },
    nextPage() {
      if (this.page + 1 <= this.numberOfPages) this.page += 1
    },
    formerPage() {
      if (this.page - 1 >= 1) this.page -= 1
    },
    updateItemsPerPage(number) {
      this.itemsPerPage = number
    },
  },
  components: {
    AddProject
  }
}
</script>
