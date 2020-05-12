<template>
  <v-layout align-start row fill-height>
    <v-container fluid>
      <v-card class="mx-auto">
        <v-col class="math">
          <v-row class="heading">Plate Calculator</v-row>
          <v-row dense>
            <v-form>
              <v-text-field
                v-model="barWeight"
                label="Bar Weight"
              ></v-text-field>
              <v-text-field
                v-model="totalWeight"
                label="Total Weight"
              ></v-text-field>
              <v-btn text @click="calculate" class="ma-2">Calculate</v-btn>
              <v-switch
                v-model="roundUp"
                class="ma-2"
                label="Round Up"
              ></v-switch>
            </v-form>
          </v-row>
          <v-col>
            <v-subheader>Plates per side: </v-subheader>

            <v-row class="results">
              <v-simple-table v-for="plate in plates" :key="plate.weight">
                <thead>
                  <tr>
                    <th>{{ plate.type }}</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>{{ plate.count }}</td>
                  </tr>
                </tbody>
              </v-simple-table>
            </v-row>
          </v-col>
          <v-col class="plates"> </v-col>
        </v-col>
      </v-card>
    </v-container>
  </v-layout>
</template>

<script>
export default {
  name: "Index",

  data: () => ({
    barWeight: 0,
    totalWeight: 0,
    roundUp: false,
    plates: [
      {
        type: "25kg Plate",
        weight: 25,
        count: 0,
      },
      {
        type: "20kg Plate",
        weight: 20,
        count: 0,
      },
      {
        type: "15kg Plate",
        weight: 15,
        count: 0,
      },
      {
        type: "10kg Plate",
        weight: 10,
        count: 0,
      },
      {
        type: "5kg Plate",
        weight: 5,
        count: 0,
      },
      {
        type: "2.5kg Plate",
        weight: 2.5,
        count: 0,
      },
    ],
  }),
  methods: {
    isInteger(value) {
      if (undefined === value || null === value) {
        return false;
      }
      return value % 1 == 0;
    },
    reset() {
      for (let i = 0; i < 5; i++) {
        this.plates[i].count = 0;
      }
    },
    calculate() {
      this.reset();
      let calcNum = (parseInt(this.totalWeight) - parseInt(this.barWeight)) / 2;
      //if number is 0 then finish here
      if (calcNum <= 0) return;
      let roundedNum = 0;

      if (roundedNum <= 80 && this.isInteger(roundedNum / 20)) {
        console.log("it's a 20 divisible number: " + roundedNum);
        this.plates[1].count = roundedNum / 20;
        return 0;
      }
      this.math(roundedNum);
    },
    math(roundedNum) {
      for (let i = 0; i < 5; i++) {
        //first checks if can use 25kg plates only, if so finishes there with return;
        if (this.isInteger(roundedNum / this.plates[i].weight)) {
          this.plates[i].count = roundedNum / this.plates[i].weight;
          return 0;
        } //if number isn't an int i.e is float then...
        else if (!this.isInteger(roundedNum / this.plates[i].weight)) {
          //if the number is > 0 but a float e.g. 2.3 then...
          if (roundedNum / this.plates[i].weight > 0) {
            // remove decimal, add int to count at position i
            this.plates[i].count = Math.floor(
              roundedNum / this.plates[i].weight
            );
            // remove already calc'd amount from total
            roundedNum =
              roundedNum - this.plates[i].count * this.plates[i].weight;
          }
        }
      }
    },
  },
};
</script>

<style>
.v-card {
  max-width: 50% !important;
}
.row {
  padding-left: 2%;
}
.v-simple-table {
  text-align: center;
}
</style>
