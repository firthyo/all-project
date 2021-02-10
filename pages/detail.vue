<template>
  <v-container>
    <p class="display-3 text--primary">
      Mepo
    </p>
    <p class="display-5 text--primary">
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusantium debitis eum iste libero natus non odit
      suscipit veritatis vitae voluptates? Autem culpa, labore libero magni natus odio quia tempore voluptatum?
    </p>

    <v-card>
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
              mdi-server-plus
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
                    <v-icon>mdi-account-circle</v-icon>
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
      <v-expansion-panels class="mb-6">
        <v-expansion-panel
        >

          <v-expansion-panel-header expand-icon="mdi-menu-down">
            <v-card-title>
              Environment
            </v-card-title>
          </v-expansion-panel-header>
          <v-expansion-panel-content>
            <h3 style="margin-left: 15px">Production</h3>
            <v-row style="margin-left: 5px">
              <v-col cols="6">
                Key name : ABC ABC ABC
              </v-col>
              <v-col cols="6">
                Value : ABC ABC ABC
              </v-col>
            </v-row>
          </v-expansion-panel-content>
          <v-divider></v-divider>
          <v-expansion-panel-content>
            <h3 style="margin-left: 15px">Staging</h3>
            <v-row style="margin-left: 5px">
              <v-col cols="6">
                Key name : EFG EFG EFG
              </v-col>
              <v-col cols="6">
                Value :  EFG EFG EFG
              </v-col>
            </v-row>
          </v-expansion-panel-content>


        </v-expansion-panel>
      </v-expansion-panels>

    </v-card>


  </v-container>

</template>
<script>
export default {
  data: () => ({
    menu: false,
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
}
</script>

<style scoped>

</style>

