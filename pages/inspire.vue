<template>
  <v-data-table
    :headers="dessertHeaders"
    :items="desserts"
    :single-expand="singleExpand"
    :expanded.sync="expanded"
    item-key="name"
    show-expand
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Expandable Table</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-switch
          v-model="singleExpand"
          label="Single expand"
          class="mt-2"
        ></v-switch>
      </v-toolbar>
    </template>
    <template v-slot:expanded-item="{ headers, item }">
      <td :colspan="headers.length">
        More info about {{ item.name }}
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
  data () {
    return {
      expanded: [],
      singleExpand: false,
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
      lorem: 'Lorem ipsum dolor sit amet, nemore equidem est. Sed in error hendrerit, in consul constituam cum.',
      dessertHeaders: [
        {
          text: 'Dessert (100g serving)',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Calories', value: 'calories' },
        { text: 'Fat (g)', value: 'fat' },
        { text: 'Carbs (g)', value: 'carbs' },
        { text: 'Protein (g)', value: 'protein' },
        {text: 'Actions', value: 'actions', sortable: false},
        { text: '', value: 'data-table-expand' },
      ],
      desserts: [
        {
          name: 'Frozen Yogurt',
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          iron: '1%',
          actions: 'icon',
        },
        {
          name: 'Ice cream sandwich',
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          iron: '1%',
        },
        {
          name: 'Eclair',
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          iron: '7%',
        },
        {
          name: 'Cupcake',
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          iron: '8%',
        },
        {
          name: 'Gingerbread',
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          iron: '16%',
        },
        {
          name: 'Jelly bean',
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          iron: '0%',
        },
        {
          name: 'Lollipop',
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          iron: '2%',
        },
        {
          name: 'Honeycomb',
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          iron: '45%',
        },
        {
          name: 'Donut',
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          iron: '22%',
        },
        {
          name: 'KitKat',
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          iron: '6%',
        },
      ],
    }
  },
}
</script>
