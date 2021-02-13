<template>
  <div class="container">
    <div class="Search">
      <form>
        <input
          type="text"
          v-model="series"
          placeholder="Series"
          required
        />
        <div v-if="series">
          <span class="pill">Series: {{series}}</span>
        </div>
        <div v-if="series">
          <ul v-for="factor in factors" :key="factor.id" class="level">
            <li><span class="pill">Factor Level: {{factor.level}}</span></li>
          </ul>
        </div>
      </form>
    </div>
    <div v-for="factor in filteredSeries" :key="factor.id" class="factor">
      <h2>Id: {{factor.id}} - Level: {{ factor.level }} - Points: {{ factor.points }}</h2>
      <p>Series: {{ factor.series }}</p>
      <p v-if="factor.header.length">FLD: {{ factor.header }}</p>
      <ul>
        <li v-for="bullet in factor.bullets" :key="bullet">{{ bullet }}</li>
      </ul>
      <!-- Only display if illustrations -->
      <div v-if="factor.illustrations.length">
        <!-- Toggle illustrations -->
        <h3 @click="showIllustrations = !showIllustrations" class="hyperlink">
          illustrations
        </h3>
        <!-- Only display if illustrations -->
        <div v-if="showIllustrations">
          <ul>
            <li v-for="illustration in factors.illustrations" :key="illustration.id">
              {{ illustration }}
            </li>
          </ul>
        </div>
      </div><!--End Illustrations Loop-->
    </div><!--End Factor Loop-->
  </div>
</template>

<script>
export default {
  name: "Factors",
  data() {
    return {
      factors: [],
      showIllustrations: false,
      searchValue: '',
      series: '',
      level: ''
    };
  },
  mounted() {
    fetch("http://localhost:3000/factors")
      .then((res) => res.json())
      .then((data) => (this.factors = data))
      .catch((err) => alert("Error: ", err.message));
  },
  computed: {
    filteredSeries() {
      if (this.series == "") {
        return this.factors;
      } else {
        return this.factors.filter((factor) =>
          factor.series.includes(this.series)
        );
      }
    },
  },
  methods: {
    setSeries() {
      
    }
  }
};
</script>

<style>
.factor {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3 px rgba(0, 0, 0, 0.05);
}
h3 {
  cursor: pointer;
}
.hyperlink {
  color: blue;
}
.pill {
  display: inline-block;
  margin: 10px 10px 0 0;
  color: #444;
  background: #ddd;
  padding: 8px;
  border-radius: 20px;
  font-size: 14px;
}
.level {
  display: inline-block;
  list-style-type: none;
  margin: 0;
  padding: 0;
}
</style>