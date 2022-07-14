<template>
  <div id="app">
    <header class="head">
      <div>
        <h1 class="form__description">Добавление товара</h1>
      </div>
      <div>
        <my-select
                v-model="itemsSort"
                :options="sortOptions"
        />
      </div>
    </header>
    <main class="main">
      <section class="main__form">
        <my-form
                :formData="formData"
                @create="createItem"
        />
      </section>
      <section class="main__items__grid">
        <transition-group name="item">
        <my-item
                v-for="item in items"
                :key="item.id"
                :item="item"
                @remove="removeItem(item)"
        />
        </transition-group>
      </section>
    </main>
  </div>
</template>
<script>
import MyForm from '@/components/MyForm.vue'
import MySelect from '@/components/MySelect.vue'
import MyItem from '@/components/MyItem.vue'

export default {
    name: 'App',
    components: {
        MyForm,
        MySelect,
        MyItem
    },
    data() {
        return {
            items: [
                {
                    id: 0,
                    name: 'Название',
                    description: 'Описание',
                    link: 'https://img.freepik.com/premium-photo/beautiful-moraine-lake-in-banff-national-park-alberta-canada_131985-98.jpg?w=2000',
                    price: '5000'
                },
                {
                    id: 1,
                    name: 'Название',
                    description: 'Описание',
                    link: 'https://img.freepik.com/premium-photo/beautiful-moraine-lake-in-banff-national-park-alberta-canada_131985-98.jpg?w=2000',
                    price: '5000'
                },
                {
                    id: 2,
                    name: 'Название',
                    description: 'Описание',
                    link: 'https://img.freepik.com/premium-photo/beautiful-moraine-lake-in-banff-national-park-alberta-canada_131985-98.jpg?w=2000',
                    price: '5000'
                },
                {
                    id: 3,
                    name: 'Название',
                    description: 'Описание',
                    link: 'https://img.freepik.com/premium-photo/beautiful-moraine-lake-in-banff-national-park-alberta-canada_131985-98.jpg?w=2000',
                    price: '5000'
                },
                {
                    id: 4,
                    name: 'Название',
                    description: 'Описание',
                    link: 'https://img.freepik.com/premium-photo/beautiful-moraine-lake-in-banff-national-park-alberta-canada_131985-98.jpg?w=2000',
                    price: '5000'
                },
                {
                    id: 5,
                    name: 'Название',
                    description: 'Описание',
                    link: 'https://img.freepik.com/premium-photo/beautiful-moraine-lake-in-banff-national-park-alberta-canada_131985-98.jpg?w=2000',
                    price: '5000'
                },
            ],
            formData: {
                name: '',
                description: '',
                link: '',
                price: '',
            },
            itemsSum: '',
            itemsSort: '',
            sortOptions: [
                {value: 'min', name: 'По возростанию'},
                {value: 'max', name: 'По убыванию'},
                {value: 'name', name: 'По названию'},
            ]
        }
    },
    methods: {
        createItem() {
            //console.log(this.formData.price)
            this.formData.id = Date.now();
            this.items.push(this.formData);
            localStorage.setItem('items', JSON.stringify(this.items));
            this.formData = {
                name: '',
                description: '',
                link: '',
                price: '',
            }
        },
        removeItem(item) {
            this.items = this.items.filter(p => p.id !== item.id);
            localStorage.setItem('items', JSON.stringify(this.items));
        }
    },
    beforeMount() {
        if (localStorage.getItem('items')) {
            let m = JSON.parse(localStorage.items);
            let b = [];
            for (let i = 0; i <= m.length; i++) {
                if (m[i] != null && m[i] != undefined && m[i] != '') {
                    b.push(m[i]);
                }
            }
            this.items = b;
        }
    },
    watch: {
        itemsSort(newValue) {
            if (newValue === 'min'){
                return this.items.sort((a, b) => a.price - b.price );
            }
            if (newValue === 'max'){
                return this.items.sort((a, b) => b.price - a.price );
            }
            if (newValue === 'name'){
                return this.items.sort((a,b) => a.name.localeCompare(b.name))
              }
            if (newValue === ''){
                return this.items.sort((a, b) => a.id - b.id );
            }
            }
        }


}
</script>

<style lang="sass" scoped>
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@200;300;400;600;700;900&display=swap')
*
  box-sizing: border-box
  font-family: 'Source Sans Pro', sans-serif !important

#app
  padding: 24px 32px 24px 32px
  width: 100vw
  background: #faf9f7

input, textarea, select
  -moz-appearance: none
  -webkit-appearance: none
  appearance: none
.head
  display: flex
  flex-direction: row
  margin-bottom: 16px
  justify-content: space-between

.form__description
    font-style: normal
    font-weight: 600
    font-size: 28px
    line-height: 35px
    color: #3F3F3F
    margin: 0
    padding: 0
.item-enter-active,.item-leave-active
  transition: all 1s ease
.item-enter-from,.item-leave-to
  opacity: 0
  transform: translateX(30px)
.item-move
  transition: 0.4s ease
@media screen and (min-width: 1201px)
.main
  display: grid
  grid-template-columns: 25% 75%
.main__items__grid
  display: grid
  grid-template-columns: repeat(3, 1fr)
  gap: 16px
  grid-auto-rows: min-content
.main__form
  margin-right: 16px
@media screen and (max-width: 1200px)
  .main
    display: grid
    grid-template-columns: 40% 60%
  .main__items__grid
    display: grid
    grid-template-columns: repeat(2, 1fr)
    gap: 16px
    grid-auto-rows: min-content
  .main__form
     margin-right: 16px
@media screen and (max-width: 1024px)
  .main
    display: grid
    grid-template-columns: 40% 60%
  .main__items__grid
    display: grid
    grid-template-columns: repeat(2, 1fr)
    gap: 16px
    grid-auto-rows: min-content
  .main__form
     margin-right: 16px
@media screen and (max-width: 769px)
  .main
    display: grid
    grid-template-columns: 40% 60%
  .main__items__grid
    display: grid
    grid-template-columns: repeat(2, 1fr)
    gap: 16px
    grid-auto-rows: min-content
  .main__form
    margin-right: 16px
@media screen and (max-width: 481px)
  #app
   padding: 16px
   width: 100vw
   background: #faf9f7
  .main
    display: flex
    flex-direction: column
  .main__items__grid
    margin-top: 16px
    display: grid
    grid-template-columns: repeat(1, 1fr)
    gap: 16px
    grid-auto-rows: min-content
  .main__form
    margin: 0
</style>
