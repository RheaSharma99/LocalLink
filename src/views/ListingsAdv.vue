<template>
  <head>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3"
      crossorigin="anonymous"
    />
    <link
      href="https://fonts.googleapis.com/css?family=Ubuntu&display=swap"
      rel="stylesheet"
    />
    <title>ListingsAdv</title>
  </head>
  <body>
    <NavBar />
    <Logo />
    <SettingsButton />

    <div class="container">
      <div style="width: 10%"></div>
      <div style="flex-grow: 1"></div>
    </div>
    <br />
    <br />
    <div id="nav" class="card text-center">
      <div class="card-header">
        <ul class="nav nav-pills card-header-pills">
          <li class="nav-item">
            <a
              class="nav-link"
              href="#"
              @click="$router.push('/listingsnature')"
              >Nature</a
            >
          </li>
          <li class="nav-item">
            <a class="nav-link active" href="#">Adventure</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#" @click="$router.push('/listingsbeach')"
              >Beach</a
            >
          </li>

          <!-- ordering buttons -->
          <div>Order results by</div>
          <li class="nav-item">
            <button class="nav-link" href="#" @click="orderByStartDate()">
              Start Date
            </button>
          </li>
          <li class="nav-item">
            <button class="nav-link" href="#" @click="orderByEndDate()">
              End Date
            </button>
          </li>
          <li class="nav-item">
            <button class="nav-link" href="#" @click="orderByCost()">
              Cost
            </button>
          </li>
          <li class="nav-item">
            <button class="nav-link" href="#" @click="orderByName()">
              Tour Name
            </button>
          </li>
        </ul>
      </div>
      <div class="card-body">
        <h1 class="card-title">Wildly Adventurous</h1>
        <p class="card-text">
          <i>
            Man cannot discover new oceans unless he has the courage to lose
            sight of the shore.</i
          >
        </p>
      </div>
    </div>
    <div class="grid">
      <div class="card" v-for="tour in tours" :key="tour.tour_name">
        <img class="card-img-top" :src="tour.tour_photo" alt="Card image cap" />
        <div class="card-body" id="tour-item">
          <br />
          <button class="btn btn-primary" @click="viewTourInfo(tour.tour_id)">
            See more</button
          ><br /><br />
          <h5 class="card-title">{{ tour.tour_name }}</h5>
          <p class="card-text">{{ tour.description }}</p>
          <!-- <p class="card-text">
            <b>{{ tour.start_date }} to {{ tour.end_date }}</b>
          </p>
          <p class="card-text">
            <b>${{ tour.cost }}</b>
          </p> -->
          <!-- <br> -->
          <!-- <p class="card-text"> -->
          <!-- <small class="text-muted">Last booked 5 mins ago</small> -->
          <!-- </p> -->
        </div>
      </div>
    </div>
    <div><DefaultFooter /></div>
  </body>
</template>

<script>
import NavBar from "@/components/NavBar.vue";
import Logo from "@/components/Logo.vue";
import DefaultFooter from "@/components/DefaultFooter.vue";
// import firebase from "firebase";
import { db } from "../main.js";
import SettingsButton from '@/components/SettingsButton.vue'

export default {
  name: "ListingsAdv",
  components: { NavBar, Logo, DefaultFooter, SettingsButton },

  data() {
    return {
      tours: [],
    };
  },

  mounted() {
    db.collection("listings")
      .where("tour_type", "==", "Adventure")
      .get()
      .then((docs) => {
        docs.forEach((doc) => {
          const data = doc.data();
          let tour = {
            email: data.email,
            tour_name: data.tour_name,
            description: data.description,
            tour_photo: data.tour_photo,
            cost: data.cost,
            start_date: data.start_date,
            end_date: data.end_date,
            tour_id: String(data.email + ", " + data.tour_name),
          };
          this.tours.push(tour);
        });
      });
  },
  methods: {
    viewTourInfo(tour_id) {
      this.$router.push({
        name: "TourInfo",
        params: {
          tour_id: tour_id,
        },
      });
    },
    orderByCost() {
      this.tours.sort(function (a, b) {
        return a.cost - b.cost;
      });
      // console.log("cost", this.tours);
    },
    orderByName() {
      this.tours.sort(function (a, b) {
        return a.tour_name.localeCompare(b.tour_name);
      });
      // console.log("name", this.tours);
    },
    orderByStartDate() {
      this.tours.sort(function (a, b) {
        return new Date(a.start_date) - new Date(b.start_date);
      });
      //  console.log("dates", this.tours);
    },
    orderByEndDate() {
      this.tours.sort(function (a, b) {
        return new Date(a.end_date) - new Date(b.end_date);
      });
      //  console.log("end dates", this.tours);
    },
  },
};
</script>

<style scoped>
body {
  width: 100%;
}
.cardheader {
  border: 10px red solid;
}
.grid {
  size: 50%;
  position: relative;
  /* top: 300px;
  left: 130px;
  right: 130px; */
  margin: 50px;
  margin-top: 250px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  justify-content: center;
  align-content: center;
}
.footer {
  width: 100%;
  position:relative;
  height: 20%;
  bottom: 0;
  left: 0;
}
#nav {
  position: absolute;
  width: 100%;
  top: 80px;
}

.btn-primary {
  width: 120px;
  height: 48px;
  background: rgba(63, 163, 184, 1);
  opacity: 1;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border: none;
  color: rgba(255, 255, 255, 1);
  font-family: Ubuntu;
  font-weight: Regular;
  font-size: 18px;
  padding: 0px;
  opacity: 1;
  text-align: center;
  cursor: pointer;
  margin: 0;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
}

.customise {
  background: rgba(63, 163, 184, 1);
  color: rgba(255, 255, 255, 1);
  font-family: Ubuntu;
  font-weight: Regular;
  font-size: 18px;
}

.card-img-top {
  float: left;
  height: 250px;
  object-fit: cover;
}

a:hover {
  color: rgba(255, 255, 255, 1);
}

button:hover {
  background: rgba(63, 163, 184, 1);
}

a.nav-link.active {
  background: rgba(63, 163, 184, 1);
  font-family: Ubuntu;
  font-weight: Regular;
  font-size: 18px;
}

a.nav-link {
  color: rgba(63, 163, 184, 1);
  font-family: Ubuntu;
  font-weight: Regular;
  font-size: 18px;
}
</style>