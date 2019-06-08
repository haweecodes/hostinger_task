<template>
  <div>
    <div v-if="!showAll">
      <button @click="prev()">&laquo;</button>
      Page {{currentPage}} of {{pageCount}}
      <button @click="next()">&raquo;</button>
    </div>
    <table>
      <tr>
        <th @click="sort(obj)" v-for="(obj, i) in header" :key="i">
          {{obj.title}}
          <font-awesome-icon :icon="obj.icon"/>
        </th>
      </tr>
      <tr v-for="(movie, i) in listData" :key="i" @click="cellColor($event, movie)">
        <td style="width: 40%">{{movie.movie_title}}</td>
        <td style="width: 10%">{{movie.title_year}}</td>
        <td style="width: 30%">{{movie.director_name}}</td>
        <td style="width: 10%">{{movie.country}}</td>
        <td style="width: 10%">{{movie.duration}}</td>
      </tr>
    </table>

    <Modal v-model="isVisible">
      <h4>Movie Information</h4>
      <p>Title: {{modalData.movie_title}}</p>
      <p>Year: {{modalData.title_year}}</p>
      <p>Director: {{modalData.director_name}}</p>
      <p>Budget: {{modalData.budget}}</p>
      <p>Gross: {{modalData.gross}}</p>
      <p>Country: {{modalData.country}}</p>
      <p>Duration: {{modalData.duration}}</p>
    </Modal>
  </div>
</template>

<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import {
  faSortAlphaUp,
  faSortAlphaDown
} from "@fortawesome/free-solid-svg-icons";
import Modal from "hawee-modal";
import "hawee-modal/dist/h-modal.css";

export default {
  name: "DataTable",
  props: ["list", "showAll"],
  data() {
    return {
      myIcon1: faSortAlphaUp,
      myIcon2: faSortAlphaDown,
      header: [],
      showPagination: true,
      listData: [],
      pStart: 0,
      pEnd: 30,
      pageCount: 0,
      currentPage: 1,
      isVisible: false,
      modalData: {},
      prevCell: ""
    };
  },
  mounted() {
    this.header = [
      {
        title: "Title",
        icon: this.myIcon2
      },
      {
        title: "Year",
        icon: this.myIcon2
      },
      {
        title: "Director",
        icon: this.myIcon2
      },
      {
        title: "Country",
        icon: this.myIcon2
      },
      {
        title: "Duration",
        icon: this.myIcon2
      }
    ];
    if (!this.showAll) {
      this.listData = this.list.slice(this.pStart, this.pEnd);
      this.pageCount = Math.max(Math.ceil(this.list.length / 30));
    } else {
      this.listData = this.list;
    }
  },
  methods: {
    sort(obj) {
      let compare = "";
      switch (obj.title) {
        case "Title":
          compare = "movie_title";
          break;
        case "Year":
          compare = "title_year";
          break;
        case "Director":
          compare = "director_name";
          break;
        case "Country":
          compare = "country";
          break;
        case "Duration":
          compare = "duration";
          break;
      }
      if (obj.icon === this.myIcon2) {
        this.listData.sort(function(a, b) {
           return a[compare] > b[compare] ? 1 : -1;
        });
        obj.icon = this.myIcon1;
      } else {
        this.listData.sort(function(a, b) {
          return a[compare] < b[compare] ? 1 : -1;
        });
        obj.icon = this.myIcon2;
      }
    },
    
    next() {
      if (this.currentPage === 169) return 0;
      this.currentPage += 1;
      this.pStart += 30;
      this.pEnd += 30;
      this.listData = this.list.slice(this.pStart, this.pEnd);
    },
    prev() {
      if (this.currentPage === 1) return 0;
      this.currentPage -= 1;
      this.pStart -= 30;
      this.pEnd -= 30;
      this.listData = this.list.slice(this.pStart, this.pEnd);
    },
    cellColor(event, movie) {
      if(this.prevCell != ""){
        this.prevCell.path[1].style.backgroundColor = "gray"
      }
      this.prevCell = event
      event.path[1].style.backgroundColor = "red"
      this.isVisible = true
      this.modalData = movie;
    }
  },
  components: {
    FontAwesomeIcon,
    Modal
  }
};
</script>

<style scoped>
table{
  width: 100%;
}

th,
td {
  text-align: left;
  padding: 10px;
}

tr {
  background-color: gray;
  color: white;
}
tr:hover {
  cursor: pointer;
}
th:hover {
  background-color: green !important;
}
</style>
