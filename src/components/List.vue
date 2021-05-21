<template>
    <div class="container table">
        <ItemHeading/>
        <div v-for="campaign in filteredCampaigns " :key="campaign.id" :class="[campaign.status ? 'active' : 'disabled' ,'item']" data-aos="fade-up" >

            <Item :campaign="campaign" @deleteItem="this.$emit('deleteItem',campaign.id)" @openEditPopup="()=>this.$emit('openEditPopup',campaign)" />
            
        </div>
    </div>
</template>

<script>
import Item from '../components/Item'
import ItemHeading from '../components/ItemHeading'
import AOS from 'aos';
import 'aos/dist/aos.css'; 


export default {
    name: 'List',
    emits:['deleteItem','openEditPopup'],
    components: {
        Item,
       ItemHeading
    },
    props: {
        campaigns: Array,
        deleteItem: Function,
        openEditPopup: Function,

    },
    mounted: function(){
        AOS.init({
            offset: 50
        });
    },
    computed: {
        filteredCampaigns: function(){
            return this.campaigns.sort((a,b) => {
                
                  if ( a.status > b.status ){
                        return -1;
                    }
                    if ( a.status < b.status ){
                        return 1;
                    }
                    return 0;
            })
        }
    }
}
</script>

<style>
</style>