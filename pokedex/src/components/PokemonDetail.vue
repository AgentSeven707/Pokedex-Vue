<template> 
  <div class="detail">
    <div class="detail-view" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="imageUrl + pokemon.id + '.png'" alt="">
      </div>
      <div v-if="pokemon" class="data">
        <h1>{{ pokemon.name }}</h1>
        <div class="property">
          <div class="left">Base Experience</div>
          <div class="right">{{ pokemon.base_experience }} XP</div>
        </div>
        <div class="property">
          <div class="left">Height</div>
          <div class="right">{{ pokemon.height / 10 }} m</div>
        </div>
        <div class="property">
          <div class="left">Weight</div>
          <div class="right">{{ pokemon.weight / 10 }} kg</div>
        </div>
        <h3>Pokemon Types</h3>
        <div class="types">
          <div class="type" 
            v-for="(value, index) in pokemon.types"
            :key="'value'+index">
            {{ value.type.name }}
          </div>
        </div>
        <h3>Abilities</h3>
        <div class="abilities">
          <div class="ability" 
            v-for="(value, index) in pokemon.abilities"
            :key="'value'+index">
            {{ value.ability.name }}
          </div>
        </div>
      </div>
      <h2 v-else>The pokemon was not found</h2>
      <button class="close" @click="closeDetail">Close</button>
    </div>
    <i v-else class="fas fa-spinner fa-spin"></i>
  </div>
</template>

<script>
  export default {
    props: [
      'pokemonUrl',
      'imageUrl'
    ],
    data: () => {
      return {
        show: false,
        pokemon: {}
      }      
    },
    methods: {
      fetchData() {
        let req = new Request(this.pokemonUrl);
        fetch(req)
          .then((resp) => {
            if(resp.status === 200)
              return resp.json();
          })
          .then((data) => {
            this.pokemon = data;
            this.show = true;
          })
          .catch((error) => {
            console.log(error);
          })
      },
      closeDetail() {
        this.$emit('closeDetail');
      }
    },
    created() {
      this.fetchData();
    }
  }
</script>

<style lang="scss" scoped>
  .detail {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    position: fixed;
    top: 0;
    left: 0;
    padding: 90px 10px 10px;
    width: calc(100% - 20px);
    height: calc(100vh - 20px);
    background-color: #21FF9E;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100%25' height='100%25' viewBox='0 0 1600 800'%3E%3Cg stroke='%23000' stroke-width='66.5' stroke-opacity='0.08' %3E%3Ccircle fill='%2321FF9E' cx='0' cy='0' r='1800'/%3E%3Ccircle fill='%2300f1ab' cx='0' cy='0' r='1700'/%3E%3Ccircle fill='%2300e3b6' cx='0' cy='0' r='1600'/%3E%3Ccircle fill='%2300d4bf' cx='0' cy='0' r='1500'/%3E%3Ccircle fill='%2300c5c5' cx='0' cy='0' r='1400'/%3E%3Ccircle fill='%2300b6c8' cx='0' cy='0' r='1300'/%3E%3Ccircle fill='%2300a6c8' cx='0' cy='0' r='1200'/%3E%3Ccircle fill='%230096c5' cx='0' cy='0' r='1100'/%3E%3Ccircle fill='%230086be' cx='0' cy='0' r='1000'/%3E%3Ccircle fill='%230076b3' cx='0' cy='0' r='900'/%3E%3Ccircle fill='%230066a6' cx='0' cy='0' r='800'/%3E%3Ccircle fill='%23005696' cx='0' cy='0' r='700'/%3E%3Ccircle fill='%23004785' cx='0' cy='0' r='600'/%3E%3Ccircle fill='%230d3871' cx='0' cy='0' r='500'/%3E%3Ccircle fill='%23212a5d' cx='0' cy='0' r='400'/%3E%3Ccircle fill='%23261c49' cx='0' cy='0' r='300'/%3E%3Ccircle fill='%23250f36' cx='0' cy='0' r='200'/%3E%3Ccircle fill='%23210024' cx='0' cy='0' r='100'/%3E%3C/g%3E%3C/svg%3E");
    background-attachment: fixed;
    background-size: cover;

    .detail-view {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      position: relative;
      width: 100%;
      max-width: 510px;
      padding: 20px 0 0;
      background-color: #fff;
      border-radius: 5px;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);
    
      .image {
        display: flex;
        justify-content: center;
        align-items: center;
        position: absolute;
        top: 238px;
        left: 330px;
        width: 120px;
        height: 100px;
        background-color: rgb(0, 217, 255);
        border-radius: 10%;
        overflow: hidden;
        box-shadow: 0 15px 30px rgba(119, 0, 255, 0.2), 0 10px 10px rgba(96, 11, 99, 0.2);
      }

      div::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        z-index: -1;
        width: 100%;
        height: 100%;
        background: linear-gradient(90deg,red,deeppink, blue,rgb(20, 208, 255),rgb(123, 255, 0),rgb(255, 217, 0),rgb(123, 255, 0),rgb(20, 208, 255),blue,deeppink,red);
        background-size: 800%;
        border-radius: 10px;
        filter: blur(3px);
        animation: glowing 30s linear infinite;
      }
      
      @keyframes glowing {
        0%{
          background-position: 0 0;
        }
        50%{
          background-position: 400% 0;
        }
        100%{
          background-position: 0 0;
        }
      }

      h1 {
        text-transform: capitalize;
        color: #801010;
      }

      .data {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column;
        width: 100%;
        margin-bottom: 20px;

        .property {
          width: 90%;
          max-width: 400px;
          border-bottom: 1px solid #ccc;
          margin-bottom: 10px;

          .left { float: left; }
          .right { float: right; }
        }

        h3 {
          width: 90%;
          max-width: 400px;
        }

        .types, .abilities {
          display: flex;
          justify-content: flex-start;
          flex-wrap: wrap;
          width: 90%;
          max-width: 400px;

          .type, .ability {
            margin: 0 10px 10px 0;
            padding: 5px 10px;
            border-radius: 20px;
            color: #fff;
            font-size: 1rem;
            letter-spacing: 2px;
            text-transform: capitalize;
            word-wrap: none;
            word-break: keep-all;
          }

          .type { background-color: #0b63b6; }
          .ability { background-color: #3dafe4; }
        }
      }

      .close {
        outline: none;
        border: none;
        border-radius: 10px;
        background-color: rgb(9, 21, 134);
        color: #efefef;
        padding: 8px 12px;
        margin-bottom: 20px;
        font-size: .9rem;
        cursor: pointer;
      }
    }

    i {
      font-size: 2rem;
      color: #efefef;
    }
  }
</style>
