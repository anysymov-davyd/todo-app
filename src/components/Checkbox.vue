<template>
    <div class="checkbox__general">
        <div :style="{ 'padding-left' : isChild ? '24px' : 0, 'width' : isChild ? 'calc(100% - 24px)' : '100%' }" class="checkbox__container">
            <label class="container">
                <input class="checkbox__rawinput" @change="checkboxEvent" ref="checkmark" type="checkbox" :checked="isChecked">
                <span v-if="!isChanging" class="checkbox__value">{{ value }}</span>
                <input :class="{ 'font-dark' : darkMode, 'font-light': !darkMode }" v-on:keyup.enter="changeTask" v-else ref="changeInput" placeholder="Enter a new name" class="checkbox__input" v-model="changeInputValue"/>
                <span class="checkmark"/>
            </label>
            
            <div :class="{'--not_transparent' : isAdding}" v-if="!isChanging" class="checkbox__icons --transparent">
                <IconButton v-if="!isAdding && !isDone && !isChild" :iconName="'add'" :darkMode="darkMode" @click="switchAddTask"/>
                <IconButton v-if="isAdding && !isDone && !isChild" :iconName="'cancel'" :darkMode="darkMode" @click="switchAddTask"/>
                <IconButton :iconName="'edit'" :darkMode="darkMode" @click="switchChangeTask"/>
                <IconButton :iconName="'delete'" :darkMode="darkMode" @click="deleteTask"/>
            </div>
            <div v-else class="checkbox__icons">
                <IconButton :iconName="'apply'" :darkMode="darkMode" @click="changeTask"/>
                <IconButton :iconName="'cancel'" :darkMode="darkMode" @click="switchChangeTask"/>
            </div>
        </div>
        <div v-if="isAdding && !isDone" class="checkbox__add-input-container">
            <input :class="{ 'font-dark' : darkMode, 'font-light' : !darkMode }" v-on:keyup.enter="addSubTask" ref="subtask_input" placeholder="Enter the subtask" class="checkbox__add-input"/>
            <svg class="checkbox__add-button" @click='addSubTask' :style="{ fill: darkMode ? 'black' : 'white' }" width="10" height="10" viewBox="0 0 10 10">
                <g clip-path="url(#clip0_8_2)">
                    <path d="M10 5.71429H5.71429V10H4.28571V5.71429H0V4.28571H4.28571V0H5.71429V4.28571H10V5.71429Z"/>
                </g>
                <defs>
                    <clipPath id="clip0_8_2">
                        <rect width="10" height="10" fill="white"/>
                    </clipPath>
                </defs>
            </svg>
        </div>
    </div>
</template>

<script>
import IconButton from './IconButton.vue'

export default {
  components: {
    IconButton
  },
  props: {
    darkMode: Boolean,
    value: String,
    isChecked: Boolean,
    unique: String,
    index: Number,
    parentId: String
  },
  data(){
    return{
        isChanging: false,
        isAdding: false,
        changeInputValue: this.value,
        isDone: this.isChecked,
        isChild: this.parentId !== undefined
    }
  },
  methods: {
    deleteTask(){
        this.$emit('deleteTask', {
            'id': this.unique,
            'index': this.index
        })
    },
    checkboxEvent(){
        this.$emit('checkTask', {
            'name': this.value,
            'isDone': this.$refs.checkmark.checked,
            'parentId': this.parentId,
            'id': this.unique,
            'index': this.index
        })
        this.isDone = this.$refs.checkmark.checked
        this.isAdding = false
    },
    switchChangeTask(){
        this.isChanging = !this.isChanging
        this.isAdding = false
    },
    switchAddTask(){
        this.isAdding = !this.isAdding
    },
    changeTask(){
        if(this.$refs.changeInput.value.trim().length > 0){
            this.$emit('changeTask', {
                'name': this.$refs.changeInput.value,
                'isDone': this.$refs.checkmark.checked,
                'parentId': this.parentId,
                'id': this.unique,
                'index': this.index
            })
            this.changeInputValue = this.$refs.changeInput.value
            this.isChanging = false
        }
    },
    addSubTask(){
        if(this.$refs.subtask_input.value.trim().length > 0 && !this.isChild){
            this.isAdding = false
            this.$emit('addSubTask', {
                'name': this.$refs.subtask_input.value,
                'isDone': false,
                'parentId': this.parentId,
                'id': this.unique,
                'index': this.index
            })
        }
        this.$refs.subtask_input.value = ''
    }
  }
}
</script>

<style>
    .checkbox__general{
        width: 100%;
        display: flex;
        flex-direction: column;
        gap: 8px;
    }

    .checkbox__add-input-container{
        width: 100%;
        display: flex;
        justify-content: space-between;
        border: 1px solid rgb(180, 180, 180);
        border-radius: 6px;
    }

    .checkbox__add-input{
        width: 100%;
        border-radius: 6px;
        border: 0;
        padding: 0 0 0 14px;
        height: 30px;
        font-size: 14px;
    }

    .checkbox__add-button{
        padding: 10px; 
        min-width: 10px;
        height: 10px;
        background: url('./../assets/add.svg') no-repeat center;
        opacity: .5;
        cursor: pointer;
        transition: .2s;
    }

    .checkbox__add-button:hover{
        background-color: rgba(180, 180, 180, .25);
    }

    .checkbox__add-button:active{
        background-color: rgba(180, 180, 180, .4);
    }

    .checkbox__value{
        display: block;
        line-height: 15px;
        text-overflow: ellipsis;
        overflow: hidden;
        margin-top: 5px;
    }

    .checkbox__container{
        width: 100%;
        display: flex;
        justify-content: space-between;
        gap: 48px;
    }

    .checkbox__container:hover > .checkbox__icons{
        opacity: 1;
    }

    .container {
        display: block;
        position: relative;
        padding-left: 35px;
        font-size: 16px;
        -webkit-user-select: none;
        user-select: none;
        word-wrap: break-word;
    }

    .container .checkbox__rawinput {
        position: absolute;
        opacity: 0;
        height: 0;
        width: 0;
    }

    .checkmark {
        transition: .2s;
        position: absolute;
        top: 0;
        left: 0;
        height: 22.5px;
        width: 22.5px;
        border: 1.5px solid #B4B4B4;
        border-radius: 6px;
        background-color: transparent;
    }

    .container input:checked ~ .checkmark {
        background-color: #29ABE2;
        border: 1.5px solid transparent;
    }

    .container input:checked + span {
        text-decoration: line-through;
    }

    .checkmark:after {
        content: "";
        position: absolute;
        display: none;
        left: 4px;
        top: 6px;
        border: 0 solid white;
        background: url('./../assets/checkmark.svg') no-repeat;
        width: 13.71px;
        height: 12px;
    }

    .container input:checked ~ .checkmark:after {
        display: block;
    }

    .checkbox__icons{
        display: inline-flex;
        gap: 5px;
        transition: .2s;
        margin: auto 0;
    }

    .checkbox__input{
        border-radius: 6px;
        font-size: 16px;
        resize: none;
        font-family: RubikRegular;
        overflow: hidden;
        outline: none;
        width: 178px;
        padding: 0;
        padding-top: 2px;
        padding-left: 10px;
        border: 1px solid #B4B4B4;
        height: 20.5px;
    }

    .checkbox__delete{
        background: url('./../assets/clear.svg') no-repeat center;
    }

    .checkbox__edit{
        background: url('./../assets/edit.svg') no-repeat center;
    }

    .checkbox__apply{
        background: url('./../assets/apply.svg') no-repeat center;
        background-color: rgba(0, 0, 0, 0.1);
    }

    .checkbox__cancel{
        background: url('./../assets/cancel.svg') no-repeat center;
        background-color: rgba(0, 0, 0, 0.1);
    }

    .checkbox__add{
        background: url('./../assets/add.svg') no-repeat center;
    }
</style>