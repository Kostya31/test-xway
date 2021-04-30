<template>
  <div class="influencer">
      <div class="influencer-row influencer-header">
        <div class="influencer-col"><span @click='sort("name")' :class="{ active: sortBy ==='name', 'sort-desc': sortOrder === 1 }">Вибрано</span></div>
        <div class="influencer-col"><span @click='sort("followers")' :class="{ active: sortBy ==='followers', 'sort-desc': sortOrder === 1 }">Количество подписчиков</span></div>
        <div class="influencer-col"><span @click='sort("er")' :class="{ active: sortBy ==='er', 'sort-desc': sortOrder === 1 }">Рейтинг увлеченности</span></div>
      </div>
      <div class="influencer-row" v-for="influencer in sortedList" :key="influencer.id">
        <div class="influencer-col">
          <div class="influencer-avatar">
              <img :src="influencer.avatarUrl" :alt="influencer.name">
              <a :href="influencer.profileUrl" class="influencer-name" target="blank">@{{ influencer.name }}</a>
          </div>
        </div>
        <div class="influencer-col">
            <span>{{ influencer.followers }}</span>
        </div>
        <div class="influencer-col">
          <span>{{ influencer.er }} %</span>
        </div>
      </div>
  </div>
</template>

<script>
import FieldService from '../services/FieldService.js';

export default {
  name: 'Influencer',

  data () {
    return {
      influencers: [],
      sortBy: "",
      sortOrder: -1,
    }
  },
   mounted () {
    const fieldService = new FieldService();
    fieldService.getInfluencers().then((influencers) => {
      this.influencers = influencers
    });

  },
  watch: {
    sortBy: function() {
      this.sortOrder = -1
    }
  },
   computed: {
    sortedList() {
      return [...this.influencers]
        .map(i => ({...i, influencers:parseFloat(i.influencers)}))
        .sort((a,b) => {
          if(a[this.sortBy] >= b [this.sortBy]) {
          return this.sortOrder
          }
        return -this.sortOrder
      })
    }
  },
    methods: {
      sort: function(sortBy){
        if(this.sortBy === sortBy) {
          this.sortOrder =-this.sortOrder;
        } else {
          this.sortBy = sortBy
        }
      }
    }
}
</script>>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .influencer-row {
    display: flex;
    justify-content: space-around;
    padding: 15px;
    border-top: 1px solid silver;
    &:hover{
     background-color: rgb(245, 245, 245);
    }
    &.influencer-header {
      &:hover{
         background-color: inherit;
      }
    }
  }
  .influencer-header {
    color: rgb(5, 165, 112);
    font-size: 16px;
    font-weight: bolt;
    span {
      cursor: pointer;
      &::after {
          content: '>';
          display: inline-block;
          transform: rotate(90deg);
        }
      &.active {
        font-weight: 700;
        &::after {
          content: '>';
        }
        &.sort-desc {
          &::after {
            content: '<';
          }
        }
      }
      
    }
  }
  .influencer-col {
    display: flex;
    align-items: center;
    width: calc(100% / 3);
    padding: 10px;
  }
  .influencer-avatar {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      object-fit: cover;
    }
    a {
      font-size: 18px;
      font-weight: 700;
      text-decoration: none;
      color: rgb(24, 24, 65);
      padding-left: 15px;
    }
  }
</style>
