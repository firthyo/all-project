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
                  <div class="ma-2">
                    <v-btn
                      color="primary"
                      class="white--text"
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
                  </div>
                  <v-expansion-panels>
                    <v-expansion-panel
                      v-for="(item,i) in productName"
                      :key="i"
                    >
                      <v-expansion-panel-header expand-icon="mdi-menu-down">
                        <h2 style="font-weight: inherit">{{ item.name }}</h2>
                      </v-expansion-panel-header>
                      <!--*********************-->
                      <!-- Detail-->
                      <!--*********************-->
                      <v-expansion-panel-content v-for="(x,i) in detail"
                                                 :key="i">

                        <template>
                          <v-container>
                            <p class="display-1 text--primary">
                              Detail :
                            </p>
                            <p class="display-5 text--primary">
                              {{ x.description }}
                            </p>
                            <v-card>
                              <v-card-title>
                                Git Repositories
                              </v-card-title>
                              <v-row>
                                <v-col cols="4">
                                  <h3 style="margin: 15px">
                                    Remote name : <span
                                    style="color: #47494e; font-weight: lighter">{{x.git}}</span>
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
                                    <a href="/https://github.com/zipzoft/mepo">https://github.com/zipzoft/mepo</a>
                                    {{x.git.gitUrl}}
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
                                        >
                                          <v-avatar left>
                                            <v-icon>mdi-account-circle</v-icon>
                                          </v-avatar>
                                          Firth Maneesuksri
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
                                        >
                                          <v-avatar left>
                                            <v-icon>
                                              mdi-account-circle
                                            </v-icon>
                                          </v-avatar>
                                          Elmo Berg
                                        </v-chip>
                                      </template>
                                    </v-menu>
                                  </template>
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
                                        >
                                          <v-avatar left>
                                            <v-icon>mdi-account-circle</v-icon>
                                          </v-avatar>
                                          Elon Musk
                                        </v-chip>
                                      </template>
                                    </v-menu>
                                  </template>
                                </v-col>
                              </v-row>
                              <v-divider></v-divider>
                              <v-row>
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
                                  v-for="(items ,i) in items"
                                  :key="i"
                                >
                                  <v-expansion-panel-header expand-icon="mdi-menu-down">
                                    <h3>{{ items.name }}</h3>
                                  </v-expansion-panel-header>
                                  <v-expansion-panel-content>
                                    <v-row>
                                      <v-col cols="6">
                                        Key : {{ items.key }}
                                      </v-col>
                                      <v-col cols="6">
                                        Value : {{ items.value }}
                                      </v-col>
                                      <v-col cols="6">
                                        Key : AB ABC ABCD
                                      </v-col>
                                      <v-col cols="6">
                                        Value : 123456789
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
                                  <v-card-title class="headline">Are you sure you want to delete this item?</v-card-title>
                                  <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="green lighten-1" class="white--text" @click="deleteItemConfirm">OK</v-btn>
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
      teamMember: ["firth", "Elon", "Bill", "Jeff", "Steve"],
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
      lorem: 'Lorem ipsum dolor sit amet, at aliquam vivendum vel,  Eum in consul legimus accusam. corruptegendi ius at, at nemore equam cum.',
      detail: [{
        description: "A pilot from Grand Rapids is delighted when she gets the chance to take part in the final of a dancing competition. However, her chances are scuppered when her daughter goes missing. Distraught at losing a dancing competition, the pilot kills herself.",
        git: [{
          remoteName: " zipzoft / mepo",
          gitUrl: 'https://github.com/zipzoft/mepo',
          personInCharge: ['firth', 'win', 'snuc']
        }],
        environ: [{
          name: "Production",
          key: "AB BC ABCD",
          value: "123456789",
        }]
      },
      ]
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
    deleteItem(item) {
      // this.editedIndex = this.desserts.indexOf(item)
      // this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },
    editItem(item) {
      // this.editedIndex = this.desserts.indexOf(item)
      // this.editedItem = Object.assign({}, item)
      this.dialogEdit = true
    },
  },
  components: {
    AddProject
  }
}
</script>
