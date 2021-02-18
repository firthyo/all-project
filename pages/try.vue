<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <template>
        <v-container fluid>
          <v-data-iterator
            :items="allDetail"
            :items-per-page.sync="itemsPerPage"
            :page.sync="page"
            :search="search"
            :sort-by="sortBy.toLowerCase()"
            :sort-desc="sortDesc"
            hide-default-footer
          >
            <!--*********************-->
            <!-- Search-->
            <!--*********************-->
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
                  <v-select
                    class="ma-2"
                    v-model="sortBy"
                    flat
                    solo-inverted
                    hide-details
                    :items="keys"
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
            <!--*********************-->
            <!-- Project list -->
            <!--*********************-->
            <template v-slot:default="props">
              <v-row>
                <v-col
                  cols="12"
                >
                  <!--                  template -->
                  <template>
                    <v-btn
                      v-bind="attrs"
                      color="primary"
                      class="white--text; ma-2"
                      @click.stop="dialogAdd = true"
                    >
                      Add Project
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
                                  v-model="addProjectName"
                                  label="Project Name"
                                  prepend-icon="mdi-folder"

                                  required
                                ></v-text-field>
                                <v-text-field
                                  v-model="addDetail"
                                  label="Detail"
                                  prepend-icon="mdi-folder"
                                  required
                                ></v-text-field>
                              </v-col>
                              <v-col cols="12">
                                <v-text-field
                                  v-model="addGitRepo"
                                  label="Git Repository Name"
                                  prepend-icon="mdi-git"
                                  required
                                ></v-text-field>
                              </v-col>
                              <v-col cols="12">
                                <v-autocomplete
                                  v-model="addMember"
                                  :items="teamMember"
                                  prepend-icon="mdi-account"
                                  attach
                                  chips
                                  label="Person in charge"
                                  multiple
                                ></v-autocomplete>
                              </v-col>

                              <v-col
                                cols="12"
                              >
                                <v-text-field
                                  v-model="addEnv"
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
                                  v-model="addKey"
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
                                  v-model="addValue"
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
                            @click="save"
                          >
                            Save
                          </v-btn>
                          <v-btn
                            color="red darken-2"
                            text
                            @click="dialog = false"
                          >
                            Cancel
                          </v-btn>
                        </v-card-actions>
                      </v-card>
                    </v-dialog>
                  </template>

                  <v-expansion-panels>
                    <v-expansion-panel
                      v-for="(item,i) in allDetail "
                      :key="i"
                    >
                      <v-expansion-panel-header expand-icon="mdi-menu-down">
                        <h2 style="font-weight: inherit">{{ item.productName }}</h2>
                      </v-expansion-panel-header>
                      <!--*********************-->
                      <!-- Detail-->
                      <!--*********************-->
                      <v-expansion-panel-content
                      >
                        <template>
                          <v-container>
                            <v-row>
                              <v-col cols="12">
                                <p class="display-2 text--primary" style="font-weight: lighter;">
                                  Detail :
                                </p>
                              </v-col>
                              <v-col cols="12">
                                <p class="display-5 text--primary">
                                  {{ item.detail }}
                                </p>
                              </v-col>
                              <v-col cols="12">
                                <v-card>
                                  <v-card-title>
                                    Git Repositories
                                  </v-card-title>
                                  <v-row>
                                    <v-col cols="4">
                                      <h3 style="margin: 15px">
                                        Remote name : <span
                                        style="color: #47494e; font-weight: lighter">{{ item.remoteName }}</span>
                                      </h3>
                                    </v-col>
                                    <v-col cols="8">
                                      <v-chip
                                        class="ma-2"
                                        color="primary"
                                        outlined

                                      >
                                        <v-icon left>
                                          mdi-git
                                        </v-icon>
                                        <a href="/https://github.com/zipzoft/mepo">{{ item.gitUrl }}</a>

                                      </v-chip>
                                      <template>
                                        <v-menu
                                          bottom
                                          right
                                          transition="scale-transition"
                                          origin="top left"
                                        >
                                          <template v-slot:activator="{ on }">
                                            <v-chip
                                              pill
                                              color="primary"
                                              v-for="person in item.personInCharge "
                                            >
                                              <v-avatar left>
                                                <v-icon>mdi-account-circle</v-icon>
                                              </v-avatar>
                                              {{ person }}
                                            </v-chip>
                                          </template>
                                          <v-card width="280">
                                            <v-list>
                                              <v-list-item>
                                                <v-list-item-avatar>
                                                  <v-img src="https://cdn.vuetifyjs.com/images/john.png"></v-img>
                                                </v-list-item-avatar>
                                                <v-list-item-content>
                                                  <v-list-item-title>John Leider</v-list-item-title>
                                                  <v-list-item-subtitle>john@vuetifyjs.com</v-list-item-subtitle>
                                                </v-list-item-content>
                                                <v-list-item-action>
                                                  <v-btn
                                                    icon
                                                    @click="menu = false"
                                                  >
                                                    <v-icon>mdi-close-circle</v-icon>
                                                  </v-btn>
                                                </v-list-item-action>
                                              </v-list-item>
                                            </v-list>
                                            <v-list>
                                              <v-list-item @click="() => {}">
                                                <v-list-item-action>
                                                  <v-icon>mdi-briefcase</v-icon>
                                                </v-list-item-action>
                                                <v-list-item-subtitle>john@gmail.com</v-list-item-subtitle>
                                              </v-list-item>
                                            </v-list>
                                          </v-card>
                                        </v-menu>
                                      </template>
                                    </v-col>
                                  </v-row>
                                  <v-divider></v-divider>
                                  <v-row>
                                    <!--branches-->
                                    <v-col cols="4">
                                      <h3 style="margin: 15px">
                                        Branches : <span style="color: #47494e; font-weight: lighter">12</span>
                                      </h3>
                                    </v-col>
                                    <v-col cols="8" style="margin-top: 15px">
                                      <v-chip
                                        class=""
                                        color="#1A237E"
                                        outlined
                                      >
                                        <v-icon left>
                                        </v-icon>
                                        master
                                      </v-chip>
                                      <v-chip
                                        class=""
                                        color="#1A237E"
                                        outlined
                                      >
                                        <v-icon left>
                                        </v-icon>
                                        call-center-chat-add-credit-picture
                                      </v-chip>
                                      <v-chip
                                        class=""
                                        color="#1A237E"
                                        outlined
                                      >
                                        <v-icon left>
                                        </v-icon>
                                        chat-banner
                                      </v-chip>
                                      <v-chip
                                        class=""
                                        color="#1A237E"
                                        outlined
                                      >
                                        <v-icon left>
                                        </v-icon>
                                        call-center-chat-add-credit-picture
                                      </v-chip>
                                      <v-chip
                                        class=""
                                        color="#1A237E"
                                        outlined
                                      >
                                        <v-icon>
                                          mdi-dots-horizontal
                                        </v-icon>
                                      </v-chip>
                                    </v-col>
                                  </v-row>
                                </v-card>
                              </v-col>
                              <v-col cols="12">
                                <v-card>
                                  <v-card-title>
                                    Environment
                                  </v-card-title>
                                  <v-card-text class="px-0">
                                    <v-expansion-panels class="px-3">
                                      <v-expansion-panel v-for="env in item.env">
                                        <v-expansion-panel-header expand-icon="mdi-menu-down"
                                        >
                                          <h3>{{ env.envname }}</h3>
                                        </v-expansion-panel-header>
                                        <v-expansion-panel-content>
                                          <v-list>
                                            .
                                            <v-row v-for="envItem in env.items">
                                              <v-col cols="5">
                                                <v-list-item>
                                                  <v-text-field label="Key" v-model="envItem.key" v-if="envItem.isEditing"></v-text-field>
                                                  <v-list-item-title v-else>Key : {{ envItem.key }}</v-list-item-title>
                                                </v-list-item>
                                              </v-col>
                                              <v-col cols="5">
                                                <v-list-item>
                                                  <v-text-field label="Value" v-model="envItem.value" v-if="envItem.isEditing"></v-text-field>
                                                  <v-list-item-title v-else>Value : {{ envItem.value }}</v-list-item-title>
                                                </v-list-item>
                                              </v-col>
                                              <v-col cols="2">
                                                <v-btn
                                                  @click="editEnvItem(envItem)"
                                                  class="mx-2"
                                                  fab
                                                  dark
                                                  small
                                                  color="primary"
                                                >
                                                  <v-icon dark>
                                                    {{ envItem.isEditing ? 'mdi-checkbox-marked' : 'mdi-pencil' }}
                                                  </v-icon>
                                                </v-btn>
                                                <v-btn
                                                  @click="deleteEnvItem(envItem, env.items)"
                                                  class="mx-2"
                                                  fab
                                                  dark
                                                  small
                                                  color="red"
                                                >
                                                  <v-icon dark>
                                                    mdi-delete
                                                  </v-icon>
                                                </v-btn>
                                              </v-col>
                                            </v-row>
                                          </v-list>
                                        </v-expansion-panel-content>

                                        <!--                                  <v-expansion-panel-content-->
                                        <!--                                    v-for="kv in envitem"-->
                                        <!--                                  >-->
                                        <!--                                    <v-row>-->
                                        <!--                                      <v-col cols="6">-->
                                        <!--                                        Key : {{ kv.key }}-->
                                        <!--                                      </v-col>-->
                                        <!--                                      <v-col cols="6">-->
                                        <!--                                        Value : {{ kv.value }}-->
                                        <!--                                      </v-col>-->
                                        <!--                                    </v-row>-->
                                        <!--                                  </v-expansion-panel-content>-->
                                      </v-expansion-panel>
                                    </v-expansion-panels>
                                  </v-card-text>
                                </v-card>
                              </v-col>
                              <v-col cols="12">
                                <template>
                                  <!--                           editItem(item) <-- item ที่ส่งไปหลังจากกดคือ ของที่อยุ่ในไหนกัน   -->
                                  <v-btn
                                    color="green lighten-1"
                                    class="ma-1 white--text"
                                    @click="editItem(item)"

                                  >
                                    แก้ไข
                                    <v-icon
                                      right
                                      dark
                                    >
                                      mdi-border-color
                                    </v-icon>
                                  </v-btn>
                                  <v-btn
                                    v-model="dialogDelete"
                                    color="red darken-1"
                                    class="ma-1 white--text"
                                    @click="deleteItem(item)"
                                  >
                                    ลบ
                                    <v-icon
                                      right
                                      dark
                                    >
                                      mdi-delete-empty
                                    </v-icon>
                                  </v-btn>
                                  <v-dialog v-model="dialogDelete" max-width="500px">
                                    <v-card>
                                      <v-card-title class="headline">Are you sure you want to delete this item?
                                      </v-card-title>
                                      <v-card-actions>
                                        <v-spacer></v-spacer>
                                        <v-btn color="green lighten-1" class="white--text" @click="deleteItemConfirm">OK
                                        </v-btn>
                                        <v-btn color="red darken-1" class="white--text" @click="closeDelete">Cancel</v-btn>
                                        <v-spacer></v-spacer>
                                      </v-card-actions>
                                    </v-card>
                                  </v-dialog>
                                  <v-dialog
                                    v-model="dialogEdit"
                                    max-width="600"

                                  >
                                    <v-card>
                                      <v-card-title>
                                        <span class="headline">Edit Project</span>
                                      </v-card-title>
                                      <v-card-text>
                                        <v-container>
                                          <v-row>
                                            <v-col
                                              cols="12"
                                            >
                                              <v-text-field
                                                v-model="editedItem.productName"
                                                label="Project Name"
                                                prepend-icon="mdi-folder"
                                                required
                                              ></v-text-field>
                                            </v-col>
                                            <v-col cols="12">
                                              <v-text-field
                                                v-model="editedItem.remoteName"
                                                label="Git Repository Name"
                                                prepend-icon="mdi-git"
                                                required
                                              ></v-text-field>
                                            </v-col>
                                            <v-col cols="12">
                                              <v-autocomplete
                                                v-model="editedItem.personInCharge"
                                                :items="teamMember"
                                                prepend-icon="mdi-account"
                                                attach
                                                chips
                                                label="Person in charge"
                                                multiple
                                              ></v-autocomplete>
                                            </v-col>
                                            <v-col
                                              cols="12"
                                            >
                                              <!--                                          ต้องวนยังไงให้ Env แยก name val kay-->
                                              <v-text-field
                                                v-model="editedItem.env"
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
                                                label="Value"
                                                prepend-icon="mdi-server-security"
                                                required
                                              ></v-text-field>
                                            </v-col>
                                            <v-col cols="12" style="justify-items: center; align-items: center;">
                                              <v-btn
                                                @click="addEnvField()"
                                                depressed
                                                color="primary"
                                              >
                                                เพิ่ม Environment
                                              </v-btn>
                                            </v-col>
                                            <v-col
                                              cols="12"
                                            >
                                              <v-select
                                                v-model="value"
                                                :items="items"
                                                filled
                                                chips
                                                label="Chips"
                                                multiple
                                              ></v-select>
                                            </v-col>
                                            <v-col
                                              cols="6"
                                            >
                                              <v-text-field
                                                dense
                                                label="Regular"
                                              ></v-text-field>
                                            </v-col>
                                            <v-col
                                              cols="6"
                                            >
                                              <v-text-field
                                                dense
                                                label="Regular"
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
                                          @click="confirmEdit(edittem)"
                                        >
                                          ยืนยัน
                                        </v-btn>
                                        <v-btn
                                          color="red darken-2"
                                          text
                                          @click="cancelEdit()"
                                        >
                                          Cancle
                                        </v-btn>
                                      </v-card-actions>
                                    </v-card>
                                  </v-dialog>
                                </template>
                              </v-col>
                            </v-row>






                          </v-container>

                        </template>
                      </v-expansion-panel-content>
                    </v-expansion-panel>
                  </v-expansion-panels>
                </v-col>
              </v-row>
            </template>
            <!--*********************-->
            <!-- Footer-->
            <!--*********************-->
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
                      >
                        {{ itemsPerPage }}
                        <v-icon>mdi-chevron-down</v-icon>
                      </v-btn>
                    </template>
                    <v-list>
                      <v-list-item
                        v-for="(number, index) in itemsPerPageArray"
                        :key="index"
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
      // dialogSave : false,
      addProjectName: '',
      addDetail: '',
      addKey: '',
      addValue: '',
      addMember: '',
      addGitRepo: '',
      addEnv: '',
      dialogDelete: false,
      itemsPerPageArray: [10, 15, 20],
      search: '',
      filter: {},
      sortDesc: false,
      page: 1,
      itemsPerPage: 10,
      sortBy: 'name',
      attrs: "",
      isEditing: false,

      keys: [
        'Name',
        'CreatedBy',
      ],
      productName: [
        {
          name: 'System True-Wallet',
          createdBy: '123',
        },
        {
          name: 'chat-banner',
          CreatedBy: '123',
        },
        {
          name: 'free-credit',

        },
        {
          name: 'remove-main-manu',
        },
        {
          name: 'aks-me-pay',


        },
        {
          name: 'dashboard',

        },
        {
          name: 'live-streaming',

        },
        {
          name: 'customer-search',

        },
      ],
      dialogAdd: false,
      //save func <-- is in side dialogAdd
      dialogEdit: false,
      newItem: [],
      editedIndex: -1,
      edit: [],
      editedItem: {
        productName: '',
        remoteName: '',
        gitUrl: '',
        personInCharge: '',
        detail: '',
        env: []
      },
      // defaultItem: {
      //   productName: '',
      //   remoteName: '',
      //   gitUrl: '',
      //   personInCharge: '',
      //   detail: '',
      //   env:[]
      // },
      teamMember: ["firth", "Elon", "Bill", "Jeff", "Steve"],
      allDetail: [
        {
          productName: 'customer-search',
          remoteName: " zipzoft / mepo",
          gitUrl: 'https://github.com/zipzoft/mepo',
          personInCharge: ["firth", "Elon", "Bill", "Steve"],
          detail: "A pilot from Grand Rapids is delighted when she gets the chance to take part in the final of a dancing competition.",
          env: [
            {
              envname: "Staging",
              items: [
                {
                  id: 1,
                  key: "YYY",
                  value: "three NYC",
                  isEditing: false,
                },
                {
                  id: 2,
                  key: "ssss",
                  value: "ssss",
                  isEditing: false,
                },
              ],
            },
            {
              envname: "Project 51",
              items: [
                {
                  id: 3,
                  key: "AAA",
                  value: "1234 B NYC",
                  isEditing: false,
                },
              ],
            }
          ]
        },
        {
          productName: 'rename-menu',
          remoteName: " zipzoft / mepo",
          gitUrl: 'https://github.com/zipzoft/mepo',
          personInCharge: ["jennie", "jisoo", "rose", "lisa"],
          detail: "The hero is a goblin from Russia who has a particular interest in breakfast. The nemesis is a goblin who eats too much. It turns out the hero and the nemesis are twins.",
          env: [{
            envname: "Production",
            key: ["ABBC"],
            value: ["onw two three",],
          }],

        },
        {
          productName: 'chat-banner',
          remoteName: " zipzoft / carry",
          gitUrl: 'https://github.com/zipzoft/mepo',
          personInCharge: ["mozart", "bach", "beethoven", "chopin"],
          detail: "In a world where vampires are starving, one nurse has no choice but to protect mankind by eating his own child. It turns out that the nurse is an android.",
          env: [{
            envname: "Production",
            key: "A",
            value: "1",
          }]
        },
      ],
      items: [{
        name: "Production",
        key: "B",
        value: "9",
        detail: "A pilot from Grand Rapids is delighted when she gets the chance to take part in the final of a dancing competition. However, her chances are scuppered when her daughter goes missing. Distraught at losing a dancing competition, the pilot kills herself.",
      },
        {
          name: "Staging",
          key: "KITTY KITTYKAT",
          value: "1",
          detail: "When a fairy from Darwin decides to become a fugitive, not everybody is supportive. However, her fortunes improve when her gardener finds a magic knitting needle. It turns out they were teddy bears all along.",
        }],
    }

  },
  computed: {
    numberOfPages() {
      return Math.ceil(this.items.length / this.itemsPerPage)
    },
    // formTitle () {
    //   return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    // },
  },
  methods: {
    deleteEnvItem(envItem, env) {
      // let index = env.indexOf(envItem)

      let index = env.findIndex(({id})=>{
        return id === envItem.id
      })

      // let index = null;
      // let i = 0;
      // for(i = 0; i < env.length; i++){
      //   if(env[i].id === envItem.id){
      //     index = i;
      //   }
      // }

      if(index){
        env.splice(index, 1)
      }

      // call api send id & search or get response -> set evn items
    },
    editEnvItem(evnItem) {
      if(evnItem){
        if(evnItem.isEditing){
          // save
          evnItem.isEditing = false

        }else{
          // change ui to edit mode
          evnItem.isEditing = true
        }
      }
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
    deleteItemConfirm() {
      this.allDetail.splice(this.editedIndex, 1)
      this.closeDelete()
    },
    closeDelete() {
      this.dialogDelete = false
    },
    deleteItem(item) {
      this.editedIndex = this.allDetail.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },
    cancelEdit() {
      this.dialogEdit = false
    },
    save() {
      this.newItem = {
        productName: this.addProjectName,
        remoteName: this.addGitRepo,
        gitUrl: 'https://github.com/zipzoft/mepo',
        personInCharge: this.addMember,
        detail: this.addDetail,
        env: [{
          envname: this.addEnv,
          key: this.addKey,
          value: this.addValue,
        }]
      }
      if (this.editedIndex > -1) {
        // Object.assign({this.allDetail},this.newItem)
        Object.assign(this.allDetail, this.newItem)
      } else {
        this.allDetail.push(this.newItem)
      }
      //close dialog
      this.closeDelete()
      // this.close()
      this.dialogAdd = false
    },
    editItem(item) {
      this.editedIndex = this.newItem.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogEdit = true
    },
    addEnvField() {
      //  push field
    },
    confirmEdit(editItem) {
      this.dialogEdit = false
      // editItem =
      this.edit = {
        productName: editItem,
        remoteName: editItem,
        gitUrl: 'https://github.com/zipzoft/mepo',
        personInCharge: editItem,
        detail: editItem,
        env: [{
          envname: editItem,
          key: editItem,
          value: editItem,
        }]
      }
      if (this.editedIndex > -1) {
        // Object.assign({this.allDetail},this.newItem)
        Object.assign(this.allDetail, this.edit)
      } else {
        this.allDetail.push(this.newItem)
      }
      //close dialog
      this.closeDelete()
      // this.close()
      this.dialogEdit = false
    },
  },
  components: {
    AddProject
  }
}
</script>
