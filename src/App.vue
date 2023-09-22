<template>
  <div :class="{ 'card-dark' : !darkMode, 'card-light' : darkMode }" class='todo__container'>
    <div class='todo__item'>
      <h3 :class="{ 'font-light' : !darkMode, 'font-dark' : darkMode}" class='todo__caption'>Things to do</h3>
      <Switch
        :darkMode="darkMode"
        @click="handleSwitch"
      />
      <div :class="{ 'font-light' : !darkMode, 'font-dark' : darkMode }" @click='isHidingCompleted = !isHidingCompleted' class='todo__hide'>{{ isHidingCompleted ? 'Show' : 'Hide' }} completed tasks</div>
    </div>
    <div class='todo__input todo__item'>
      <Input
        v-on:keyup.enter="addTask"
        ref="inputValue"
        @inputValue="handleInputValue"
        :darkMode="darkMode"
      />
      <Button
        @click="addTask"
        :darkMode="darkMode"
      />
    </div>
    <div v-if='items.length > 0'>
      <div
        v-for='(item, index) in items'
        :key="item.id"
        :class="{ 'font-light' : !darkMode, 'font-dark' : darkMode }"
      >
        <div v-if='(!isHidingCompleted || !item.isDone) && item.parentId === undefined' class='todo__item'>
          <Checkbox
            @changeTask="handleChangeTask"
            @checkTask="handleCheckTask"
            @deleteTask="handleDeleteTask"
            @addSubTask="handleAddSubTask"
            :index='index'
            :unique='item.id'
            :value='item.name'
            :isChecked='item.isDone'
            :darkMode="darkMode"
          />
        </div>
        <div v-if="!isHidingCompleted || !item.isDone" class="todo__sublist" v-for="(child, index) in items.filter(task => task.parentId === item.id)">
          <Checkbox
            @changeTask="handleChangeTask"
            @checkTask="handleCheckTask"
            @deleteTask="handleDeleteTask"
            :index="index"
            :unique="child.id"
            :value="child.name"
            :isChecked="child.isDone"
            :parentId="child.parentId"
            :darkMode="darkMode"
          />
        </div>
      </div>
    </div>
    <div v-if='items.length == 0 || (items.every(task => task.isDone === true) && isHidingCompleted)' class='todo__notasks' style='margin-top: 8px'>
      <a :class="{ 'font-dark' : darkMode, 'font-light' : !darkMode}" v-if='isLoading'>Loading...</a>
      <a :class="{ 'font-dark' : darkMode, 'font-light' : !darkMode}" v-else>You have no tasks</a>
    </div>
  </div>
</template>

<script>
// Import libraries and components
import { initializeApp } from 'firebase/app'
import { getFirestore, collection, getDocs, deleteDoc, doc, setDoc } from 'firebase/firestore'
import firebaseConfig from './db_config'

import Switch from './components/Switch.vue'
import Checkbox from './components/Checkbox.vue'
import Button from './components/Button.vue'
import Input from './components/Input.vue'

// Database connection
const app = initializeApp(firebaseConfig)
const db = getFirestore(app)


export default {
  components: {
    Checkbox,
    Button,
    Input,
    Switch
  },
  data() {
    return {
      items: [],
      isLoading: true,
      inputValue: '',
      isHidingCompleted: false,
      darkMode: false
    }
  },
  mounted(){
    // Generate the list, when the app has started
    this.fetchData()
    // Find out which theme was choosed before the start
    this.darkMode = localStorage.getItem('darkMode') === 'false' ? false : true
    document.body.style.backgroundColor = this.darkMode ? '#333333' : '#F5F5F5'
  },
  methods: {
    // Generate a random identificator
    generateRandomID() {
      const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
      let result = ''
      for (let i = 0; i < 20; i++) {
        const randomIndex = Math.floor(Math.random() * characters.length)
        result += characters.charAt(randomIndex)
      }
      return result
    },
    // Get data from the database
    fetchData(){
      function findChildren(id, arr){
        return arr.filter(item => item.parentId === id)
      }
      getDocs(collection(db, 'tasks'))
      .then((querySnapshot) => {
        let list = []
        querySnapshot.forEach((doc) => {
          list.push({
            name: doc.data().name,
            isDone: doc.data().isDone,
            parentId: doc.data().parentId,
            id: doc.id
          })
        })
        this.items = []
        for(const item of list){
          if(item.parentId === undefined){
            this.items.push(item)
            const children = findChildren(item.id, list)
            for(const subitem of children){
              this.items.push(subitem)
            }
          }
        }
        this.isLoading = false

        // Delete useless data
        list.filter(item => !this.items.some(item2 => item2.id === item.id)).forEach(elem => {
          deleteDoc(doc(db, 'tasks', elem.id))
        })
      })
      .catch((error) => {
        console.error(error)
      })
    },
    // Add a new task and refresh the list
    addTask(){
      this.$refs.inputValue.clearInputValue()
      if(this.inputValue.trim().length > 0 && this.items.length < 30){
        this.fetchData()
        const newId = this.generateRandomID()
        const docRef = doc(db, 'tasks', newId)
        setDoc(docRef, {
          name: this.inputValue,
          isDone: false,
          id: newId
        })
      }
    },
    // Event, when the task is getting checked or unchecked. Store the change in the database and refresh the list
    handleCheckTask(value){
      this.fetchData()
      if(value.parentId){
        setDoc(doc(db, 'tasks', value.id), {
          name: value.name,
          isDone: value.isDone,
          parentId: value.parentId
        })
      } else{
        setDoc(doc(db, 'tasks', value.id), {
          name: value.name,
          isDone: value.isDone
        })
        if(value.isDone){
          this.items.filter(task => task.parentId === value.id).forEach(item => {
            item.isDone = true
            setDoc(doc(db, 'tasks', item.id), {
              name: item.name,
              isDone: true,
              parentId: item.parentId
            })
          })
        }
      }
    },
    // Watch the value of the input and refresh, when it's getting changed
    handleInputValue(value){
      this.inputValue = value
    },
    // Delete the choosed task and refresh the list
    handleDeleteTask(value){
      this.fetchData()
      deleteDoc(doc(db, 'tasks', value.id))
    },
    // Change the value of the choosed input and refresh the list
    handleChangeTask(value){
      this.fetchData()
      if(value.parentId){
        setDoc(doc(db, 'tasks', value.id), {
          name: value.name,
          isDone: value.isDone,
          parentId: value.parentId
        })
      } else{
        setDoc(doc(db, 'tasks', value.id), {
          name: value.name,
          isDone: value.isDone
        })
      }
    },
    // Add a new subtask for the choosed task and refresh the list
    handleAddSubTask(value){
      if(this.items.filter(elem => elem.parentId === value.id).length < 30) {
        this.fetchData()
        const newId = this.generateRandomID()
        const docRef = doc(db, 'tasks', newId)
        setDoc(docRef, {
          parentId: value.id,
          id: newId,
          name: value.name,
          isDone: false
        })
      }
    },
    // A event, when user switches the theme color
    handleSwitch(){
      localStorage.setItem('darkMode', !this.darkMode)
      this.darkMode = !this.darkMode
      document.body.style.backgroundColor = this.darkMode ? '#333333' : '#F5F5F5'
    }
  }
}
</script>