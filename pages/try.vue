<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <template>
        <v-container fluid>
          <v-data-iterator
            :items="testtest"
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
                  <template >
                    <v-btn
                      v-bind="attrs"
                      v-on="on"
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
                                  label="Project Name"
                                  prepend-icon="mdi-folder"
                                  required
                                ></v-text-field>
                              </v-col>
                              <v-col cols="12">
                                <v-text-field
                                  label="Git Repository Name"
                                  prepend-icon="mdi-git"
                                  required
                                ></v-text-field>
                              </v-col>
                              <v-col cols="12">
                                <v-autocomplete
                                  v-model="value"
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
                            Cancle
                          </v-btn>
                        </v-card-actions>
                      </v-card>
                    </v-dialog>
                  </template>

                  <v-expansion-panels>
                    <v-expansion-panel
                      v-for="(item,i) in testtest "
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

                            <p class="display-2 text--primary" style="font-weight: lighter;">
                              Detail :
                            </p>
                            <p class="display-5 text--primary">
                              {{ item.detail }}
                            </p>
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
                                      v-model="menu"
                                      bottom
                                      right
                                      transition="scale-transition"
                                      origin="top left"
                                    >
                                      <template v-slot:activator="{ on }">
                                        <v-chip
                                          pill
                                          v-on="on"
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
                            <v-card style="margin-top: 30px">
                              <v-row>
                                <v-col cols="6">
                                  <v-card-title>
                                    Environment
                                  </v-card-title>
                                </v-col>
                              </v-row>
                              <v-expansion-panels class="mb-6">
                                <v-expansion-panel
                                >
                                  <v-expansion-panel-header expand-icon="mdi-menu-down"
                                                            v-for="envitem in item.env"
                                  >
                                    <h3>{{ envitem.envname}}</h3>
                                  </v-expansion-panel-header>
                                  <v-expansion-panel-content
                                    v-for="kv in item.env"
                                  >
                                    <v-row>
                                      <v-col cols="6">
                                        Key : {{ kv.key }}
                                      </v-col>
                                      <v-col cols="6">
                                        Value : {{ kv.value }}
                                      </v-col>
                                    </v-row>
                                  </v-expansion-panel-content>
                                </v-expansion-panel>
                              </v-expansion-panels>
                            </v-card>
                            <template>
                              <v-btn
                                :loading="loading3"
                                :disabled="loading3"
                                color="green lighten-1"
                                class="ma-1 white--text"
                                @click="editItem()"
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
                                :loading="loading3"
                                :disabled="loading3"
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
                                            v-model="editedItem.name"
                                            label="Project Name"
                                            prepend-icon="mdi-folder"
                                            required
                                          ></v-text-field>
                                        </v-col>
                                        <v-col cols="12">
                                          <v-text-field
                                            label="Git Repository Name"
                                            prepend-icon="mdi-git"
                                            required
                                          ></v-text-field>
                                        </v-col>
                                        <v-col cols="12">
                                          <v-autocomplete
                                            v-model="value"
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
                                      </v-row>
                                    </v-container>
                                  </v-card-text>
                                  <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn
                                      color="green darken-1"
                                      text
                                      @click="dialog = false"
                                    >
                                      Add
                                    </v-btn>
                                    <v-btn
                                      color="red darken-2"
                                      text
                                      @click="dialog = false"
                                    >
                                      Cancle
                                    </v-btn>
                                  </v-card-actions>
                                </v-card>
                              </v-dialog>
                            </template>
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
      dialogDelete: false,
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
      dialogEdit: false,
      newItem: [],
      editedIndex: -1,
      editedItem: {
        productName: '',
        remoteName: '',
        gitUrl: '',
        personInCharge: '',
        detail: '',
        env:[]
      },
      defaultItem: {
        productName: '',
        remoteName: '',
        gitUrl: '',
        personInCharge: '',
        detail: '',
        env:[]
      },
      teamMember: ["firth", "Elon", "Bill", "Jeff", "Steve"],
      testtest: [{
        productName: 'customer-search',
        remoteName: " zipzoft / mepo",
        gitUrl: 'https://github.com/zipzoft/mepo',
        personInCharge: ["firth", "Elon", "Bill", "Steve"],
        detail: "A pilot from Grand Rapids is delighted when she gets the chance to take part in the final of a dancing competition.",
        env:[{
          envname: "Staging",
          key: ["III", "PPP", "MMM"],
          value: ["three NYC",],
        }]
      }, {
        productName: 'rename-menu\n',
        remoteName: " zipzoft / mepo",
        gitUrl: 'https://github.com/zipzoft/mepo',
        personInCharge: ["jennie", "jisoo", "rose", "lisa"],
        detail: "The hero is a goblin from Russia who has a particular interest in breakfast. The nemesis is a goblin who eats too much. It turns out the hero and the nemesis are twins.",
        env:[{
          envname: "Production",
          key: ["ABBC", "WWW", "JAKK"],
          value: ["onw two three",],
        }],

      },
        {
          productName: 'chat-banner',
          remoteName: " zipzoft / carry",
          gitUrl: 'https://github.com/zipzoft/mepo',
          personInCharge: ["mozart", "bach", "beethoven", "chopin"],
          detail: "In a world where vampires are starving, one nurse has no choice but to protect mankind by eating his own child. It turns out that the nurse is an android.",
          env:[{
            envname: "Production",
            key: "AB BC ABCD",
            value: "123456789",
          }]
        },
      ],
      items: [{
        name: "Production",
        key: "AB BC ABCD",
        value: "123456789",
        detail: "A pilot from Grand Rapids is delighted when she gets the chance to take part in the final of a dancing competition. However, her chances are scuppered when her daughter goes missing. Distraught at losing a dancing competition, the pilot kills herself.",
      },
        {
          name: "Staging",
          key: "KITTY KITTYKAT",
          value: "1212312121",
          detail: "When a fairy from Darwin decides to become a fugitive, not everybody is supportive. However, her fortunes improve when her gardener finds a magic knitting needle. It turns out they were teddy bears all along.",
        }],
    }
  },
  computed: {
    // formTitle () {
    //   return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    // },
  },
  methods: {
    nextPage() {
      if (this.page + 1 <= this.numberOfPages) this.page += 1
    },
    formerPage() {
      if (this.page - 1 >= 1) this.page -= 1
    },
    updateItemsPerPage(number) {
      this.itemsPerPage = number
    },
    deleteItemConfirm () {
      this.testtest.splice(this.editedIndex, 1)
      this.closeDelete()
    },
    deleteItem(item) {
      this.editedIndex = this.testtest.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },
    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.newItem[this.editedIndex], this.editedItem)
      } else {
        this.newItem.push(this.editedItem)
      }
      this.close()
    },
    editItem(item) {
      this.editedIndex = this.newItem.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogEdit = true
    },
  },
  components: {
    AddProject
  }
}
</script>
