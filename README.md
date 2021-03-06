
#  π Book Search App 

<a href="http://book.jacobko.info/" target="_blank">Live Demo</a>



<img src = "https://github.com/jacobkosmart/12.-June.21_SearchBookApp_VueJS/blob/98fbbde52d92f7ccad9fe2040bd93526cfac6a76/src/assets/Animation1.gif" width ="100%" /> 
<img src = "https://github.com/jacobkosmart/12.-June.21_SearchBookApp_VueJS/blob/98fbbde52d92f7ccad9fe2040bd93526cfac6a76/src/assets/Animation2.gif" width ="100%" />


## π» 1.νλ‘μ νΈ μκ°  

### π μ¬μ©κΈ°μ  λ° μΈμ΄    

- Vue.JS
- Vuex
- Vue Router
- SCSS
- BootStrap
- Netlify serverless
- Kakao Open API

### β° κ°λ° κΈ°κ°  
2021-05-29 ~ 2021-06-12


## π 2.νλ‘μ νΈ λ΄μ©

###  μ£Όμ κΈ°λ₯


- μ± μ λͺ©μ κ²μ νμ¬ μ± νμ§, κΈμ΄μ΄, μΆκ°μΌ, μΆνμ¬, μ κ°, isbn, μμΈν λμμ λ³΄ λ§ν¬ νμ΄μ§λ₯Ό μ κ³΅

- κ²μμ νλ²μ μ΅λ 50κ° λμ κΉμ§ κ°λ₯, κ²μ νν°λ accuracy (μ νλ), latest (μ΅μ ) μμλ³λ‘ κ²μ 

- Fully responsive design (λλ°μ΄μ€ ν΄μλμ λ°λΌ λ°μν λμμΈ) 

- μμΈν λμμ λ³΄ λ²νΌ ν΄λ¦­ μ, μΈλΆμ λ€μ μ¬μ΄νΈμ λΆ κ²μ νμ΄μ§λ‘ μλ λ§ν¬ 



## π 3.μ£Όμ μ½λ

### 1.App Structure 

- μ΄ νλ‘μ νΈ vue.js v3.0 μΌλ‘ κ°λ° νμμ΅λλ€. App structure λ μλμ κ·Έλ¦Όκ³Ό κ°μ΅λλ€.


<img src = "https://github.com/jacobkosmart/12.-June.21_SearchBookApp_VueJS/blob/98fbbde52d92f7ccad9fe2040bd93526cfac6a76/src/assets/App%20Structure.jpg" width ="100%" /> 



### 2.Kakao API λΉλκΈ° μ°κ²° with axios

- `axios.get()` μ ν΅ν΄μ kakao API μ μ λ³΄λ₯Ό κ°μ Έμ΄. Authorization μ .env μ api key λ₯Ό λ°λ‘ μ μ₯ν΄μ μΉ μμΌλ‘ λΈμΆλμ§ μμ

```js
const { kakaoKey } = process.env

function _fetchBook(payload) { // api μ λ³΄ κ°μ Έμ€λ _fetch ν¨μ μμ±
  const { title, type } = payload
  return new Promise((resolve, reject) => {
    axios.get(`https://dapi.kakao.com/v3/search/book?target=title&query=${title}&sort=${type}&size=50`, {
      headers: {
      Authorization: kakaoKey
      }
    })
    .then(res => {
      if (res.data.Error) {
        reject()
      }
      resolve(res)
    })
    .catch(err => {
      reject(err.message)
    })
  })
}
```

### 3. Vue Router 

- νΉμ  μ£Όμμ μ κ·Όν  νμ΄μ§ μ λ³΄λ₯Ό μ€μ  ν¨

- Hash mode λ₯Ό μ¬μ©νμ¬ λͺ¨λ  URL μ HASH(#) ννλ‘, URL μ΄ λ³κ²½λ λ νμ΄μ§κ° λ€μ λ‘λ λμ§ μμ

```js
import { createRouter, createWebHashHistory } from 'vue-router'
import Home from './Home'
import About from './About'
import Book from './Book'
import NotFound from './NotFound'


export default createRouter({
  // Hash, history
  // https://google.com/#/search  # λ₯Ό μ¬μ©ν΄μ sub page λ₯Ό μ°κ²°ν΄μ€
  history: createWebHashHistory(),
  scrollBehavior() {
    return { top: 0 }
  },
  // pages
  routes: [
    {
      path: '/', // κ²½λ‘λ₯Ό μλ―Έ home directoryλ₯Ό κ°λ¦¬μΉ¨
      component: Home
    },
    {
      path: '/about',
      component: About
    },
    {
      path: '/book/:id',
      component: Book
    },
    {
      path: '/:notFound(.*)',
      component: NotFound
    }
  ]
})
```

### 4. Vuex (store)

- μνκ΄λ¦¬ λΌμ΄λΈλ¬λ¦¬λ‘ App μ λͺ¨λ  component μ λν μ€μ μ§μ€μ μ μ₯μ μ­νμ νμ¬ μν κ΄λ¦¬λ₯Ό ν¨

- Vuex λ μ΄λ ν κ³³μ μ’μλμ§ μκ³  μ€μμμ κ΄λ¦¬ λλ―λ‘ λͺ¨λ  Component κ° μ½κΈ° / μ°κΈ° κ° κ°λ₯ν¨

```js
// index.js in store

import { createStore } from 'vuex'
import book from './book.js'
import about from './about.js'

export default createStore({
  modules: {
    book,
    about
  }
})
```

### 5. Nelify functions (Serverless) 

- Structure

<img src = "https://github.com/jacobkosmart/12.-June.21_SearchBookApp_VueJS/blob/c7813705cce16315f26e3f055db54e5c47a3cb88/src/assets/network%20serverless.jpg" width ="100%" /> 


- λ΄λΆμ μΌλ‘ `AWS Lambda` μ μ¬μ©νμ¬ serverless λ₯Ό μ¬μ©

```bash
# in netlify.toml

# Netlify Dev
# https://cli.netlify.com/netlify-dev/#netlifytoml-dev-block

# μ νλͺ¨λ
[build]
  command = "npm run build"
  functions = "functions" # Netlify μλ²λ¦¬μ€ ν¨μκ° μμ λ λλ ν λ¦¬λ₯Ό μ§μ ν©λλ€.
  publish = "dist" # νλ‘μ νΈ λΉλ κ²°κ³Όμ λλ ν λ¦¬λ₯Ό μ§μ ν©λλ€.


#  κ°λ° λͺ¨λ
[dev]
  framework = "#custom" # κ°μ§ν  νλ‘μ νΈ μ νμ μ§μ ν©λλ€. μ± μλ² λ° `targetPort` μ΅μμ μ€ννλ λͺλ Ή μ΅μμ
  command = "npm run dev:webpack" # μ°κ²°ν  νλ‘μ νΈμ κ°λ° μλ²λ₯Ό μ€ννλ λͺλ Ή (μ€ν¬λ¦½νΈ)μ μ§μ ν©λλ€.
  targetPort = 8079 # μ°κ²°ν  νλ‘μ νΈ κ°λ° μλ²μ ν¬νΈλ₯Ό μ§μ ν©λλ€.
  port = 8080 # μΆλ ₯ν  netlify μλ²μ ν¬νΈλ₯Ό μ§μ ν©λλ€.
  publish = "dist" # νλ‘μ νΈμ μ μ  μ½νμΈ  λλ ν λ¦¬λ₯Ό μ§μ ν©λλ€.
  autoLauch = "false" # Netlify μλ²κ° μ€λΉλλ©΄ μλμΌλ‘ λΈλΌμ°μ λ₯Ό μ€νν  κ²μΈμ§ μ§μ ν©λλ€.
```

### 6.Boot Strap btn primary μ λ³κ²½ - scss

> [boot strap btn variable](https://getbootstrap.com/docs/5.0/customize/sass/#variable-defaults)

- !defalut νλκ·Έλ SCSSμμ μ κ³΅νλ κΈ°λ₯μΌλ‘, μλ‘­κ² μ§μ λλ κ°μ΄ μμΌλ©΄ κΈ°μ‘΄ κ°μ λ¬΄μνκ² λ€λ μλ―Έλ₯Ό κ°μ§λλ€. μ¦, Bootstrapμμ μ§μ ν 'νλμ'μ primary μμμ μΈλΆμμ μμ ν  μ μλ€λ μλ―Έ μλλ€.

```scss

// Required
@import "../node_modules/bootstrap/scss/functions";

// Default variable overrides
$body-bg: #000;
$body-color: #111;

// Required
@import "../node_modules/bootstrap/scss/variables";
@import "../node_modules/bootstrap/scss/mixins";

```

### 7.μ€λ³΅λ isbn  μ­μ 

- API μμμ μ€λ³΅λλ isbn μ μ κ±°νμ¬ μ€λ³΅ κ²μμ λ°©μ§

- lodash μ uniqBy μ¬μ©ν΄μ μ€λ³΅λλ isbn μ μ­μ  ν΄μ€


```js
import _uniqBy from 'lodash/uniqBy'

  actions: { // κΈ°λ³Έμ μΌλ‘ λΉλκΈ°λ‘ μ²λ¦¬κ° λ¨
    async searchBooks({ state, commit }, payload) {
      if (state.loading)  return // μμ©μκ° λμμ μ¬λ¬λ² μ€νμ λ°©μ§νκΈ° μν΄  loading μ΄ true κ° λλ©΄ searchBooks λ₯Ό λΉ μ Έλκ°κ² ν¨
      commit('updateState', { // λ©μΈμ§ μ΄κΈ°ν
        message: '',
        loading: true
      })

      try {
        const res = await _fetchBook({
        ...payload,
        page: 1
      })
      console.log(res)
      // const rawIsbn = res.data.documents.isbn
      // const fisrtIsbn = rawIsbn.split(" ")
      // console.log(fisrtIsbn)
      const { documents, meta } = res.data
      const isbn = []
      for (let i = 0; i < documents.length; i++)
        isbn.push(documents[i].isbn)
      console.log(isbn)

      commit('updateState', {
        books: _uniqBy(documents, 'isbn'),
        isbn
      })
      
```

### 8.white-space MDN 

```scss
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```

- μμ μ½λλ₯Ό μΈνΈλ‘ μ¬μ©νμ¬ text κ° λμΉ λ λ€μλΆλΆμ ... μΌλ‘ μλ΅ μ²λ¦¬ ν΄μ€

![image](https://user-images.githubusercontent.com/28912774/122627853-40ec8100-d0ed-11eb-9511-4c940d654645.png)



## Reference

- [kakao developers](https://developers.kakao.com/)

- [Vue.js 3.0](https://v3.vuejs.org/)

- [Vuex](https://next.vuex.vuejs.org/)

- [Vue router](https://next.router.vuejs.org/)

- [axios github](https://github.com/axios/axios)

