<template>
     <div class="item item--heading" >
        <div>Campaign name</div>
        <div>Keywords</div>
        <div>Bid</div>
        <div>Status</div>
        <div>City</div>
        <div>Radius</div>
        <div>Edit/delete</div>
    </div>
</template>

<script>

export default ({
   name:'ItemHeading'
})
</script>

<style scoped lang="sass">
.item--heading
    background-color: var(--black)
    color: var(--white)
    @media (max-width: 767.9px)
        display: none !important
    div
        font-weight: 800
        text-transform: uppercase
        font-size: 12px
        letter-spacing: 1px
        @media (min-width: 992px)
            font-size: 14px
</style>