<template>
  <div :class="darkMode ? 'allsection-dark' : 'allsection'">
    <div v-if="headlines.length > 0">
       <ul :class="darkMode ? 'headline-list-dark' : 'headline-list'" ref="headlinesContainer">
      <li v-for="(headline, index) in paginatedHeadlines" :key="headline.url" class="headline-item">    
        <div class="headline-details">
          <div class="thumbnail-div">
            <img v-if="headline.thumbnail" :src="headline.thumbnail" :alt="headline.description + ' thumbnail'" class="author-thumbnail" />
          </div>
              <h2 v-if="headline.thumbnail" 
            :class="[
              headline.thumbnail ? 'newsheadline' : '',
              headline.title.length > 130 ? 'newsheadline-fix' : '',
              headline.title.length <= 75 ? 'newsheadline-fix-2' : ''
            ]">
          <a :href="headline.url" target="_blank">{{ headline.title }}</a>
        </h2>
          <h2 v-if="!headline.thumbnail" :class="darkMode ? 'headline-title-dark' : 'headline-title'">
            <a :href="headline.url" target="_blank">{{ headline.title }}</a>
          </h2>
           <div v-if="headline.description" class="p-2">
            <p :class="darkMode ? 'headline-description-dark' : 'headline-description'">{{ headline.description }}</p>
          </div>
           <ul v-if="headline.keywords && headline.keywords.length" class="keywords">
              <li v-for="keyword in headline.keywords" :key="keyword">{{ keyword }}</li>
          </ul>
          <p class="published-date">{{ formatDate(headline.published_date) }}</p>
          <div class="flex flex-row justify-center">
              <p class="publisher">
              <a :href="headline.publisher.url" target="_blank" class="mr-4">Published by {{ headline.publisher.name }}</a>
            </p>
            <div class="additional-info" v-if="headline.publisher.favicon">
              <img  :src="headline.publisher.favicon" :alt="headline.publisher.name + ' favicon'" class="publisher-favicon" />
            </div>
          </div>  
        </div>
      </li>
    </ul>
      <!-- Pagination controls -->
      <div class="pagination p-1" style="background: #669bbc;" >
        <button @click="prevPage" :disabled="currentPage === 1" style="background: #ebebeb;" class="border-2 border-white rounded-md m-2">Previous</button>
        <span>{{ currentPage }} / {{ totalPages }}</span>
        <button @click="nextPage" :disabled="currentPage === totalPages" style="background: #ebebeb;" class="border-2 border-white rounded-md m-2">Next</button>
      </div>
    </div>

  <div v-else class="loading-div" :class="darkMode ? 'loading-div-dark' : ''">
      <div v-if="!darkMode">
         <img src="../assets/Spinner.gif" alt=""> 
         <h1 class="loading-title">Loading...</h1>
      </div>
      <div v-if="darkMode">
         <img src="../assets/Spinner-dark.gif" alt="">
         <h1 class="loading-title">Loading...</h1>  
      </div>
       
  </div>

   
  </div>
</template>


<script>
export default {
  props: ['selectedLanguage', 'selectedSort','darkMode','category'],
  data() {
    return {
      headlines: [],
      currentPage: 1, // Current page number
      itemsPerPage: 10, // Number of items to display per page
    };
  },
  async created() {
    this.fetchHeadlines();
  },
  watch: {
    selectedLanguage(selectedLanguage) {
      this.fetchHeadlines(selectedLanguage);
    },
    selectedSort(selectedSort) {
      this.sortHeadlines(selectedSort);
    },
  },
  methods: {
    async fetchHeadlines(language = this.selectedLanguage) {

      if(this.category!=''){
        const url = 'https://news-api14.p.rapidapi.com/top-headlines';
      const queryParams = new URLSearchParams({
        country: 'us',
        language: language,
        pageSize: '100',
        category: this.category,
      });

      const options = {
        headers: {
          'X-RapidAPI-Key': '950508bac4msh0808960aed5f40fp1b2ee0jsnc8dda4aa73b4',
          'X-RapidAPI-Host': 'news-api14.p.rapidapi.com',
        },
      };

      try {
        const response = await fetch(`${url}?${queryParams.toString()}`, options);
        const data = await response.json();
        this.headlines = data.articles;

      } catch (error) {
        console.error(error);
      }
      }
      else{
        const url = 'https://news-api14.p.rapidapi.com/top-headlines';
        const queryParams = new URLSearchParams({
          country: 'us',
          language: language,
          pageSize: '100',
          category: ''
        });

        const options = {
          headers: {
            'X-RapidAPI-Key': '950508bac4msh0808960aed5f40fp1b2ee0jsnc8dda4aa73b4',
            'X-RapidAPI-Host': 'news-api14.p.rapidapi.com',
          },
        };

        try {
          const response = await fetch(`${url}?${queryParams.toString()}`, options);
          const data = await response.json();
          this.headlines = data.articles;

        } catch (error) {
          console.error(error);
        }
      }      
    },

    sortHeadlines(selectedSort) {
      const headlinesCopy = [...this.headlines];

      if (selectedSort === 'timestamp') 
      {
        headlinesCopy.sort((a, b) => {
          const timestampA = new Date(a.published_date).getTime();
          const timestampB = new Date(b.published_date).getTime();
          return timestampB - timestampA;
        });
      } else if (selectedSort === 'title') {
        headlinesCopy.sort((a, b) => {
          const titleA = a.title.toLowerCase();
          const titleB = b.title.toLowerCase();
          return titleA.localeCompare(titleB);
        });
      }

      this.headlines = headlinesCopy;
    },

    formatDate(dateString) {
      const options = {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit',
        second: '2-digit',
        timeZoneName: 'short',
      };
      return new Date(dateString).toLocaleDateString(undefined, options);
    },
     // Go to the previous page
  prevPage() {
    if (this.currentPage > 1) {
      this.currentPage--;
      // Scroll to the top of the page
      this.scrollToTop();
    }
  },

  // Go to the next page
  nextPage() {
    if (this.currentPage < this.totalPages) {
      this.currentPage++;
      // Scroll to the top of the page
      this.scrollToTop();
    }
  },

  // Scroll to the top of the page
  scrollToTop() {
    window.scrollTo({
      top: 0,
      behavior: 'smooth', // Optional: smooth scrolling animation
    });
  },
  },
  computed: {
    totalPages() {
      return Math.ceil(this.headlines.length / this.itemsPerPage);
    },
    paginatedHeadlines() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.headlines.slice(start, end);  
    },
},

};
</script>


<style scoped>
h2 {
  margin-bottom: 20px;
  font-size: 36px;
}

p {
  margin: 20px 0;
}

.allsection{
    text-align: center;
    margin-top: 20px;
}

.allsection-dark{
    background: #011627;
    text-align: center;
    margin-top: 20px;
}

.headline-list {
  width: 600px;
  list-style: none;
  padding: 0;
  margin: 0 auto; /* Center the <ul> horizontally */
  text-align: center; /* Center the <li> elements horizontally within the <ul> */
}

.headline-list-dark{
  width: 600px;
  background: #011627;
  padding: 0;
  margin: 0 auto; /* Center the <ul> horizontally */
  text-align: center; /* Center the <li> elements horizontally within the <ul> */
}

.headline-item {
  display: flex;
  margin-bottom: 20px;
  border-bottom: 1px solid #eee;
  padding-bottom: 15px;
  flex-direction: column; /* Adjust to stack items vertically */
  align-items: center; /* Center items horizontally within the <li> */
}

.headline-details {
  flex: 1;
}

.headline-title {
  font-size: 28px;
  margin-bottom: 14px;
  font-weight: bolder;
}

.headline-title-dark{
    font-size: 28px;
    margin-bottom: 14px;
    font-weight: bolder;
}

.headline-title a {
  color: #333;
  text-decoration: none;

}

.headline-title-dark a{
    color: white;
}


.headline-title a:hover {
  text-decoration: underline;
}

.newsheadline{
  margin-top: -250px;
  color: white;
  padding: 10px; /* Add padding for better readability */
  text-shadow: 4px 4px 6px rgba(0, 0, 0, 0.8);
  margin-bottom: 100px;
}

.newsheadline:hover {
  text-shadow: 10px 10px 12px rgba(0, 0, 0, 1);
}


.headline-description{
  color: black;
}

.headline-description-dark{
  color: white;
}


.published-date {
  color: #777;
  font-size: 14px;
  margin-bottom: 5px;
}

.publisher {
  font-size: 14px;
  color: #007bff;
}

.publisher a {
  color: #007bff;
  text-decoration: none;
}

.publisher a:hover {
  text-decoration: underline;
}

.publisher-favicon {
  display: block;
  width: 30px;
  margin-top: 10px;
}

.thumbnail-div {
  display: flex;
  justify-content: center; /* Center horizontally */
  align-items: center; /* Center vertically */
} 

.author-thumbnail{
  text-align: center;
  width: 600px;
  height: 400px;
  object-fit: cover;
}


.keywords {
  display: flex;
  flex-wrap: wrap; /* Wrap keywords to the next row when they overflow */
  margin-top: 10px;
  justify-content: center;
  max-width: 600px;
}

.keywords li {
  margin-right: 10px; /* Add space between keywords */
  margin-bottom: 10px; /* Add space below keywords */
  background: #ebebeb;
  padding: 5px;
  border-radius: 15px;
}

.no-desc-small-title{
  margin-top: 200px;
}

.container {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.loading-div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 77vh;
  overflow: hidden;
}

.loading-div-dark{
  background: #011627;
}

.loading-title{
  color: #669bbc;
  font-size: 2rem;
}


@media (max-width: 600px) {
  /* Add mobile-specific styles here */
  .headline-list {
  max-width: 100%; /* Allow the list to adapt to smaller screens */
}

.headline-list-dark {
  max-width: 100%;
}

.headline-item {
  display: flex;
  flex-direction: row; /* Display items in a row on larger screens */
  justify-content: space-between; /* Add space between items */
  align-items: center;
}
}


@media (max-width: 600px) {
  .headline-list,
  .headline-list-dark {
    max-width: 100%;
  }

  .headline-item {
    flex-direction: column; /* Stack items vertically on smaller screens */
  }

  .headline-title,
  .headline-title-dark {
    font-size: 18px; /* Reduce font size for headings */
    flex-wrap: wrap;
    margin-bottom: 0px;
  }

  .headline-description,
  .headline-description-dark {
    font-size: 14px; /* Reduce font size for text content */
  }

  .publisher {
    font-size: 12px; /* Reduce font size for publisher info */
  }

  .newsheadline{
    margin-top: -350px;
  }

  .newsheadline a{
    font-size: 1.8rem;
  }

  .newsheadline-fix{
    margin-bottom: 20px;
    font-size: 1.4rem !important;
  }

  .newsheadline-fix-2{
    margin-top: -300px !important;
  }

  .pagination {
  background: red; /* Apply styles to the "pagination" div */
}

.pagination-button {
  border: 2px solid red; /* Apply styles to the pagination buttons */
}
}
</style>
