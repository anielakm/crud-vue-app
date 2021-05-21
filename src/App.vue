<template>
  <Header/>
  <List 
    :campaigns="campaigns" 
    @deleteItem="deleteItem" 
    @openEditPopup="openEditPopup"  
  />

  <Summary
    :fund="fund"
    :avaliableFund="avaliableFund"
    :bidSum="bidSum"
    :bidSumActive="bidSumActive"

    @openPopup="openPopup"

   />

  <Popup 

    @add="addItem"
    @close="closeEditPopup"
    @update="updateEditPopup"
    v-model="formValues"
    :popupMode="popupMode"
    :minValues="formMinValues"

    id="popup"
  />


</template>

<script>
import Header from './components/Header.vue'
import List from './components/List.vue'
import Popup from './components/Popup.vue'
import Summary from './components/Summary.vue'

export default {
  name: 'App',
  components: {
    Header,
    List,
    Popup,
    Summary
  },
  data(){
    return {
      formMinValues: {
        bid: 1000,
        radius: 10,
      },
      formValues: {
        name:'',
        keywords: [],
        bid: 1000,
        status: true,
        city: 'Cracow',
        radius: 10
      },

      fund: 1000000,
      popupMode: 'add',
      campaigns: [
      {
        id: 1,
        name: 'Notebook HP Pavilion 15',
        keywords: ['notebook', 'hp', 'it'],
        bid: 15500,
        fund: 0,
        status: true,
        city: 'Cracow',
        radius: 50
      },
      {
        id: 2,
        name: 'Notebook ASUS VivoBook',
        keywords: ['notebook', 'asus', 'it'],
        bid: 10000,
        fund: 0,
        status: true,
        city: 'Gdansk',
        radius: 30
      },
      {
        id: 3,
        name: 'Notebook Acer Nitro 5' ,
        keywords: ['notebook', 'acer', 'it'],
        bid: 36000,
        fund: 0,
        status: false,
        city: 'Wroclaw',
        radius: 33
      },
      {
        id: 4,
        name: 'Xiaomi Mi Pro 2',
        keywords: ['xiaomi', 'electric scooter', 'electric vehicle'],
        bid: 150000,
        fund: 0,
        status: true,
        city: 'Warsaw',
        radius: 33
      },
      {
        id: 5,
        name: 'Apple MacBook Air M1',
        keywords: ['apple', 'notebook'],
        bid: 200000,
        fund: 0,
        status: true,
        city: 'Gdynia',
        radius: 80
      },
      {
        id: 6,
        name: 'Xiaomi Redmi Note 9',
        keywords: ['xiaomi', 'redmi', 'smartphone'],
        bid: 328000,
        fund: 0,
        status: false,
        city: 'Poznan',
        radius: 83
      },
    ]

    }
    
  },

  methods: {

    deleteItem(id){
      this.campaigns.splice(this.campaigns.indexOf(this.campaigns.find(item => item.id === id)),1);
    },

    openEditPopup(editedItem){
      this.popupMode = 'edit'
      //create copy of object values in formValues object
      this.formValues = Object.assign({}, this.campaigns[this.campaigns.indexOf(this.campaigns.find(item => item.id === editedItem.id))])

      //open popup
      document.querySelector('#popup').style.display="flex";

    },

    addItem(){
      
      this.campaigns.push({
        id: this.campaigns.length + 1,
        name: this.formValues.name,
        keywords: this.formValues.keywords,
        bid: parseInt(this.formValues.bid),
        status: this.formValues.status,
        city: this.formValues.city,
        radius: parseInt(this.formValues.radius)
      })

      document.querySelector('#popup').style.display="none";
      
    },


    updateEditPopup(){
      this.popupMode = 'update'
      //update old object with new
      this.campaigns.splice(this.campaigns.indexOf(this.campaigns.find(item => item.id === this.formValues.id)),1,Object.assign({}, this.formValues));
      document.querySelector('#popup').style.display="none";
      this.clearFormValues()
    },

    closeEditPopup(){
      //close popup without changes
      document.querySelector('#popup').style.display="none";
      this.clearFormValues()
    },

    clearFormValues(){
      this.formValues = {
        name:'',
        keywords: '',
        bid: this.formMinValues.bid,
        status: true,
        city: 'Cracow',
        radius: this.formMinValues.radius

      }
    },

    openPopup(){
      this.popupMode = 'add'
      document.querySelector('#popup').style.display="flex";
    },

 

  },

  computed:{
    avaliableFund(){
      return this.fund - this.campaigns.reduce((prev,curr) => curr.status ? prev+parseInt(curr.bid) : prev , 0);
    },
    bidSumActive(){
      return this.campaigns.reduce((prev,curr) => curr.status ? prev+parseInt(curr.bid) : prev , 0);
    },
    bidSum(){
      return this.campaigns.reduce((prev,curr) => prev+parseInt(curr.bid), 0);
    }
  },

  
}
</script>

<style lang="sass">

</style>
