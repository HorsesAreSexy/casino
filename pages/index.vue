<template>
  <div class="box">
    <div class="header">
      <img class="logo" src="logo.svg">
      <div class="beckon">
        Sign up & get Welcome Bonus
      </div>
      <div class="info">
        € 100 + 55 free spins
      </div>
      <div class="button">
        Deposit now
      </div>
    </div>
    <div class="body">
      <div class="buttons">
        <div v-for="(button,key) in buttonsStructure" :key="key" class="button" :class="active === button && 'active'" @click="()=>buttonClick(button)">
          <img class="icon" :src="`icons/${button.icon}.svg`">
          <div class="name">
            {{ button.name }}
          </div>
        </div>
      </div>
      <div class="items">
        <img v-for="(item,key) in itemsStructure" :key="key" class="item" :src="`https://aws-origin.image-tech-storage.com/gameRes/sq/500/${item.item_title}.jpg`">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    const buttonsStructure = [
      {
        icon: 'star',
        name: 'all games',
        filter: () => true
      },
      {
        icon: 'crown',
        name: 'top games',
        // eslint-disable-next-line
        filter: ({ is_hot }) => is_hot
      },
      {
        icon: 'play',
        name: 'live casino',
        // eslint-disable-next-line
        filter: ({ live_dealer }) => live_dealer
      },
      {
        icon: 'axes',
        name: 'slots games',
        // eslint-disable-next-line
        filter: ({ slots_features }) => slots_features.length
      },
      {
        icon: 'pizza',
        name: 'roulette',
        // eslint-disable-next-line
        filter: ({ game_family_group }) => game_family_group === 'Roulette'
      },
      {
        icon: 'cracker',
        name: 'table games',
        // eslint-disable-next-line
        filter: ({ is_classic }) => is_classic
      },
      {
        icon: 'cards',
        name: 'card games',
        filter: ({ decks }) => +decks > 0
      }
    ]
    return {
      items: [],
      itemsStructure: {},
      buttonsStructure
    }
  },
  async mounted () {
    this.items = await fetch('https://57d10932-44d0-4d3a-98a9-6dda8c67bdd3.mock.pstmn.io?liveCasinoOnly=true').then(response => response.json())
    this.buttonClick(this.buttonsStructure[0])
  },
  methods: {
    buttonClick (structObject) {
      this.active = structObject
      this.itemsStructure = this.items.filter(structObject.filter)
    }
  }
}
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap');
.box{
  width: 1248px;
  margin: auto;
  position: relative;
}
.header{
  height: 640px;
  background: url('../static/back.svg');
  padding: 231px 53px;
  .logo{
    position: absolute;
    top: 26px;
    left: 16px;
  }
  .beckon{
    font-weight: bold;
    font-size: 43px;
    line-height: 109%;
    width: 410px;
  }
  .info{
    font-size: 28px;
    line-height: 118%;
    margin-top: 12px;
  }
  .button{
    margin-top: 24px;
    width: 217px;
    height: 72px;
    line-height: 72px;
    background: #F3B233;
    text-align: center;
  }
}
.body{
  padding: 0 16px;
  background: #060E2B;
  float: left;
  padding-bottom: 50px;
  .buttons{
    float: left;
    padding-top: 23px;
    padding-bottom: 17px;
    .button{
      float: left;
      width: 165px;
      height: 96px;
      text-align: center;
      padding-top: 21px;
      background: #0E152F;
      position: relative;
      user-select: none;
      &:last-of-type>.icon{
        margin-bottom: 10px;
      }
      &:not(:last-of-type){
        margin-right: 10px;
      }
      .icon{
        margin-bottom: 13px;
      }
      .name{
        font-size: 12px;
        line-height: 124.2%;
      }
      &:not(.active){
        cursor: pointer;
      }
      &.active:before{
        content: '';
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 2px;
        background: #F3B233;
      }
    }
  }
  .items{
    .item{
      width: 194px;
      height: 195px;
      &:not(:last-of-type){
        margin-right: 10px;
      }
    }
  }
}

</style>
