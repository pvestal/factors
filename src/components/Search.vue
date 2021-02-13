<template>
  <div class="Search">
    <form>
      <input
        type="text"
        v-model="searchValue"
        placeholder="Search Series"
        required
        @keydown.enter.prevent="handleKeydown"
      />
      <span>{{searchValue}}</span>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchValue: "",
      factors: [],
    };
  },
  mounted() {
    console.log("mounted")
    fetch("http://localhost:3000/factors")
      .then((res) => res.json())
      .then((data) => (this.factors = data))
      .catch((err) => alert("Error: ", err.message))
  },
  computed: {
    handleKeyDown() {
      console.log("SearchValue: ", this.searchValue)
      console.log("Factors: ", this.factors)
      let tempSearch = this.factors
      // if (this.searchValue != "" && this.searchValue) {
      //   tempSearch = tempSearch.filter((searchValue) => {
      //     return searchValue.series;
      //   });
      // } 
      return tempSearch
    }
  }
}
</script>
