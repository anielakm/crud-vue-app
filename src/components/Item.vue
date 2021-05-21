<template>

    <div class="name">{{campaign.name}}</div>
    <div class="keywords" >
        <span v-for="(keyword,index) in campaign.keywords" :key="index">{{keyword}}</span>
    </div>
    <div class="bid"><span class="mobile">Bid: </span>{{campaign.bid.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}} zł</div>
    <div class="status"><span class="mobile">Status: </span><span v-if="campaign.status">active</span><span v-else>inactive</span></div>
    <div class="city"><span class="mobile">City: </span>{{campaign.city}}</div>
    <div class="radius"><span class="mobile">Radius: </span>{{campaign.radius}} km</div>
    <div class="btns"><button class="btn" @click="()=>this.$emit('openEditPopup',campaign.id)">edit</button> <button class="btn" @click="()=>this.$emit('deleteItem',campaign.id)">delete</button></div>

</template>

<script>

export default {
    name: 'Item',
    emits:['openEditPopup','deleteItem'],
    props: {
        campaign: Object,
        deleteItem: Function,
        openEditPopup: Function,
    },
  
}
</script>

<style lang="sass">
.mobile
    @media (min-width: 768px)
        display: none
.name
    font-weight: 500
    @media (max-width: 767.9px)
        text-align: center
        font-size: 22px
       
.keywords
    display: flex
    align-items: flex-start
    flex-wrap: wrap
    gap: 7px
    @media (max-width: 767.9px)
        width: 100%
        justify-content: center
        padding-bottom: 20px !important
    >span, >div
        background-color: var(--gray-light-2)
        padding: 4px 7px
        text-transform: uppercase
        font-size: 12px
        letter-spacing: 1px
        border-radius: 3px
        width: min-content
        @media (min-width: 992px), (max-width: 519.9px)
            white-space: nowrap
.bid
    font-size: 14px
    white-space: nowrap
    @media (min-width: 1200px)
        font-size: 16px
.btns
    display: flex
    justify-content: space-between
    flex-wrap: wrap
    @media (max-width: 767.9px)
        width: 100%
        margin-top: 10px
    button
        width: 100%
        max-width: 100%
        padding: 7px
        @media (max-width: 991.9px)
            &:first-child
                margin-bottom: 7px
        @media (max-width: 767.9px)
            width: 48%
            margin-bottom: 0 !important
        @media (min-width: 992px)
            width: 48%

</style>