<template>
    <div class="popup">

        <div class="popup__bg" @click="()=>{handleClose()}"></div>
        
        <form class="popup__container" >

            <div class="popup__close" @click="(e)=>{e.preventDefault(); handleClose()}">
               <svg height="329pt" viewBox="0 0 329.26933 329" width="329pt" xmlns="http://www.w3.org/2000/svg"><path d="m194.800781 164.769531 128.210938-128.214843c8.34375-8.339844 8.34375-21.824219 0-30.164063-8.339844-8.339844-21.824219-8.339844-30.164063 0l-128.214844 128.214844-128.210937-128.214844c-8.34375-8.339844-21.824219-8.339844-30.164063 0-8.34375 8.339844-8.34375 21.824219 0 30.164063l128.210938 128.214843-128.210938 128.214844c-8.34375 8.339844-8.34375 21.824219 0 30.164063 4.15625 4.160156 9.621094 6.25 15.082032 6.25 5.460937 0 10.921875-2.089844 15.082031-6.25l128.210937-128.214844 128.214844 128.214844c4.160156 4.160156 9.621094 6.25 15.082032 6.25 5.460937 0 10.921874-2.089844 15.082031-6.25 8.34375-8.339844 8.34375-21.824219 0-30.164063zm0 0"/></svg>
            </div>

            <div class="popup__content">

                <p v-if="popupMode === 'add'" class="popup__heading">Add new product campaign</p>
                <p v-else class="popup__heading">Edit product campaign</p>
                <p class="popup__info"><small>* all fields are required</small></p>

                <div class="input-container" id="name">
                    <input type="text" name="name" placeholder="add company name" v-model="modelValue.name" required>
                </div>
                
                <div class="input-container" id="keywords">
                    <input type="text" placeholder="add keywords" v-on:keydown.enter.prevent='updateKeywords($event.target.value)' :value='keywordValue' >
                </div>
                
                <div v-if="keywords.length !== 0" class="keywords key">
                    <div v-for="(keyword, key) in keywords" :key="key" >
                        <span>{{keyword}} <span class="delete" @click="()=>deleteKeyword(keyword)">x</span></span>
                    </div>
                </div>

                <div v-else class="keywords">
                    <div v-for="(keyword, key) in modelValue.keywords" :key="key" >
                        <span>{{keyword}} <span class="delete" @click="()=>deleteKeyword(keyword)">x</span></span>
                    </div>
                </div>

                <div :class="[modelValue.bid >= minValues.bid ? null : 'error', 'input-container']" id="bid">
                    <input type="number"  placeholder="campaign bid" v-model="modelValue.bid" :min="minValues.bid">
                    <span class="error-text">Value must be grater than {{minValues.bid}}</span>
                </div>

                <div class="input-container" id="city">
                    <select name="" v-model="modelValue.city">
                        <option value="Cracow">Cracow</option>
                        <option value="Warsaw">Warsaw</option>
                        <option value="Wroclaw">Wroclaw</option>
                        <option value="Poznan">Poznan</option>
                        <option value="Szczecin">Szczecin</option>
                        <option value="Katowice">Katowice</option>
                        <option value="Gdansk">Gdansk</option>
                        <option value="Gdynia">Gdynia</option>
                        <option value="Sopot">Sopot</option>
                    </select>
                </div>

                <div :class="[modelValue.radius >= minValues.radius ? null : 'error', 'input-container']" id="radius" >
                    <input type="number"  placeholder="campaign radius" v-model="modelValue.radius" :min="minValues.radius">
                    <span class="error-text">Value must be grater than {{minValues.radius}}</span>
                </div>

                <div class="input-container" id="checkbox">
                    <p>Status: </p>
                    <label :class="[modelValue.status ? 'active' : '','chbx']">
                        <input type="checkbox" id="status" placeholder="status" v-model="modelValue.status">
                        <span class="chbx-heading" v-if="modelValue.status">active</span>
                        <span class="chbx-heading" v-else>inactive</span>
                    </label>
                </div>         

                <div class="popup__btns">

                    <input v-if="popupMode === 'add'" type="submit" class="btn" value="add" :disabled="!(modelValue.bid >= minValues.bid && modelValue.radius >= minValues.radius && modelValue.name !== '' && (modelValue.keywords.length !==0 || keywords.length!==0))"
                        @click="(e)=>{
                            e.preventDefault(); handleAdd()
                        
                        }"
                    />

                    <input v-else type="submit" class="update btn" @click="(e)=>{e.preventDefault(); handleUpdate()}" value="update">
        
                </div>

            </div>
           
       </form>
    </div>
</template>

<script>
export default {
    name: 'Popup',
    emits:['close','update','add'],
    data(){
        return{
            keywords: [],
            keywordValue: ''
        }
    },
    props: {
        add: Function,
        close: Function,
        update: Function,
        popupMode: String,

        modelValue: Object,
        minValues: Object
    },

     watch: {
        modelValue() {
            this.$emit('update:modelValue', this.modelValue);
        }
    },
   
    methods: {
        updateKeywords(value){
            this.keywords.length === 0 ? this.keywords = [...this.modelValue.keywords] : null;
            this.keywords.push(value)
            //clear input
            value = '';
        },

        deleteKeyword(value){
            
            this.keywords.length === 0 ? this.keywords = [...this.modelValue.keywords] : null;
            this.keywords.splice(this.keywords.indexOf(value),1);
        },

        handleClose(){
            this.keywords = [];
            this.$emit('close')
        },

        handleUpdate(){
            this.keywords.length === 0 ? null : this.modelValue.keywords = [...this.keywords]
            this.keywords = [];
            this.$emit('update')
        },
        handleAdd(){
            this.modelValue.keywords = [...this.keywords]
            this.keywords = [];
            this.$emit('add')
        }
       
    }
   
   
}
</script>

<style scoped lang="sass">
.popup 
    display: none
    align-items: center
    justify-content: center
    position: fixed
    top: 0
    left: 0
    width: 100%
    height: 100%

    &__info
        width: 100%
        margin: 0 0 5px
        text-align: right
        small
            font-size: .7em

    &__bg
        background-color: var(--black)
        position: absolute
        top: 0
        left: 0
        width: 100%
        height: 100%
        opacity: .8
        cursor: pointer

    &__heading
        text-align: center
        width: 100%
        margin-bottom: 40px
        font-size: 20px
        text-transform: uppercase
        padding: 0 30px
    
    &__container 
        width: 100% !important
        height: 100%
        max-height: 100vh
        background-color: var(--white)
        display: flex
        flex-wrap: wrap
        justify-content: center
        align-items: center
        padding: 20px
        z-index: 3
        border-radius: 0
        position: relative
        overflow: scroll
        @media (min-width: 520px)
            max-width: 650px
            width: 90% !important
            height: fit-content
            max-height: 90vh
            border-radius: 15px
    &__close
        position: absolute
        background-color: var(--black)
        padding: 10px
        border-radius: 50%
        width: 30px
        height: 30px
        display: flex
        align-items: center
        justify-content: center
        top: 10px
        right: 10px
        cursor: pointer
        svg
            fill: var(--white)
            width: 100%

    &__content
        display: flex
        flex-wrap: wrap
        justify-content: space-between

        .input-container
            position: relative
            margin-bottom: 20px
            width: 100%
            > span
                display: none
            &.error
                input
                    border-color: red
                > span
                    display: block
                    font-size: 12px
                    height: 18px
                    padding-top: 20px
                    margin-top: -18px
                    color: red

            &#name:after
                content: 'name'
                position: absolute
                top: -10px
                left: 10px
                display: block
                background-color: var(--white)
                padding: 3px 5px
                text-transform: uppercase
                font-size: 11px
                letter-spacing: 1px
            &#keywords:after
                content: 'keywords'
                position: absolute
                top: -10px
                left: 10px
                display: block
                background-color: var(--white)
                padding: 3px 5px
                text-transform: uppercase
                font-size: 11px
                letter-spacing: 1px
            &#bid:after
                content: 'bid (zł)'
                position: absolute
                top: -10px
                left: 10px
                display: block
                background-color: var(--white)
                padding: 3px 5px
                text-transform: uppercase
                font-size: 11px
                letter-spacing: 1px
            &#city:after
                content: 'city'
                position: absolute
                top: -10px
                left: 10px
                display: block
                background-color: var(--white)
                padding: 3px 5px
                text-transform: uppercase
                font-size: 11px
                letter-spacing: 1px
            &#radius:after
                content: 'radius (km)'
                position: absolute
                top: -10px
                left: 10px
                display: block
                background-color: var(--white)
                padding: 3px 5px
                text-transform: uppercase
                font-size: 11px
                letter-spacing: 1px

                
        input, select
            width: 100%
            padding: 15px
            outline: none
            border: 1px solid var(--gray-light-3)
            &:focus, &:active
                border: 1px solid var(--black)
        .keywords 
            width: 100%
            justify-content: flex-start
            div:last-child
                margin-bottom: 30px
            .delete
                cursor: pointer
        #bid, #city, #radius
            @media (min-width: 520px)
                width: 32%
        #checkbox
            display: flex
            flex-wrap: wrap
            margin-bottom: 40px
            p
                width: 100%
                margin-top: 0
                margin-bottom: 10px
            .chbx
                width: 20px
                height: 20px
                margin-right: 10px
                position: relative
                cursor: pointer
                display: flex
                align-items: center
            
                &:after
                    content: ''
                    position: absolute
                    top: 0
                    left: 0
                    width: 100%
                    height: 100%
                    border: 1px solid var(--black)
                    box-sizing: border-box
                &:before
                    content: ''
                    width: 14px
                    height: 14px
                    display: block
                    top: 0
                    left: 0
                    right: 0
                    bottom: 0
                    margin: auto
                    position: absolute
                
                input
                    opacity: 0
                    cursor: pointer
                    height: 0
                    width: 0
                .chbx-heading
                    cursor: pointer
                    text-transform: uppercase
                    font-size: 12px
                    letter-spacing: 1px
                    margin-left: 23px
                &.active:before
                    background-color: var(--black)
        .popup__btns
            display: flex
            justify-content: center
            flex-wrap: nowrap
            width: 100%

</style>