<template>
  <div id="delete-student">
    <div class="breadcrumb" style="margin-bottom: 20px">
      <div class="container" style="padding: 10px 20px;">
        <router-link class="breadcrumb-item" to="/">Home</router-link>
        <!-- <a class="breadcrumb-item" href="index.html">Book</a> -->
        <span class="breadcrumb-item active">Search Student</span>
      </div>
    </div>

    <div style="margin-left: 10%; margin-right: 10%">
      <hr />
      <h2>Delete Student Page</h2>
      <hr />
    </div>

    <div class="centre">
      <v-card-title>
        <v-spacer></v-spacer>
        <v-text-field v-model="search" append-icon="search" label="Search" single-line hide-details></v-text-field>
      </v-card-title>
    </div>

    <div class="list">
      <v-data-table fixed-header :headers="headers" :items="items" :search="search">
        <v-progress-linear v-show="progressBar" color="blue" indeterminate></v-progress-linear>
        <!-- <template v-slot:items="props">
          <tr>
            <td class="text-xs-left">{{ props.item.student_id}}</td>
            <td class="text-xs-left">{{ props.item.name }}</td>
            <td class="text-xs-left">{{ props.item.email }}</td>
            <td class="text-xs-left">{{ props.item.contact }}</td>
            <td class="text-xs-left"><v-icon
                small
                @click="deleteStud(props.item)"
            >delete</v-icon></td>
          </tr>
        </template>-->
        <template v-slot:item.action="{ item }">
          <v-icon small @click="deleteStud(item)">delete</v-icon>
        </template>
      </v-data-table>
    </div>

    <!-- <b-container> -->
    <!-- User Interface controls -->
    <!-- <b-row>
        <b-col md="6" class="my-1">
          <b-form-group label-cols-sm="3" label="Filter" class="mb-0">
            <b-input-group>
              <b-form-input v-model="filter" placeholder="Type to Search"></b-form-input>
              <b-input-group-append>
                <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
              </b-input-group-append>
            </b-input-group>
          </b-form-group>
        </b-col>

        <b-col md="6" class="my-1">
          <b-form-group label-cols-sm="3" label="Sort" class="mb-0">
            <b-input-group>
              <b-form-select v-model="sortBy" :options="sortOptions">
                <option slot="first" :value="null">-- none --</option>
              </b-form-select>
              <b-form-select v-model="sortDesc" :disabled="!sortBy" slot="append">
                <option :value="false">Asc</option>
                <option :value="true">Desc</option>
              </b-form-select>
            </b-input-group>
          </b-form-group>
        </b-col>

        <b-col md="6" class="my-1">
          <b-form-group label-cols-sm="3" label="Sort direction" class="mb-0">
            <b-form-select v-model="sortDirection">
              <option value="asc">Asc</option>
              <option value="desc">Desc</option>
              <option value="last">Last</option>
            </b-form-select>
          </b-form-group>
        </b-col>

        <b-col md="6" class="my-1">
          <b-form-group label-cols-sm="3" label="Per page" class="mb-0">
            <b-form-select v-model="perPage" :options="pageOptions"></b-form-select>
          </b-form-group>
        </b-col>
    </b-row>-->

    <!-- Main table element -->
    <!-- <b-table
        show-empty
        stacked="md"
        selectable
        :select-mode="selectMode"
        selectedVariant="success"
        :items="items"
        :fields="fields"
        :current-page="currentPage"
        :per-page="perPage"
        :filter="filter"
        :sort-by.sync="sortBy"
        :sort-desc.sync="sortDesc"
        :sort-direction="sortDirection"
        @filtered="onFiltered"
        @row-selected="rowSelected"
      >
        <template slot="selected" slot-scope="{ rowSelected }">
          <span v-if="rowSelected">✔</span>
        </template>
      </b-table>

      <b-row class="row d-flex justify-content-center">
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="my-0"
        ></b-pagination>
      </b-row>
    </b-container>-->
  </div>
</template>

<script>
import db from "./../../firebase/firestoreInit";
import Vue from "vue";
import VeeValidate from "vee-validate";
import { firestorePlugin } from "vuefire";
import BootstrapVue from "bootstrap-vue";

Vue.use(BootstrapVue);
Vue.use(firestorePlugin);
Vue.use(VeeValidate);

// This imports all the layout components such as <b-container>, <b-row>, <b-col>:
import { LayoutPlugin } from "bootstrap-vue";
Vue.use(LayoutPlugin);

// This imports <b-modal> as well as the v-b-modal directive as a plugin:
import { ModalPlugin } from "bootstrap-vue";
Vue.use(ModalPlugin);

// This imports <b-card> along with all the <b-card-*> sub-components as a plugin:
import { CardPlugin } from "bootstrap-vue";
Vue.use(CardPlugin);

// This imports directive v-b-scrollspy as a plugin:
import { VBScrollspyPlugin } from "bootstrap-vue";
Vue.use(VBScrollspyPlugin);

// This imports the dropdown and table plugins
import { DropdownPlugin, TablePlugin } from "bootstrap-vue";
Vue.use(DropdownPlugin);
Vue.use(TablePlugin);

import Vuetify from "vuetify";
import "vuetify/dist/vuetify.min.css";
Vue.use(Vuetify);

export default {
  name: "delete-student",
  data() {
    return {
      search: "",
      items: [],
      headers: [
        {
          text: "Student ID",
          value: "student_id",
          align: "left",
          sortable: true
        },
        {
          text: "Student Name",
          value: "name",
          align: "left",
          sortable: true
        },
        {
          text: "Student Email",
          value: "email",
          align: "left",
          sortable: true
        },
        {
          text: "Student Contact",
          value: "contact",
          align: "left",
          sortable: true
        },
        {
          text: "Action",
          value: "action",
          align: "left",
          sortable: false
        }
      ],
      totalRows: 1,
      currentPage: 1,
      perPage: 5,
      pageOptions: [5, 10, 15],
      sortBy: null,
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
      selected: [],
      selectMode: "single",
      role: "",
      valid: true
    };
  },
  firestore() {
    return {
      items: db.collection("students").orderBy("name")
    };
  },
  computed: {
    sortOptions() {
      // Create an options list from our fields
      return this.fields
        .filter(f => f.sortable)
        .map(f => {
          return { text: f.label, value: f.key };
        });
    },
    rows() {
      return this.items.length;
    }
  },
  mounted() {
    // Set the initial number of items
    this.totalRows = this.items.length;
  },
  beforeRouteEnter(to, from, next) {
    next(vm => {
      vm.getReserve(vm);
    });
  },
  methods: {
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
    clear() {
      this.$refs.form.reset();
    },
    deleteStud(student) {
      if (confirm("Are you sure to remove this record?")) {
        db.collection("students")
          .doc(student.id)
          .delete();
      } else {
      }
    }
  }
};
</script>

<style scoped>
div.centre {
  text-align: center;
  width: 60%;
  margin-right: 30px;
}

div.list {
  text-align: justify;
  width: 75%;
  margin: auto;
  margin-bottom: 20px;
}

.sign-up {
  margin-top: 40px;
}

input {
  margin: 10px 0;
  width: 20%;
  padding: 15px;
}
input[type="radio"] {
  width: 30px;
}

.no-spinner::-webkit-outer-spin-button,
.no-spinner::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

label.radio-inline {
  margin: 0px 10px;
}

button {
  margin-top: 20px;
  width: 10%;
  cursor: pointer;
}
p {
  margin-top: 40px;
  font-size: 13px;
}
p a {
  text-decoration: underline;
  cursor: pointer;
}

table {
  margin-left: 10%;
  margin-right: 10%;
  border-collapse: collapse;
  width: 80%;
}

th,
td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

tr:hover {
  background-color: #f5f5f5;
}
</style>


