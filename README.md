

> **AI Assistance & Project Purpose:** I used AI to assist with generating and refining portions of the HTML, CSS, and JavaScript in this project. My primary purpose in building this project was not to demonstrate that I could write every component of a webpage from scratch. Instead, I used it as a hands-on environment to become more comfortable working through the command line, navigating directories, creating and managing files, running commands, and seeing how changes made through the CLI affected a working project. I also used the process to experiment with commands, make mistakes, troubleshoot problems, and better understand the relationship between my local environment, the files in a project, and the finished application. AI helped me build something substantial enough to practice against, while my focus was on understanding the workflow and becoming more confident using the CLI rather than simply memorizing commands.


# Colombia Estratos Interactive Preview

## Overview

I created this project as an interactive way to organize and present information about social class and urban life in Colombia. Instead of putting everything into a normal static table, I wanted the user to be able to move between **Bogotá, Medellín, and Cartagena** and compare what life looks like across **Estratos 4, 5, and 6**.

The main idea was to take a fairly large amount of information and make it easier to understand visually. Each city contains information about neighborhoods, clothing, social life, weekend activities, and indicators of wealth.

This project also gave me practice thinking about something beyond simply making a webpage work. I had to think about **how the information should be structured, how the user would move through it, and how HTML, CSS, and JavaScript work together to create one interface.**

## How I Built It

I built the project around three basic layers:

**HTML = structure**  
**CSS = presentation**  
**JavaScript = behavior**

That distinction helped me understand the project as I was creating it.

The HTML holds the actual information and gives the page its structure. I created a masthead, navigation buttons for the three cities, Estrato columns, individual comparison rows, section breaks, and a footer.

For example, the city navigation uses buttons with a `data-city` attribute:

```html
<button class="city-btn active-bog" data-city="bog">
    <span class="city-label">Bogotá</span>
</button>
```

I then created separate content sections for each city:

```html
<div id="table-bog" class="table-body active">
<div id="table-med" class="table-body">
<div id="table-car" class="table-body">
```

Bogotá starts with the `active` class, which means it is the information displayed when the page first loads.

## Creating the Visual Design

I used CSS to create the visual identity of the project.

One thing I wanted was for each city to have its own visual character while still keeping the entire project consistent. I created CSS variables for the main city colors:

```css
--bog: #2D4A3E;
--med: #7A3020;
--car: #1A3A5C;
```

Bogotá uses green, Medellín uses a darker red, and Cartagena uses blue.

I also created separate colors for Estratos 4, 5, and 6. This gave me another visual layer for showing the difference between the cities and the socioeconomic categories.

For the typography, I combined **Playfair Display** with **DM Sans**. My goal was to make the project feel more like an editorial or research presentation than a generic webpage.

I used CSS Grid heavily for the comparison table:

```css
.row {
    display: grid;
    grid-template-columns: 160px 1fr 1fr 1fr;
}
```

The first column contains the category being discussed, while the next three columns represent Estratos 4, 5, and 6.

Conceptually, I can read this as:

> Create four columns. Give the first column a fixed width for the label, and divide the remaining space equally among the three Estratos.

That made CSS Grid a good fit because the information itself is naturally organized into columns.

## Making the Page Interactive

The most important JavaScript I wrote controls the city selector.

I created a function called `setCity()`:

```javascript
function setCity(city, btn) {
    ['bog','med','car'].forEach(c => {
        root.querySelector('#table-' + c).classList.remove('active');
    });

    root.querySelectorAll('.city-btn').forEach(b => {
        b.classList.remove(
            'active-bog',
            'active-med',
            'active-car'
        );
    });

    root.querySelector('#table-' + city).classList.add('active');
    btn.classList.add('active-' + city);
}
```

This was one of the most useful parts of the project for understanding how JavaScript interacts with HTML.

The logic is basically an automated checklist:

1. Find all three city tables.
2. Remove the `active` status from them.
3. Remove the active styling from all three buttons.
4. Find the city the user selected.
5. Give that city's table the `active` class.
6. Give the selected button its corresponding active style.

I then connected that function to the buttons:

```javascript
root.querySelectorAll('[data-city]').forEach(btn =>
    btn.addEventListener('click', () =>
        setCity(btn.dataset.city, btn)
    )
);
```

This tells JavaScript to find every element containing `data-city` and listen for a click.

When somebody clicks a button, JavaScript reads the city's value from the button:

```javascript
btn.dataset.city
```

So clicking:

```html
data-city="med"
```

passes `med` into my `setCity()` function.

The function then knows that it needs to display:

```html
#table-med
```

and apply:

```css
.active-med
```

to the selected button.

That connection between **HTML attributes → JavaScript logic → CSS classes** is probably the biggest concept I took away from building this.

## Making It Responsive

I also added media queries so the project would not only work on a desktop.

On larger screens, the information appears as a traditional comparison table. On smaller screens, I change the layout:

```css
@media(max-width:600px) {
    #colombia-preview .row {
        grid-template-columns: 1fr;
    }
}
```

Instead of forcing four columns onto a small phone screen, the content collapses into a single-column layout.

This taught me that responsive design is not simply making everything smaller. Sometimes the actual **structure of the interface needs to change** depending on the amount of screen space available.

## What I Learned

The biggest thing I learned from this project is that HTML, CSS, and JavaScript make more sense when I stop thinking about them as three unrelated languages.

They are three parts of the same system.

**HTML identifies what something is.**  
**CSS determines what it looks like.**  
**JavaScript determines what it does.**

For example, the HTML says that a button represents Bogotá. CSS determines what the Bogotá button looks like when it is selected. JavaScript listens for the click and changes which city is active.

I also got more practice with:

- CSS variables
- CSS Grid
- responsive design
- classes and IDs
- `data-*` attributes
- DOM selection
- `querySelector()`
- `querySelectorAll()`
- `classList`
- JavaScript functions
- `forEach()`
- event listeners
- connecting user actions to changes in the DOM

## What I Would Improve Next

If I continued developing this project, I would separate the HTML, CSS, and JavaScript into their own files instead of keeping most of the project together.

I would probably structure it like this:

```text
colombia-estratos/
│
├── index.html
├── styles.css
├── script.js
└── README.md
```

I would also consider moving the city information into JavaScript objects or JSON. Right now, the content is written directly into the HTML. That works for three cities, but separating the data from the interface would make the project easier to expand.

For example, I could eventually add cities such as Cali or Barranquilla without having to manually duplicate large sections of HTML.

## Final Takeaway

This project helped me move from thinking about code as individual commands toward thinking about it as a **system**.

The page has information, visual rules, and behavior. Each part has a specific responsibility, but they work together.

The simplest way I would describe what I built is:

> I created an interactive comparison interface where HTML organizes the information, CSS creates the visual system, and JavaScript controls which information the user sees.

That is the part of this project that was most valuable to me. I did not just want to create something that looked good. I wanted to understand **why it worked and how the different pieces connected together.**
