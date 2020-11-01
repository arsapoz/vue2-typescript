<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-toolbar dark color="teal">
          <v-toolbar-title>City selection</v-toolbar-title>
          <v-autocomplete
            v-model="select"
            :loading="loading"
            :items="items"
            item-text="name"
            item-value="id"
            :search-input.sync="search"
            class="mx-4"
            flat
            hide-no-data
            hide-details
            label="What city are you from?"
            solo-inverted
          ></v-autocomplete>
          <v-btn text @click="getWeatherByCity">
            Add
          </v-btn>
        </v-toolbar>
      </v-col>

      <v-col cols="12" v-if="weatherCards.length">
        <draggable  v-model="weatherCards" group="people" @start="drag=true" @end="drag=false">
        <v-card
          class="mx-auto mb-10"
          max-width="400"
          v-for="card in weatherCards"
          :key="card.id"
        >
          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title class="headline">
                {{ card.name }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>

          <v-card-text>
            <v-row align="center">
              <v-col class="display-3" cols="12">
                {{ card.main.temp }}
              </v-col>
            </v-row>
          </v-card-text>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-send</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle
              >{{ card.wind.speed }} km/h</v-list-item-subtitle
            >
          </v-list-item>

          <!-- <v-slider
              v-model="time"
              :max="6"
              :tick-labels="labels"
              class="mx-4"
              ticks
          ></v-slider>

          <v-list class="transparent">
            <v-list-item
                v-for="item in forecast"
                :key="item.day"
            >
              <v-list-item-title>{{ item.day }}</v-list-item-title>

              <v-list-item-icon>
                <v-icon>{{ item.icon }}</v-icon>
              </v-list-item-icon>

              <v-list-item-subtitle class="text-right">
                {{ item.temp }}
              </v-list-item-subtitle>
            </v-list-item>
          </v-list>-->

          <v-divider></v-divider>

          <v-card-actions>
            <v-btn text @click="removeCard(card.id)">
              Remove card
            </v-btn>
          </v-card-actions>
        </v-card>
        </draggable>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";
import draggable from 'vuedraggable'

export default Vue.extend({
  name: "Weather",
  components: {
    draggable,
  },
  data: () => ({
    weatherCards: [],
    loading: false,
    drag: false,
    items: [],
    search: null,
    select: null,
    states: [],
    apiKey: "8041567580af3b158a42e66ef367b44a"
  }),
  watch: {
    search(val) {
      val && val !== this.select && this.querySelections(val);
    }
  },
  mounted() {
    this.getCities();
  },
  methods: {
    removeCard(cardId) {
      const i = this.weatherCards.map(card => card.id).indexOf(cardId);
      this.weatherCards.splice(i, 1);
    },
    getCities() {
      axios.get("/current.city.list.min.json").then(resp => {
        this.states = resp.data;
      });
    },
    getWeatherByCity() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?id=${this.select}&appid=${this.apiKey}`
        )
        .then(resp => {
          this.weatherCards.push(resp.data);
        })
        .finally(() => {
          this.search = null;
          this.select = null;
        });
    },
    querySelections(value) {
      this.loading = true;
      // Simulated ajax query
      this.items = this.states.filter(state => {
        return (
          (state.name || "")
            .toLowerCase()
            .indexOf((value || "").toLowerCase()) > -1
        );
      });
      this.loading = false;
    }
  }
});
</script>
