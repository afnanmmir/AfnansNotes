---
title: San Francisco (July 2022)
date: 2023-12-27
lastmod: 2023-12-27
tags:
- travel
---
![beach](travel/images/sf.jpg)
I went to San Francisco for a weekend and visited my friend [Rachel](thoughts/admiration.md#jonathan-tung-and-rachel-qian).

I arrived on Friday night, but I was stuck on the plane while it was waiting for a gate to open up, so I didn't get to Rachel's place until 1am, so we did nothing on Friday.

On Saturday, we first visited a couple of the Google offices because Rachel was interning there. We visited the Mountain View office as well as the Bay View office. The Bay View office was in the middle of nowhere, but it was still pretty nice. We then went to [Zareen's](https://www.zareensrestaurant.com/), an Indopak restaraunt. This was one of the best Indopak restaraunts I had ever been to. Most places water down the flavor and the spices to appeal to the masses, but this was the first place I had been that it felt like I was eating food cooked at home.

After a nap, we then went to San Francisco and walked around Golden Gate Park for a while. I had no idea it was bigger than Central Park. There were a bunch of cool places within the park we stopped at like a bunch of playgrounds, the botanical gardens, and a live music show. We then ended up at Ocean Beach where we chilled for a while. We then went to have dinner at a Korean place which was pretty good but pretty expensive. We tried getting ice cream afterwards, but every place we stopped at was way too busy for it to be worth it, so we just went home. We then just played a bunch of games on the Nintendo Switch and went to sleep.

The next day, the plan was to go to Half Moon Bay. In the morning, we ate breakfast and watched some Spongebob. We then left for Half Moon Bay, but we stopped at Costco on the way. We got some $1.50 hot dogs and left for the beach. The drive to the beach was like 10 miles, but it was still 40 minutes long. It was still a really nice drive with very scenic views. We got to the beach and it was very nice. It was like a small town with a very nice beach. We just chilled there for a while, played in the sand a bit, went looking for crabs, and walked around the coast. We then had lunch at a local sushi place around the area. Again, it was pretty good, but pretty expensive. We then had to leave because my flight was in the evening. We went straight from Half Moon Bay to the airport.

All in all, it was one of the most fun trips I had been on.

<div class="carousel">
  <div class="carousel-image-container">
    <img class="carousel-image" src="/travel/images/google.jpg">
    <img class="carousel-image" src="/travel/images/zareens.jpg" style="display: none;">
    <img class="carousel-image" src="/travel/images/goldenpark.jpg" style="display: none;">
    <img class="carousel-image" src="/travel/images/halfmoon.jpg" style="display: none;">
    <img class="carousel-image" src="/travel/images/field.jpg" style="display: none;">
    <img class="carousel-image" src="/travel/images/coast.jpg" style="display: none;">
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