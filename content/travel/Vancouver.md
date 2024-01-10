---
title: Vancouver (August 2022)
date: 2023-12-28
lastmod: 2023-12-28
tags:
- travel
enableToc: true
---
![stanley](travel/images/stanley.jpg)
I went to Vancouver for a weekend with a few of my friends Rishi, Ishan, and Michael. Rishi and I went from Seattle, and we picked up Ishan and Michael from the airport.

We arrived around 11am on Friday, but we could not check in to our Airbnb until 3pm, so we spent some time in downtown Vancouver. Walking through the city, I noticed Vancouver was very similar to the United States (especially Seattle), which I guess shouldn't be a surprise. We got brunch at a local place (I forgot the name). It was pretty good. One of my friends wanted to get poutine afterwards, so we went to a mall to get poutine. I wasn't really a fan of it, but I guess it's an acquired taste. 

We then went to our Airbnb and rested for a bit. We watched some TV and took a nap. We then went to get dinner at a local Indian restaurant that served dosas. They tasted pretty good, but it felt a little watered down in taste and spice. We then walked down the street towards the English Bay Beach, where we got some ice cream and saw the [A-maze-ing Laughter](https://www.vancouverbiennale.com/artworks/a-maze-ing-laughter/) statues. We then went to a local pub to get some food and drinks. Afterwards, we went and got more food at a local korean fried chicken place. We ate a lot of food that day.

The next day, we started off by going to Granville Island, which was a small peninsula district with a bunch of shops and restaurants. We got lunch at the public market, where I got some fish and chips. We walked around the island for a bit and then went shopping around Downtown Vancouver. I didn't buy anything, mainly because I don't really shop, my friends bought some stuff. We were pretty tired after this, so we went back to the Airbnb to rest. We then went to a local sushi place for dinner. We got an insane plate of sushi that looked crazy and tasted great. It was pretty expensive though. We ended the night going to a local club. It was pretty fun, but I'm not really a club person, so I didn't really enjoy it that much.

The next day, we spent most of it at Stanley Park. We got lunch at a local restaurant in the area, then we walked around the park for a while. It had a very scenic view, and I enjoyed the walk. This was the last thing we did before we headed back to the airport to drop off Ishan and Michael. Rishi and I then drove back to Seattle.

It was a pretty fun trip. We did a lot in the short amount of time we had there. Though I think Vancouver is a one time visit kind of place.

<div class="carousel">
  <div class="carousel-image-container">
    <img class="carousel-image" src="/travel/images/mountains.jpg">
    <img class="carousel-image" src="/travel/images/totem.jpg" style="display: none;">
    <img class="carousel-image" src="/travel/images/sushi.jpg" style="display: none;">
    <img class="carousel-image" src="/travel/images/stanleycoast.jpg" style="display: none;">
  </div>
  <button class="carousel-button" id="prev">&#10094;</button>
  <button class="carousel-button" id="next">&#10095;</button>
</div>

<style>
.carousel {
  position: relative;
  width: 100%;
}

.carousel-image-container {
  position: relative;
  width: 100%;
}

.carousel-image {
  width: 100%; /* or any other size */
  height: auto; /* maintain aspect ratio */
}

.carousel-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: transparent;
  border: none;
  font-size: 2em;
  color: white;
}

#prev {
  left: 10px;
}

#next {
  right: 10px;
}
</style>

<script>
var currentIndex = 0;
var images = document.querySelectorAll('.carousel-image');
var prevButton = document.getElementById('prev');
var nextButton = document.getElementById('next');

prevButton.addEventListener('click', function() {
  images[currentIndex].style.display = 'none';
  currentIndex = (currentIndex - 1 + images.length) % images.length;
  images[currentIndex].style.display = 'block';
});

nextButton.addEventListener('click', function() {
  images[currentIndex].style.display = 'none';
  currentIndex = (currentIndex + 1) % images.length;
  images[currentIndex].style.display = 'block';
});
</script>