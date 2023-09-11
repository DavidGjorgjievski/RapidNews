<template>
  <div>
    <header>
      <div class="header">
        <h1 class="header-title">RapidNews</h1>
        <nav class="header-nav">
          <!-- Show the div with NuxtLinks on desktop -->
          <div class="desktop-menu">
            <NuxtLink class="header-link" to="/">Home</NuxtLink>
            <NuxtLink class="header-link" to="/business">Business</NuxtLink>
            <NuxtLink class="header-link" to="/science">Science</NuxtLink>
            <NuxtLink class="header-link" to="/sports">Sports</NuxtLink>
            <NuxtLink class="header-link" to="/entertainment">Entertainment</NuxtLink>
            <NuxtLink class="header-link" to="/health">Health</NuxtLink>
            <NuxtLink class="header-link" to="/technology">Technology</NuxtLink>
          </div>
          
          <!-- Show the dropdown menu on mobile -->
          <div class="mobile-menu">
            <ul>
              <li class="dropdown">
               <div>
                  <div class="hamburger"></div>
                  <div class="hamburger"></div>
                  <div class="hamburger"></div>
                </div> 
                <div class="dropdown-content">
                  <NuxtLink to="/">Home</NuxtLink>
                  <NuxtLink to="/business">Business</NuxtLink>
                  <NuxtLink to="/science">Science</NuxtLink>
                  <NuxtLink to="/sports">Sports</NuxtLink>
                  <NuxtLink to="/entertainment">Entertainment</NuxtLink>
                  <NuxtLink to="/health">Health</NuxtLink>
                  <NuxtLink to="/technology">Technology</NuxtLink>
                </div>
              </li>
            </ul>
          </div>
        </nav>
      </div>
      <hr class="lightBlueLine">
    </header>
    <slot/>
  </div>
</template>

<!-- Rest of your code remains the same -->


<script> 


</script>

<style>

header{
    text-align: center;
}

body, h1, h2, h3, p, ul, li {
  margin: 0;
  padding: 0;
}

/* Apply some basic styling to the page */
body {
  font-family: Arial, sans-serif;
}

.header {
  color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #669bbc;
}

.header-title {
  font-size: 24px;
  background-color: #669bbc;
  padding: 25px;
}

.header-nav {
  padding: 20px;
}

.header-nav ul {
  list-style: none;
  display: flex;
}

.header-nav li {
  margin-right: 20px;
}

.header-nav a {
  text-decoration: none;
  color: black;
}

.header-nav a:hover {
  text-decoration: underline;
}

.header-link {
  margin-right: 20px;
  text-decoration: none;
  color: #fff;
}

.header-link.router-link-exact-active {
  text-decoration: underline;
}



.lightBlueLine {
  color: white;
  height: 2px;
  background: white;
  margin: 0;
  border: white;
  padding: 0;
}


/* Style for the dropdown menu */
  .dropdown {
    position: relative;
    display: inline-block;
    margin-right: 30px;
  }

  .dropdown-content {
    display: none;
    position: absolute;
    background-color: #f9f9f9;
    min-width: 160px;
    z-index: 1;
    margin-left: -80px;
  }

  .dropdown-content a {
    padding: 12px 16px;
    text-decoration: none;
    display: block;
    color: #333;
  }

  .dropdown-content a:hover {
    background-color: #ddd;
  }

  .dropdown:hover .dropdown-content {
    display: block;
  }

  /* Style for the active link */
  .active {
    background-color: #669bbc;
    color: #fff;
  }

  /* Add styles for desktop menu */
.desktop-menu {
  display: flex;
  align-items: center;
}

/* Add styles for mobile menu */
.mobile-menu {
  display: none;
}

.hamburger {
  width: 35px;
  height: 5px;
  background-color: white;
  margin: 6px 0;
}

/* Use media query to show/hide elements based on screen size */
@media (max-width: 768px) {
  /* Hide desktop menu and show mobile menu on smaller screens */
  .desktop-menu {
    display: none;
  }

  .mobile-menu {
    display: block;
  }
}

</style>