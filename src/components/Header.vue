<template>
  <header>
    <h1><a href="/">Bookman 図書管理</a></h1>
    <div id="form">
      <input type="text" placeholder="Search" class="text" v-model="keyword" @change="search">
      <div class="selectWrap">
       <select class="select" v-model="selected" @change="search">
         <option value="" :key="-1">Category</option>
         <option v-for="category of categoryList" :value="category.id" :key="category.id">{{ category.name }}</option>
       </select>
      </div>
    </div>
  </header>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
import { UPDATE_CATEGORIES, UPDATE_BOOKS } from '../store/mutation-types'
export default {
  name: 'Header',
  data () {
    return {
      keyword: '',
      selected: ''
    }
  },
  created () {
    this.$http(this.$httpCategories)
      .then(response => {
        return response.json()
      })
      .then(data => {
        this[UPDATE_CATEGORIES](data)
      })
  },
  computed: {
    ...mapGetters(['categoryList'])
  },
  methods: {
    ...mapActions([UPDATE_CATEGORIES, UPDATE_BOOKS]),
    search () {
      this.$http(`${this.$httpBooks}?keyword=${this.keyword}&category=${this.selected}`)
        .then(response => {
          return response.json()
        })
        .then(data => {
          this[UPDATE_BOOKS](data)
        })
    }
  }
}
</script>

<style scoped>
  header {
    background-color: #000;
    color: #fff;
    height: 50px;
    display: grid;
    grid-template-columns: 20px 1fr 20px;
    grid-template-rows: 1fr;
    margin-bottom: 80px;
  }
  header > * {
    grid-row: 1;
    grid-column: 2;
  }
  h1 {
    justify-self: start;
    align-self: center;
    font-size: 20px;
    font-weight: normal;
  }
  h1 > a {
    color: #fff;
    text-decoration: none;
  }
  #form {
    justify-self: end;
    align-self: center;
    display: none;
  }
  .text {
    border-bottom: solid 1px #ccc;
    border-right: none;
    border-top: none;
    border-left: none;
    background-color: transparent;
    color: #fff;
    width: 200px;
    margin-left: 20px;
    padding-left: 6px;
    padding-bottom: 1px;
    font-family: fot-tsukuardgothic-std, sans-serif;
  }
  .selectWrap {
    margin-left: 20px;
    width: 150px;
    position: relative;
    display: inline-block;
  }
  .selectWrap::after {
    content: '';
    width: 6px;
    height: 6px;
    border: 0;
    border-bottom: solid 2px #ccc;
    border-right: solid 2px #ccc;
    transform: rotate(45deg);
    position: absolute;
    top: 50%;
    right: 10px;
    margin-top: -4px;
  }
  .select {
    appearance: none;
    border-bottom: solid 1px #ccc;
    border-right: none;
    border-top: none;
    border-left: none;
    background-color: transparent;
    color: #fff;
    width: 100%;
    font-family: fot-tsukuardgothic-std, sans-serif;
  }
  .select > option {
    color: black;
  }
  ::placeholder {
      color: #fff;
      opacity: 1;
      font-family: fot-tsukuardgothic-std, sans-serif;
  }
  @media (min-width: 768px) {
    header {
      grid-template-columns: 1fr 700px 1fr;
    }

    #form {
      display: block;
    }
  }
  @media (min-width: 1024px) {
    header {
      grid-template-columns: 1fr 980px 1fr;
    }
  }
</style>
