<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    :single-expand="singleExpand"
    :expanded.sync="expanded"
    sort-by="calories"
    class="elevation-1"
    show-expand
    style="font-size: 40px"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>My CRUD</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              New Item
            </v-btn>
          </template>
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
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-file-document
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
    <template v-slot:expanded-item="{ headers, item }">
      <td :colspan="headers.length">
        <template>
          <v-container>
            <p class="display-3 text--primary">
              Mepo
            </p>
            <p class="display-5 text--primary">
              Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusantium debitis eum iste libero natus non odit
              suscipit veritatis vitae voluptates? Autem culpa, labore libero magni natus odio quia tempore voluptatum?
            </p>

            <v-card >
              <v-card-title>
                Git Repositories
              </v-card-title>
              <v-row>
                <v-col cols="4">
                  <h3 style="margin: 15px">
                    Remote name : <span style="color: #47494e; font-weight: lighter">zipzoft / mepo</span>
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
                              mdi-account-circle</v-icon>
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
                <v-col cols="6" >
                  <v-card-title >
                    <v-btn
                      class="ma-2"
                      color="purple"
                      dark
                    >
                      <v-icon dark>
                        mdi-wrench
                      </v-icon>
                    </v-btn>
                  </v-card-title>
                </v-col>
              </v-row>

              <v-expansion-panels class="mb-6">
                <v-expansion-panel
                  v-for="(items ,i) in items"
                  :key="i"

                >

                  <v-expansion-panel-header expand-icon="mdi-menu-down">
                    <h3>{{items.name}}</h3>
                  </v-expansion-panel-header>
                  <v-expansion-panel-content>
                    <v-row>
                      <v-col cols="6">
                        Key : {{items.key}}
                      </v-col>
                      <v-col cols="6">
                        Value : {{items.value}}
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



          </v-container>

        </template>
      </td>
    </template>

  </v-data-table>

</template>
<script>
export default {

  name: "crudTable.vue",

  data: () => ({
    expanded: [],
    singleExpand: false,
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: 'Project Name',
        align: 'start',
        // sortable: false,
        value: 'projectName',
      },
      {text: 'Git Repo Name', value: 'gitRepoName'},
      // {text: 'Fat (g)', value: 'fat'},
      // {text: 'Carbs (g)', value: 'carbs'},
      // {text: 'Protein (g)', value: 'protein'},
      {text:'Created By', value: 'createdBy'},
      {text: 'Actions', value: 'actions', sortable: false},
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      projectName: '',
      gitRepoName : 'System',
      createdBy : 'Firthyo',
    },
    defaultItem: {
      projectName: '',
      gitRepoName : '',
      createdBy : '',
    },
    menu: false,
    items:[{
      name: "Production",
      key:"AB BC ABCD",
      value:"123456789"
    },
      {
        name: "Staging",
        key:"KITTY KITTYKAT",
        value:"1212312121"
      }],

    messages: [
      {
        avatar: 'https://avatars0.githubusercontent.com/u/9064066?v=4&s=460',
        name: 'Production',
        title: 'Value',
        excerpt: 'hello hello hello hello hello hello hello',
      },
      {
        color: 'red',
        icon: 'mdi-account-multiple',
        name: 'Staging',
        new: 1,
        title: 'Value',
      },
      {
        color: 'teal',
        icon: 'mdi-tag',
        name: 'Dev',
        new: 2,
        title: 'Value',
        exceprt: 'New deals available, Join Today',
      },
    ],
    lorem: 'Lorem ipsum dolor sit amet, at aliquam vivendum vel, everti delicatissimi cu eos. Dico iuvaret debitis mel an, et cum zril menandri. Eum in consul legimus accusam. Ea dico abhorreant duo, quo illum minimum incorrupte no, nostro voluptaria sea eu. Suas eligendi ius at, at nemore equidem est. Sed in error hendrerit, in consul constituam cum.',


  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    },
  },

  watch: {
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
    },
  },

  created() {
    this.initialize()
  },

  methods: {
    initialize() {
      this.desserts = [
        {
          projectName: 'Frozen Yogurt',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
        {
          projectName: 'Ice cream sandwich',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
        {
          projectName: 'Eclair',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
        {
          projectName: 'Cupcake',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
        {
          projectName: 'Gingerbread',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
        {
          projectName: 'Jelly bean',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
        {
          projectName: 'Lollipop',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
        {
          projectName: 'Honeycomb',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
        {
          projectName: 'Donut',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
        {
          projectName: 'KitKat',
          gitRepoName : 'System',
          createdBy : 'Firthyo',
        },
      ]
    },

    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm() {
      this.desserts.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete() {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>




<style scoped>
table.v-table thead tr th {
  font-size: 20px!important;
}
</style>
