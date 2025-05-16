---
layout: default
title: PhD
permalink: /phd/
base_path: phd
---

<div class="container">
  <div class="row ">
    <div class="col pt-5">
      <h1 class=" text-primary">XR-empowered dynamic reality modeling for AECO applications</h1>
      <p>
        This project will innovate built environment digitization through dynamic reality modeling,
        a groundbreaking 3D modeling process with eXtended Reality (XR) technologies that simultaneously reconstructs
        –both seen and unseen– built environments, and interacts with these digital models in real-time to make
        real-world processes safer, more effective and more efficient.
        The crucial steps in harnessing XR technologies to dynamically digitize and interact with digital built
        environments can be divided in three segments.
      </p>
    </div>
  </div>

  <div class="row">
    <div class="col pb-5 pt-5">
      <img class="img-fluid" src="../img/research/PhD Overview.png" alt="PhD Schema" title="Phd Schema">
    </div>
  </div>


  <!---Start the different parts in one single row to create boxes that can scale-->
  <h2 class=" text-primary mt-5">On-site Data Updating</h2>
  <ul class="category-list">
    {% assign my_array = site.pages | sort: "date" | where: 'objective', 1 %}
    {% for item in my_array %}
    {% include research_card.html %}
    {% endfor %}
  </ul>
  <hr/>
  <h2 class=" text-primary mt-5">Dynamic Reality Modeling</h2>
  <!---Start the different parts in one single row to create boxes that can scale-->
  <ul class="category-list">
    {% assign my_array = site.pages | sort: "date" | where: 'objective', 2 %}
    {% for item in my_array %}
    {% include research_card.html %}
    {% endfor %}
  </ul>
  <hr/>
  <h2 class=" text-primary mt-5">XR Scene Interaction</h2>
  <!---Start the different parts in one single row to create boxes that can scale-->
  <ul class="category-list">
    {% assign my_array = site.pages | sort: "date" | where: 'objective', 3 %}
    {% for item in my_array %}
    {% include research_card.html %}
    {% endfor %}
  </ul>


</div>