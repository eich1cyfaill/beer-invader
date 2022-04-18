<template >
  <header class="header">
    <div class="header__beer">Beer</div>
    <div class="header__invader">Invader</div>
  </header>
  <div class="container">
    <main class="profile">

        <section class="profile__info" v-if="person.first_name">

          <div class="profile__img">
            <img v-if="person.avatar" v-bind:src="person.avatar" alt="robopicture" >
          </div>

          <div class="profile__details">
            <div class="profile__info_desc">
              <div>{{ this.person.first_name }}, {{ this.personAge }}, {{ this.personEmployment.title }}</div>
              <div>{{ this.person.email }}</div>
              <div>{{ this.personAddress.city}}, {{this.personAddress.country}}</div>
            </div>
            <div class="profile__info_button">
              <button @click="fetchBeer">get my random beer!</button>
            </div>

          </div>

        </section>

        <div v-else class="profile__loading">
          <div class="profile__loading_container">
            <span class="profile__loading_circle"></span>
            <span class="profile__loading_circle"></span>
            <span class="profile__loading_circle"></span>
            <span class="profile__loading_circle"></span>
          </div>
        </div>



        <section v-if="beer.name" class="profile__beer">
          <header>Your Recommended Beer</header>
          <main>
            <img src="./assets/beer.png" alt="">
            <div class="profile__beer_title">{{ this.beer.name }} by {{this.beer.brand}}</div>
            <div class="profile__beer_info">
              <ul>
                <li>{{ this.beer.style }}</li>
                <li>Used yeast: {{ this.beer.yeast }}</li>
                <li>Hop: {{ this.beer.hop }}</li>
                <li>Malts: {{ this.beer.malts }}</li>
                <li>{{ this.beer.alcohol }}, {{ this.beer.ibu }}, {{ this.beer.blg }}</li>
              </ul>
            </div>
            <button @click="fetchBeer">thats bad, roll another beer!</button>
          </main>
        </section>

        <section v-else></section>

    </main>
  </div>

</template>

<script>
import axios from 'axios'
export default {
  data(){
    return{
      person: {},
      personEmployment: {},
      personAddress: {},
      personAge: 0,
      beer: {}
    }
  },
  methods: {
    async fetchPerson(){
      const response = await axios.get('https://random-data-api.com/api/users/random_user')
      this.person = response.data
      this.personEmployment = response.data.employment
      this.personAddress = response.data.address
      let yearOfBirth = response.data.date_of_birth.slice(0, 4)
      let date = new Date()
      this.personAge = date.getFullYear() - yearOfBirth
    },
    async fetchBeer(){
      const response = await axios.get('https://random-data-api.com/api/beer/random_beer')
      this.beer = response.data
    },

  },
  mounted() {
    this.fetchPerson()
  }
}
</script>


<style lang="sass">
@import url('https://fonts.googleapis.com/css2?family=Dosis:wght@300;400;500&family=Square+Peg&display=swap')

$background: #FFF5CF
$red: #CE3216
html, body
  background-image: url("./assets/bar.png")
  background-repeat: no-repeat
  background-size: cover
  background-attachment: fixed
  overflow-x: hidden
  font-family: 'Dosis', sans-serif
  padding: 0
  margin: 0
.container
  margin: 0 auto
  max-width: 1220px
.header
  display: flex
  justify-content: center
  align-items: center
  font-family: 'Square Peg', cursive
  font-size: 55px
  &__beer
    color: #FBB117
    animation: beeron 600ms ease

  &__invader
    color: #f28e1c
    animation: invaderon 600ms ease

.profile
  background: $background
  &__img
    width: 300px
    height: 300px
  &__details
    display: flex
    flex-direction: column
    justify-content: space-around
    align-items: start
  &__loading
    width: 300px
    height: 300px
    display: flex
    margin: 0 auto
    justify-content: center
    align-items: center
    &_container
      height: 15px
      width: 105px
      display: flex
      position: relative
    &_circle
      width: 15px
      height: 15px
      border-radius: 50%
      background-color: $red
      animation: move 500ms linear 0ms infinite
      margin-right: 30px
      &:first-child
        position: absolute
        top: 0
        left: 0
        animation: grow 500ms linear 0ms infinite
      &:last-child
        position: absolute
        top: 0
        right: 0
        margin-right: 0
        animation: grow 500ms linear 0s infinite reverse


  &__info
    display: flex
    border-top: 5px solid $red
    img
      width: 100%
      height: 100%
    &_desc
      font-size: 25px
    &_button
      display: flex
      justify-content: center
      align-items: center
      padding: 15px
      button
        background: $red
        border: none
        font-size: 20px
        text-transform: uppercase
        color: white
        padding: 15px 10px
        &:hover
          cursor: pointer
  &__beer
    display: flex
    flex-direction: column
    justify-content: center
    align-items: center
    header
      background: $red
      height: 50px
      color: white
      width: 100%
      text-align: center
      font-size: 30px
      padding: 15px 0
      margin-bottom: 70px
    &_info
      margin-top: 55px
      display: flex
      flex-direction: column
      align-items: center
      font-size: 25px
      ul
        list-style: none
    &_title
      display: flex
      justify-content: center
      align-items: center
      font-size: 35px
    button
      background: $red
      border: none
      width: 100%
      height: 10vh
      font-size: 20px
      text-transform: uppercase
      color: white
      display: flex
      justify-content: center
      align-items: center
      &:hover
        cursor: pointer

@keyframes grow
  from
    transform: scale(0,0)
    opacity: 0
  to
    transform: scale(1,1)
    opacity: 1


@keyframes move
  from
    transform: translateX(0px)
  to
    transform: translateX(45px)


@keyframes beeron
  from
    transform: translateX(-30px)
  to
    transform: translateX(0px)


@keyframes invaderon
  from
    transform: translateX(30px)
  to
    transform: translateX(0px)


@media screen and (max-width: 425px)
  .container
    width: 100vw
    margin: 0 auto
  .profile
    &__info
      display: flex
      flex-direction: column
      justify-content: center
      align-items: center
      &_desc
        text-align: center
    &__details
      align-items: center
      margin-top: 15px
    &__beer
      &_info
        ul
          width: 60%
      &_title
        background: $red
        padding: 10px 155px
        color: white
        text-align: center
</style>
