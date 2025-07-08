---
layout: default
title: Test Page
nav_order: 99
---

<style>
body {
  margin: 0;
  padding: 0;
  background-color: white; /* default background */
}

body::before {
  content: "";
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 90vh; 
  background-image: url('https://images.pexels.com/photos/2833721/pexels-photo-2833721.jpeg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  z-index: -1;
}

.main-content {
  max-width: 350px;
  margin-left: auto;
  margin-right: 50px;
  background-color: rgba(255, 255, 255, 0.85);
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
  position: relative;
  z-index: 1;
}
</style>


# OS²Product

This is a test **product page** with the main content area aligned to the right and a footer outside the content box.
