<template>
<div>
  <div class="cats-container">
    <div class="cats-header">
      <div class="cats-title"><span>Cats</span></div>
    </div>

    <div class="cats-content">
      <div v-for="cat in cats">
        <cat-card :cat="cat" />
      </div>
    </div>
  </div>
</div>
</template>


<script>
import service from '@/api/service'
import CatCard from '@/components/CatCard'

export default {
  components: {
    CatCard
  },

  data () {
    return {
      cats: []
    }
  },

  methods: {
    getCats() {
      service.getCats().then(response => {
        this.cats = response.data
      }, error => {
        console.err(error)
      })
    }
  },

  mounted(){
    this.getCats()
  }
}
</script>


<style scoped>
.cats-container {
  min-width: 320px;
  margin: 20px;
  background-color: rgba(255, 255, 255, 0.4);
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.15);
  border-radius: 4px;
}

.cats-header {
  width: 100%;
  height: 50px;
  font-weight: bold;
  font-size: 20px;
  color: #515151;
  display: flex;
}

.cats-title {
  display: flex;
  margin-left: 20px;
}

.cats-title span {
  margin: auto;
}

.cats-content {
  height: calc(100vh - 200px);
  overflow-y: auto;
  padding: 20px;
}
</style>
