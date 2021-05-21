<template>
    <div class="bar container">
        <button class="btn" @click="()=>open()">Add item</button>
        <div class="bar__summary">
            <ul>
                <li><p>Total fund:</p> <span>{{fund.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}}</span></li>
                <!-- <li><p>Bid sum :</p> <span>{{bidSum.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}}</span></li> -->
                <li><p>Bid sum : <small>* only active campaigns</small></p> <span>{{bidSumActive.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}}</span></li>
                <li><p>Avaliable fund:</p><span>{{avaliableFund.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}}</span></li>
            </ul>
        </div>
    </div>
</template>

<script>
export default ({
    name:'Summary',
    props: {
        bidSum: Number,
        avaliableFund: Number,
        fund: Number,
        bidSumActive: Number,
        openPopup: Function,
    },
    methods: {
      open() {
        this.$emit('openPopup')
      }
    }
    
})
</script>

<style scoped lang="sass">
.bar
    display: flex
    align-items:  flex-start
    justify-content: space-between
    flex-wrap: wrap
    width: 100%
    margin-top: 30px
    margin-bottom: 40px
    padding: 0 15px
    button
        @media (max-width: 767.9px)
            order: 1
            max-width: 100%
            width: 100%
            margin-top: 30px
            font-size: 18px
            letter-spacing: 2px
    &__summary
        max-width: 350px
        width: 100%
        
        @media (max-width: 767.9px)
            margin: 10px 0 20px
            max-width: 100%
            
    ul
        list-style: none
        padding: 0
        margin: 0
        text-align: right
        font-size: 13px
        letter-spacing: 1px
        text-transform: uppercase
        
        width: 100%
        li
            display: flex
            justify-content: space-between
            align-items: center
            text-align: right
            margin-bottom: 10px
            p
                margin: 0
                width: 60%
                display: flex
                flex-direction: column
                small
                    text-transform: lowercase
            span
                font-weight: 500
                width: 40%
                font-size: 1.3em
            &:last-child
                font-size: 15px
                border-top: 1px solid var(--gray-light-2)
                padding-top: 15px
                margin-top: 15px
</style>