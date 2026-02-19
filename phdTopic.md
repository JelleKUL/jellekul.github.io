---
layout: default
title: PhD
permalink: /phd/
base_path: phd
---


<h1>XR-empowered dynamic reality modeling for AECO applications</h1>
<p>
  This project will innovate built environment digitization through dynamic reality modeling,
  a groundbreaking 3D modeling process with eXtended Reality (XR) technologies that simultaneously reconstructs
  –both seen and unseen– built environments, and interacts with these digital models in real-time to make
  real-world processes safer, more effective and more efficient.
  The crucial steps in harnessing XR technologies to dynamically digitize and interact with digital built
  environments can be divided in three segments.
</p>
<img src="../img/research/PhD Overview.png" alt="PhD Schema" title="Phd Schema">

<h2 class = "center-with-lines">On-site Data Updating</h2>

<p>
The first part of the research focusses on the use of XR technologies to digitize AECO structures. We aim to develop the cyclical processes to map, track and update digital information models with XR remote sensing. To this end, we will study the spatial registration of XR with pre-recorded datasets, taking into consideration that some scenes have changed (displaced objects). Subsequently, we will study how to use the XR recordings to dynamically update the digital built environment.
</p>
<ul class="category-list">
  {% assign my_array = site.pages | sort: "date" | where: 'objective', 1 %}
  {% for item in my_array %}
  {% include research_card.html %}
  {% endfor %}
</ul>

<h2 class = "center-with-lines">Dynamic Reality Modeling</h2>

<p>
The second objective deals with the reality modeling from the pre-recordings and XR inputs. Specifically, we will develop the methods to segment objects from the scene and complete their shape and appearance so they can be used by AECO applications. To achieve this challenging task, we will study the joint processing of both image and geometry inputs. To this end, we will harness Generative 3D modeling that will allow us to model the observed object parts (shape, appearance and structure) and use that information to predict the obscured object parts (e.g. missing column or window parts).
</p>
<ul class="category-list">
  {% assign my_array = site.pages | sort: "date" | where: 'objective', 2 %}
  {% for item in my_array %}
  {% include research_card.html %}
  {% endfor %}
</ul>

<h2 class = "center-with-lines">XR Scene Interaction</h2>

<p>
The third objective studies the interactions with the digital built environment. First, we will study how XR can be used to dynamically change the digital built environment as it records the real world i.e. by digitally mirroring the movement of objects in the real world. Second, we will study how to manipulate the real world from within the digital built environment i.e. show virtually changed objects in real environments for assembly or refurbishment. To this end, we will study how reality modeling can be combined with augmented and diminished reality to manipulate user experiences in real environments.
</p>
<ul class="category-list">
  {% assign my_array = site.pages | sort: "date" | where: 'objective', 3 %}
  {% for item in my_array %}
  {% include research_card.html %}
  {% endfor %}
</ul>
