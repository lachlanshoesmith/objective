<template>
  <div class="home">
    <nav>
      <h1>objective</h1>
      <p>Plan your next five years (or more) in five minutes (or less).</p>
    </nav>
    <main>
      <p>
        Some of these questions are pretty personal.
        We ask them to get you the best trajectory possible. None of this data leaves your device.
      </p>
      <section>
        <p>How old are you?</p>
        <label for="age">Age</label>
        <input type="text" name="age" id="age" v-model="age" placeholder="18">
      </section>
      <section>
        <p>Do you currently work?</p>
        <input type="radio" name="working" id="working" :value="true" v-model="working">
        <label for="working">Yes</label>
        <input type="radio" name="not-working" id="not-working" :value="false" v-model="working">
        <label for="not-working">No</label>
      </section>
      <div v-if="working">
        <section>
          <p>What is your award?</p>
          <p class="small">Don't know? Find out
            <a href="https://services.fairwork.gov.au/find-my-award">here ></a>
          </p>
          <select v-model="award">
            <option disabled value="">Please choose an award.</option>
            <option v-for="award in awards" :key="award.id">
              {{ award.name }}
            </option>
          </select>
        </section>
        <section>
          <p>Which one of these are you?</p>
          <input type="radio" name="part-time" id="part-time" value="Part time" v-model="typeOfEmployee">
          <label for="part-time">Part time</label>
          <input type="radio" name="full-time" id="full-time" value="Full time" v-model="typeOfEmployee">
          <label for="full-time">Full time</label>
          <input type="radio" name="casual" id="casual" value="Casual" v-model="typeOfEmployee">
          <label for="casual">Casual</label>
        </section>
        <section v-if="typeOfEmployee !== 'Full time'">
          <p>How many hours do you work per week?</p>
          <label for="hours-of-work">Hours</label>
          <input type="text" name="hours-of-work" id="hours-of-work" v-model="hoursWorked" placeholder="0">
        </section>
      </div>
      <section>
        <p>Do you currently study?</p>
        <input type="radio" name="studying" id="studying" :value="true" v-model="studying">
        <label for="studying">Yes</label>
        <input type="radio" name="not-studying" id="not-studying" :value="false" v-model="studying">
        <label for="not-studying">No</label>
      </section>
      <div v-if="studying">
        <section>
          <p>What do you study?</p>
          <select v-model="degree">
            <option disabled value="">Please choose a degree.</option>
            <option v-for="degree in degrees" :key="degree.id">
              {{ degree.name }}
            </option>
          </select>
        </section>
        <section>
          <p>How many more years are you going to be studying?</p>
          <label for="years-studying">Years</label>
          <input type="text" name="years-studying" id="years-studying" v-model="yearsStudying" placeholder="3">
        </section>
      </div>
      <section>
        <p>How much do you spend recreationally every week?</p>
        <label for="recreational-spending">Recreational spending ($)</label>
        <input type="text" name="recreational-spending" id="recreational-spending" v-model="recreationalSpending" placeholder="0">
      </section>
      <section>
        <p>How much do you spend on rent, utilities, etc, per week?</p>
        <label for="life-spending">Life spending ($)</label>
        <input type="text" name="life-spending" id="life-spending" v-model="lifeSpending" placeholder="0">
      </section>
      <section>
        <p>How much do you spend on food (groceries, restaurants, and takeaway) per week?</p>
        <label for="food-spending">Food spending ($)</label>
        <input type="text" name="food-spending" id="food-spending" v-model="foodSpending" placeholder="0">
      </section>
      <section>
        <p>Where do you see yourself living after your study concludes?</p>
        <select v-model="city">
          <option disabled value="">Please choose a city.</option>
          <option v-for="city in cities" :key="city.id">
            {{ city.name }}
          </option>
        </select>
      </section>
      <section>
        <button @click="calculate">Calculate</button>
      </section>
      <h1 v-if="yearsToReachGoals > 0">
        It will take you {{ yearsToReachGoals}} to reach your goals.
      </h1>
    </main>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
  data: () => {
    return {
      age: 18,
      working: false,
      award: '',
      awards: {
        'Amusement, Events and Recreation Award': {
          name: 'Amusement, Events and Recreation Award',
          code: 'MA000080'
        },
        'Fast Food Industry Award': {
          name: 'Fast Food Industry Award',
          code: 'MA000003'
        },
        'General Retail Industry Award': {
          name: 'General Retail Industry Award',
          code: 'MA000004'
        },
        'Pharmacy Industry Award': {
          name: 'Pharmacy Industry Award',
          code: 'MA000012',
          wages: {
            18: 16.37,
            19: 18.71,
            20: 21.04,
            max: 23.38
          },
          casualWages: {
            18: 20.46,
            19: 23.39,
            20: 26.30,
            max: 29.23
          }
        },
        'Restaurant Industry Award': {
          name: 'Restaurant Industry Award',
          code: 'MA000119'
        }
      },
      typeOfEmployee: 'Part time',
      hoursWorked: 0,
      studying: false,
      degree: '',
      degrees: [
        {
          name: 'Bachelor of Computer Science'
        },
        {
          name: 'Bachelor of Science'
        },
        {
          name: 'Bachelor of Arts'
        },
        {
          name: 'Bachelor of Software Engineering'
        }
      ],
      yearsStudying: 3,
      recreationalSpending: 0,
      lifeSpending: 0,
      foodSpending: 0,
      city: 'Sydney',
      cities: {
        Sydney: {
          name: 'Sydney',
          averageHousePrice: 1260000
        },
        Melbourne: {
          name: 'Melbourne',
          averageHousePrice: 500000
        },
        Brisbane: {
          name: 'Brisbane',
          averageHousePrice: 450000
        }
      },
      yearsToReachGoals: 0
    }
  },
  methods: {
    calculate () {
      // first, figure out target
      // we need to calculate what the average house price will be. assume house prices rise 6% every year
      const averageHousePrice = this.cities[this.city].averageHousePrice
      let housePrice = averageHousePrice
      const lifeSpending = parseInt(this.lifeSpending)
      const foodSpending = parseInt(this.foodSpending)
      const recreationalSpending = parseInt(this.recreationalSpending)
      let hoursWorked = parseInt(this.hoursWorked)
      let target = housePrice + (lifeSpending * 52) + (foodSpending * 52) + (recreationalSpending * 52)
      // now figure out how much this person is making while studying
      let currentWage
      const award = this.awards[this.award]
      let age = parseInt(this.age)
      let income = 0
      let years = 0
      if (this.typeOfEmployee === 'Full time') { hoursWorked = 40 }
      while (age < 21) {
        // if the user is a junior, their pay changes
        if (this.typeOfEmployee === 'Casual') {
          currentWage = award.casualWages[this.age]
        } else {
          currentWage = award.wages[this.age]
        }
        income += (currentWage * hoursWorked) * 52
        income -= (lifeSpending * 52) + (foodSpending * 52) + (recreationalSpending * 52)
        age += 1
        years += 1
      }
      currentWage = award.wages.max
      income += (currentWage * hoursWorked) * 52
      // simulate years until target is reached
      // assume house prices will rise of course
      while (income <= target) {
        income += (currentWage * hoursWorked) * 52
        income -= (lifeSpending * 52) + (foodSpending * 52) + (recreationalSpending * 52)
        housePrice = housePrice * 1.05
        target = housePrice + (lifeSpending * 52) + (foodSpending * 52) + (recreationalSpending * 52)
        years += 1
      }
      this.yearsToReachGoals = years
    }
  }
}
</script>
