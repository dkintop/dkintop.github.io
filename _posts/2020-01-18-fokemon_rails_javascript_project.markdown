---
layout: post
title:      "Fokemon! Rails + Javascript Project"
date:       2020-01-18 18:59:48 +0000
permalink:  fokemon_rails_javascript_project
---


For this project I decided to do something a little more creative on the front-end side since it was my first time getting to experiment with the different features that can be achieved through java script. Through this project I was able to gain a much better understanding of DOM manipulation through javascript and how it can make an application appear to be much more dynamic.

Throughout this application I ended up creating and appending quite a few DOM elements in order to compose what the user will see as a virtual playing card representing each Fokemon in the database. Here is the javascript I used to compose each card element:

```
  createCard(fokemon) {
    let card = document.createElement("DIV");
    card.setAttribute("class", "card");

    let name = document.createElement("DIV");
    name.setAttribute("id", "fokemon_name");
    name.innerHTML = `${fokemon.name.charAt(0).toUpperCase() +
      fokemon.name.slice(1)}`;

    let element_type = document.createElement("DIV");
    element_type.setAttribute("id", "element_type");
    element_type.innerHTML = `Element: ${fokemon.element_type}`;

    let hit_points = document.createElement("DIV");
    hit_points.setAttribute("id", `hit_points${fokemon.id}`);
    hit_points.innerHTML = `HP: ${fokemon.hit_points}`;

    let attack_points = document.createElement("DIV");
    attack_points.setAttribute("id", `attack_points${fokemon.id}`);
    attack_points.innerHTML = `Attack Points: ${fokemon.attack_points}`;

    let avatar = document.createElement("IMG");
    avatar.setAttribute("class", "avatar");
    avatar.setAttribute("id", `avatar${fokemon.id}`);
    avatar.setAttribute("src", fokemon.avatar);
    avatar.setAttribute("data-id", fokemon.id);

    const container = document.getElementById("index-container");

    let delete_button = document.createElement("BUTTON");
    delete_button.setAttribute("id", `delete_button${fokemon.id}`);
    delete_button.setAttribute("class", "delete_button");
    delete_button.setAttribute("data-id", `${fokemon.id}`);
    delete_button.innerHTML = "X";

    let train_button = document.createElement("BUTTON");
    train_button.setAttribute("id", `train_button${fokemon.id}`);
    train_button.setAttribute("class", "training_button");
    train_button.setAttribute("data-id", `${fokemon.id}`);
    train_button.innerHTML = "Train";
    train_button.addEventListener("click", function() {
      new Train(fokemon.id); // takes in fokemon_id to pass to constructor method of Train
    });

    let card_elements = [
      name,
      avatar,
      hit_points,
      element_type,
      attack_points,
      train_button,
      delete_button
    ];

    card_elements.forEach(element => card.appendChild(element));

    document.querySelector("#index-container").appendChild(card);
```

That is a lot of code! I will admit, I am sure there is a more elegant way to accomplish the same thing, an I hope to find the time to refactor, but this allows us to see each component of creating a card. Let's break it down!



```
let card = document.createElement("DIV");
    card.setAttribute("class", "card");
```
First we create an element we call "card". This element will serve as the container for all of the othre elements of the card to go into.

```
 let name = document.createElement("DIV");
    name.setAttribute("id", "fokemon_name");
    name.innerHTML = `${fokemon.name.charAt(0).toUpperCase() +
      fokemon.name.slice(1)}`;

    let element_type = document.createElement("DIV");
    element_type.setAttribute("id", "element_type");
    element_type.innerHTML = `Element: ${fokemon.element_type}`;

    let hit_points = document.createElement("DIV");
    hit_points.setAttribute("id", `hit_points${fokemon.id}`);
    hit_points.innerHTML = `HP: ${fokemon.hit_points}`;

    let attack_points = document.createElement("DIV");
    attack_points.setAttribute("id", `attack_points${fokemon.id}`);
    attack_points.innerHTML = `Attack Points: ${fokemon.attack_points}`;

    let avatar = document.createElement("IMG");
    avatar.setAttribute("class", "avatar");
    avatar.setAttribute("id", `avatar${fokemon.id}`);
    avatar.setAttribute("src", fokemon.avatar);
    avatar.setAttribute("data-id", fokemon.id);

    const container = document.getElementById("index-container");

    let delete_button = document.createElement("BUTTON");
    delete_button.setAttribute("id", `delete_button${fokemon.id}`);
    delete_button.setAttribute("class", "delete_button");
    delete_button.setAttribute("data-id", `${fokemon.id}`);
    delete_button.innerHTML = "X";

    let train_button = document.createElement("BUTTON");
    train_button.setAttribute("id", `train_button${fokemon.id}`);
    train_button.setAttribute("class", "training_button");
    train_button.setAttribute("data-id", `${fokemon.id}`);
    train_button.innerHTML = "Train";
    train_button.addEventListener("click", function() {
      new Train(fokemon.id); // takes in fokemon_id to pass to constructor method of Train
    });

    let card_elements = [
      name,
      avatar,
      hit_points,
      element_type,
      attack_points,
      train_button,
      delete_button
    ];
```


Here, we create each element of the card, set attributes of those elements based on the fokemon object that is passed in as an argument when it is called. once each element is created and attributes set we create an array filled with each element in the order in which we want them rendered to the page. We then iterate over this collection of HTML elements and append them to our card element like this:

```
card_elements.forEach(element => card.appendChild(element));
```

At this point if we were to open the html document in our browser we may be dissapointed to see that our card element is still not rendering. That is because we haven't appended our card to our doecument yet! we that like this:

```
 document.querySelector("#index-container").appendChild(card);
```

here we search our document for an element with the id 'index container') and append our card as a child element to the 'index-container' element. At this point we now have our card element appended to the document!

With a little practive I think this project has made DOM manipulation feel much more natural and I am excited to see what else I can do as I improve my javascript skills!



