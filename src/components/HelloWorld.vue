<template>
  <v-container >
     <v-snackbar
      v-model="snackbar"
      top
      color="indigo"
      class="white--text"
    >
    {{ snackbarText }}
      <template v-slot:action="{ attrs }">
        <v-btn
          color="white"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-row>
      <v-col cols="12" sm="6" md="3">
        <v-select
        v-model="model"
        :items="type"
        :append-icon="appendIcon ? 'mdi-plus' : ''"
        label="Type"
        outlined
      >
         </v-select>
      </v-col>
      <v-col cols="12" sm="6" md="3">
          <v-text-field
          v-model="reason"
            label="Transcation Reason"
            outlined
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6" md="3">
          <v-text-field
            v-model="amount"
            label="Amount"
            outlined
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6" md="3">
          <v-btn dark class="indigo white--text" @click="process">Procced</v-btn>
      </v-col>
    </v-row>
    <v-card>
    <v-card-title>
      Balance: {{balance}}
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
    :headers="headers"
    :items="items"
    :items-per-page="5"
    :item-class= "row_classes"
    class="elevation-1"
  ></v-data-table>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',

    data: () => ({
      snackbar: null,
      snackbarText: "SomeError",
      balance: 0,
      appendIcon: true,
       type: ['Credit', 'Debit'],
       model: null,
       reason: null,
       amount: null,
       search: null,
       headers:[ {
            text: 'S.no',
            align: 'start',
            sortable: false,
            value: 'sno',
          },
          { text: 'Type',align: 'start', value: 'type' },
          { text: 'Reason',sortable: false,align: 'start', value: 'reason',color: "red" },
          { text: 'Balance',align: 'start', value: 'balance' },
          { text: 'Credit/Debit Amount',align: 'start', value: 'typeCredit' },
          { text: 'Date',align: 'start', value: 'date' },
          ],
          items:[]
    }),
  methods:{
    row_classes(item) {
        if (item.type === "Debit") {
          return 'redColor'
        } else if(item.type === "Credit"){
          return 'greenColor'
        }
    },
    add(x, y){
      var a = parseFloat(x);
      var b = parseFloat(y);
      return (a+b);
    },
    minus(x,y){
      var a = parseFloat(x);
      var b = parseFloat(y);
      return (a-b);
    },
    process(){
      console.log(this.$vuetify.theme.dark);
      var m = parseFloat(this.amount)
      var n = m.toString()
      if(n === this.amount){
        if(this.reason !== null){
        if(this.model === "Credit"){
        //
        var amt = this.add(this.balance, this.amount);
        this.balance = amt;
        this.items.push({
          sno: this.items.length + 1,
          type: this.model,
          reason: this.reason,
          balance: amt,
          typeCredit: this.amount,
          date: new Date()
        })

      }else if(this.model === "Debit"){
        //
        var amtMinus = this.minus(this.balance, this.amount);
        this.balance = amtMinus;
        this.items.push({
          sno: this.items.length + 1,
          type: this.model,
          reason: this.reason,
          balance: amtMinus,
          typeCredit: this.amount,
          date: new Date()
        })
      }
      else{
        this.snackbar = true;
        this.snackbarText = "Select Credit or Debit"
      }}else{
        this.snackbar = true;
        this.snackbarText = "Enter Reason"
      }
      }else{
        this.snackbar = true;
        this.snackbarText = "Invalid Amount"
      }
    }
  }
  }
</script>

<style scoped>
.green{
  background-color: green;
  font-size: 60px;
}
</style>