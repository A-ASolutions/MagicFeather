<template>
  <div id="content">
    <img alt="Vue logo" src="./logo1.png" />
    <h1 class="title">PocketBooks</h1>
    <h2 class="h2" @click="showModal = true">New Book</h2>
    <hr />
    <h2>My Bookshelf</h2>

    <ol class="search-results">
      <li class="search-result" v-for="book in bookShelf" :key="book">
        <div>
          <i class="fa fa-trash-o" @click="removeBook(book)">
            Remove from Bookshelf</i
          >
          <h3 class="booktitle">{{ book.volumeInfo.title }}</h3>
          <h4 v-if="book.volumeInfo.authors" class="bookAuthor">
            by {{ book.volumeInfo.authors[0] }}
          </h4>

          <img
            v-if="book.volumeInfo.imageLinks"
            :src="book.volumeInfo.imageLinks.thumbnail"
            class="bookThumbnail"
          />
          <img v-else src="~@/assets/unavailable1.png" />

          <p v-if="book.volumeInfo.description" class="bookDesc">
            {{ book.volumeInfo.description.slice(0, 200) + "..." }}
          </p>
          <p v-else>{{ "Information missing" }}</p>

          <p class="isbn">
            ISBN: {{ book.volumeInfo.industryIdentifiers[0].identifier }}
          </p>
        </div>
      </li>
    </ol>

    <transition name="fade" appear>
      <div
        class="modal-overlay"
        v-if="showModal"
        @click="showModal = false"
      ></div>
    </transition>
    <transition>
      <div id="search-content" class="modal" v-if="showModal">
        <img alt="Vue logo" src="./logo1.png" />
        <form id="book-search" class="input-box" @submit.prevent="search">
          <label for="content">Book Title:</label><br />
          <input
            type="text"
            id="book-title"
            placeholder="Enter book title"
            v-model="title"
          /><br />
          <label for="search-content"> Author:</label><br />
          <input
            type="text"
            id="book-author"
            placeholder="enter author's name"
            v-model="author"
          /><br />
          <button id="search-btn" value="submit">Search</button> <br />
          <button id="cancel-btn" value="reset" @click="showModal = false">
            Cancel</button
          ><br />
          <div v-if="searchResults.totalItems === 0">
            <h2>No book was found"</h2>
          </div>

          <ol class="search-results">
            <li
              class="search-result"
              v-for="book in searchResults.items"
              :key="book.id"
            >
              <div>
                <i class="fa fa-bookmark" @click="saveBook(book)">
                  Add to Bookshelf</i
                >
                <h3 class="booktitle">{{ book.volumeInfo.title }}</h3>
                <h4 v-if="book.volumeInfo.authors" class="bookAuthor">
                  by {{ book.volumeInfo.authors[0] }}
                </h4>
                <img
                  v-if="book.volumeInfo.imageLinks"
                  :src="book.volumeInfo.imageLinks.thumbnail"
                  class="bookThumbnail"
                />
                <img v-else src="~@/assets/unavailable1.png" />

                <p v-if="book.volumeInfo.description" class="bookDesc">
                  {{ book.volumeInfo.description.slice(0, 200) + "..." }}
                </p>
                <p v-else>{{ "Information missing" }}</p>

                <p class="isbn">
                  ISBN: {{ book.volumeInfo.industryIdentifiers[0].identifier }}
                </p>
              </div>
            </li>
          </ol>
        </form>
        <hr />
         <h2>My Bookshelf</h2>

    <ol class="search-results">
      <li class="search-result" v-for="book in bookShelf" :key="book">
        <div>
          <i class="fa fa-trash-o" @click="removeBook(book)">
            Remove from Bookshelf</i
          >
          <h3 class="booktitle">{{ book.volumeInfo.title }}</h3>
          <h4 v-if="book.volumeInfo.authors" class="bookAuthor">
            by {{ book.volumeInfo.authors[0] }}
          </h4>

          <img
            v-if="book.volumeInfo.imageLinks"
            :src="book.volumeInfo.imageLinks.thumbnail"
            class="bookThumbnail"
          />
          <img v-else src="~@/assets/unavailable1.png" />

          <p v-if="book.volumeInfo.description" class="bookDesc">
            {{ book.volumeInfo.description.slice(0, 200) + "..." }}
          </p>
          <p v-else>{{ "Information missing" }}</p>

          <p class="isbn">
            ISBN: {{ book.volumeInfo.industryIdentifiers[0].identifier }}
          </p>
        </div>
      </li>
    </ol>
      </div>
    </transition>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",

  data() {
    return {
      author: "",
      searchResults: [],

      storageLength: "",
      bookShelf: [],

      showModal: false,
    };
  },
  mounted() {
    this.bookShelf = JSON.parse(sessionStorage.getItem("bookShelf") || "[]");
  },

  methods: {
    search() {
      axios
        .get(
          "https://www.googleapis.com/books/v1/volumes?q=" +
            this.title +
            " " +
            this.author
        )
        .then((response) => {
          this.searchResults = response.data;

          console.log(this.title);
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    saveBook(book) {
      if (this.bookShelf.indexOf(book) == -1) {
        this.bookShelf.push(book);

        sessionStorage.setItem("bookShelf", JSON.stringify(this.bookShelf));
      } else {
        alert("You cannot add the same book twice");
      }
    },

    removeBook(book) {
      this.bookShelf.splice(this.bookShelf.indexOf(book), 1);

      sessionStorage.setItem("bookShelf", JSON.stringify(this.bookShelf));
    },
  },
};
</script>

<style>
@media all and (max-width: 320px) {
  .search-results {
    display: inline;
    flex-flow: wrap;
    flex-direction: row;
    align-items: center;
    justify-content: space-evenly;
    float: none;
  }
}

i {
  padding: 20px;
  font-size: 16px;
  color: black;
  font-family: Arial;
  cursor: pointer;
  text-align: center;
  justify-content: center;
  align-items: center;
}
.h2 {
  border: none;
  background: #404040;
  color: #ffffff !important;
  font-weight: 100;
  padding: 20px;
  text-transform: uppercase;
  border-radius: 6px;
  display: inline-block;
  transition: all 0.3s ease 0s;
}

.h2:hover {
  color: #404040 !important;
  font-weight: 700 !important;
  letter-spacing: 3px;
  background: none;
  -webkit-box-shadow: 0px 5px 40px -10px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 5px 40px -10px rgba(0, 0, 0, 0.57);
  transition: all 0.3s ease 0s;
}
.h2:active {
  background-color: #3e6e8e;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
input[type="text"],
select {
  width: 90%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
button {
  color: #444444;
  background: #f3f3f3;
  border: 1px #dadada solid;
  padding: 10px 10px;
  border-radius: 2px;
  font-weight: bold;
  font-size: 9pt;
  outline: none;
  margin: 10px;
  width: 150px;
}

button:hover {
  border: 1px #c6c6c6 solid;
  box-shadow: 1px 1px 1px #eaeaea;
  color: #333333;
  background: #f7f7f7;
}

button:active {
  box-shadow: inset 1px 1px 1px #dfdfdf;
}
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 98;
  background-color: rgba(0, 0, 0, 0.3);
}

.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 99;
  overflow-y: scroll;
  text-align: center;

  width: 100%;
  height: 100%;
  background-color: whitesmoke;
  padding: 25px;
}
.fade-enter-active,
.fade-leave-active {
  transition: opactiy 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.input-box {
  margin: 40px;
}
.search-results {
  padding-left: 0;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  width: 100%;
  justify-content: center;
  
  li + li {
    margin-top: 0.5rem;
  }
}

.search-result {
  align-items: center;
  background: radial-gradient(
    circle,
    rgba(2, 0, 36, 1) 0%,
    rgba(255, 255, 255, 1) 0%,
    rgba(204, 222, 213, 1) 100%
  );
  border-radius: 0.25rem;
  display: flex;
  flex-direction: row;
  overflow: hidden;
  position:relative;
  width: 720px;
  max-width: 96%;
  margin: 15px auto;
  display: block;
 float:left; 
}
.bookDesc {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
  letter-spacing: 0px;
  word-spacing: 2px;
  color: #000000;
  font-weight: normal;
  text-decoration: none;
  font-style: normal;
  font-variant: normal;
  text-transform: none;
}
</style>


