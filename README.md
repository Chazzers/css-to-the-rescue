# Pure CSS Interactive 3D Rubiks Cube

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/33430669/223968606-19509ff0-bfb8-42a1-a0fd-7babf4b842f0.png">

This is a pure CSS interactive 3D rubiks cube. This was an assignment for the study CMD at HvA for which we had to create an interactive concept with pure CSS. The goal of the assignment was to explore the possibilities of CSS, look into new CSS tech and limit-test yourself with what you can achieve by just using CSS.

## Process

Underneath will be the process and the steps taken to creating a pure CSS interactive 3D rubiks cube.

### 1. Creating one cube and then multiplying

The first step to take was creating one cube. Since this is my second rodeo but it was a long time ago, I had to do some digging on how people created cubes. I quickly found [this](https://3dtransforms.desandro.com/cube) website explaining how to create a 3D CSS cube. Once I knew how to create one, and how the HTML is supposed to look like I just needed to copy paste that 27 times so I would have enough for my Rubiks cube. To make my css more concise I used the power of custom properties.

```css
:root {
  --cube-sides: 10cqw;
  --cube-single-sides-pos-x: 0;
  --cube-single-sides-pos-y: 0;
  --cube-single-sides-pos-z: 0;
  --cube-single-sides-rotate-x: 0;
  --cube-single-sides-rotate-y: 0;
  --cube-single-sides-rotate-z: 0;
}
/* Select all the cube sides */
body > div > div > div > div {
  width: var(--cube-sides);
  height: var(--cube-sides);
  border: var(--cube-sides-border);
  display: block;
  padding: 0;
  margin: 0;
  /* https://github.com/SamSlotemaker/CSS-Rubiks-Cube/blob/master/rubiks-kubus/styles/style.css Thanks sam voor deze pos! */
  position: absolute;
  transform: rotateX(var(--cube-single-sides-rotate-x)) rotateY(
      var(--cube-single-sides-rotate-y)
    )
    rotatez(var(--cube-single-sides-rotate-z)) translateX(
      calc(var(--cube-single-sides-pos-x) * var(--cube-sides))
    )
    translateY(calc(var(--cube-single-sides-pos-y) * var(--cube-sides))) translateZ(
      calc(var(--cube-sides) / 2)
    );
  background-color: black;
  box-shadow: inset 0 0 0 0.25rem rgba(0, 0, 0, 1);
}

/* Select single cube side */
/* front */
body > div > div > div > div:nth-of-type(1) {
  --cube-single-sides-pos-x: 0;
  --cube-single-sides-pos-y: 0;
  --cube-single-sides-rotate-x: 0;
  --cube-single-sides-rotate-y: 0;
  --cube-single-sides-rotate-z: 0;
}
```

And basically all you have to do is assign a color for each side of the rubiks cube and then change a rotation value so you can rotate all the sides in 3D.

**Blooper**

I was wondering also when creating a cube what would happen without `transform-style: preserve-3d`. Well here's your answer.

![image 6](https://user-images.githubusercontent.com/33430669/223976752-884b4c34-3dc0-426f-890b-72abcfcb9abf.jpg)
![image 5](https://user-images.githubusercontent.com/33430669/223976760-715426e2-968f-4dcc-a6a3-21ab88f4c968.jpg)

### 2. Positioning the cubes

The positioning of the cubes is also done with custom properties to create clarity and concise css. First you select all of the single cubes. And then you start to position them in a way that seems logical. I started building the Cube from the center cube. So all the values need to be adjusted according to this logic.

```css
/* Select all cubes */
body > div > div > div {
  transform-style: preserve-3d;
  display: flex;
  position: absolute;
  transform: rotateX(var(--cube-single-rotate-x)) rotateY(
      var(--cube-single-rotate-y)
    )
    rotateZ(var(--cube-single-rotate-z)) translateX(
      calc(var(--cube-single-pos-x) * var(--cube-sides))
    )
    translateY(calc(var(--cube-single-pos-y) * var(--cube-sides))) translateZ(
      calc(var(--cube-single-pos-z) * var(--cube-sides))
    );
  transform-origin: center center;
  perspective-origin: center;
  transition: all 1s ease var(--anime-delay);
  width: var(--cube-sides);
  height: var(--cube-sides);
}

/* Select one cube */
/* front-top-left */
body > div > div > div:nth-of-type(1) {
  --cube-single-pos-x: -1;
  --cube-single-pos-y: -1;
  --cube-single-pos-z: -1;
}
```

As you can see the logic is that if you want the cube in the middle of the rubiks cube, and you start building the cube from the top left, the first cube should have an x value of -1. Then the y value needs to also be -1. Because to go left with a transform you need to go into minus values and to go up you also need to go into minus values. So right now we go 1 space left, and one space up. The final z value also needs to be minus one because in 3d space you need to go to the front.

The rest of the other 27 cubes are built in the same way, selecting each individual cube. As I started off colorless, this is what I ended up with:

![image 1](https://user-images.githubusercontent.com/33430669/223976774-75bea9f9-5f3e-430d-bd11-1d05f9ce6b75.jpg)

**Blooper**

When positioning the cubes I forgot to put `position: absolute` on the singular cubes which caused some weird behavior.

![image 2](https://user-images.githubusercontent.com/33430669/223976771-9499d8cd-f0fa-4d79-94fa-968af06f8bda.jpg)

### 3. Assigning colors

The easiest and laziest way of assigning colors is just selecting all the single faces of a cube and assigning a color. Which is what I did.

```css
/*
	Cube sides
*/
body > div > div > div > div {
  /* other properties... */
  background-color: var(--cube-background-color);
}
/* front */
body > div > div > div > div:nth-of-type(1) {
  /* other properties... */
  --cube-background-color: var(--cube-front-color);
}
```

Which then ended up making the cubes like this:

![image 3](https://user-images.githubusercontent.com/33430669/223976767-2fadcaed-1307-4ccc-9d6a-715732683d6c.jpg)

But I thought this styling was a bit boring so I wanted to make it a bit more interesting by making the cube sides look like this:

![image 4](https://user-images.githubusercontent.com/33430669/223976763-f6d297b7-0e79-44a4-8fe0-b179ed85481c.jpg)

To achieve this I had to do the following:

```css
/*
	Cube sides make them all black
*/
body > div > div > div > div {
  /* other properties... */
  background-color: black;
}

/* Add before with some styling and the face color inside*/
body > div > div > div > div::before {
  position: absolute;
  content: "";
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: var(--cube-background-color);
  box-shadow: inset 0 0 0 0.25rem rgba(0, 0, 0, 1);
  border-radius: 8px;
}
```

### Exploring position relative with the singular cubes

At some point I was like, it would be fun to just add a blooper to my final concept so I wanted to recreate this again. But after changing some other values in the cubes I wasn't able to recreate this anymore.

![image 2](https://user-images.githubusercontent.com/33430669/223976771-9499d8cd-f0fa-4d79-94fa-968af06f8bda.jpg)

Because if I now changed the position of a single cube from absolute to relative, something entirely different happened.

<img width="401" alt="image" src="https://user-images.githubusercontent.com/33430669/223983257-958aef03-dc1f-405b-a3d6-b0b6f6388ce9.png">

I call this **Blooper**: **The Stairway** which I implemented in my final concept.

### Adding interaction with radio buttons

Now here's where it get's tricky. Adding interaction to a cube with pure css seems weird and impossible at first, but since you can use :checked on input fields with CSS you can add interaction with some input fields like checkboxes. Or you can use media queries and/or container queries to also do some fun interactions. I ended up doing both.

For adding interaction with input buttons I first had to create some. And I figured, since I've already added a blooper to this project. Maybe let's not focus as much on accurately turning the cube as many times as possible. Let's instead add buttons that give you control over rotating the cube.

Now the challenge was, how can I create buttons to rotate the cube more than once per side. So I started thinking, what if I had four radio buttons for each side I wanted to rotate, and then when one is checked all the others are not visible. And repeat that logic so that you could turn each side 4 times like so.

And after experimenting a bit, it seemed to work flawlessly. This is what the CSS looked like.

```css
/* Radio buttons never shown and when one is checked the next is shown */
input[type="radio"],
input[type="radio"] + label {
  display: none;
}

section:nth-of-type(2):has(input[type="radio"]:nth-of-type(1):checked)
  input[type="radio"]:nth-of-type(2)
  + label {
  display: inline-block;
}

/* This is for rotating the right row of a cube. */
section:nth-of-type(2):has(input[type="radio"]:nth-of-type(24):checked)
  ~ div
  > div
  > div:nth-of-type(1),
section:nth-of-type(2):has(input[type="radio"]:nth-of-type(24):checked)
  ~ div
  > div
  > div:nth-of-type(2),
section:nth-of-type(2):has(input[type="radio"]:nth-of-type(24):checked)
  ~ div
  > div
  > div:nth-of-type(3),
section:nth-of-type(2):has(input[type="radio"]:nth-of-type(24):checked)
  ~ div
  > div
  > div:nth-of-type(10),
section:nth-of-type(2):has(input[type="radio"]:nth-of-type(24):checked)
  ~ div
  > div
  > div:nth-of-type(11),
section:nth-of-type(2):has(input[type="radio"]:nth-of-type(24):checked)
  ~ div
  > div
  > div:nth-of-type(12),
section:nth-of-type(2):has(input[type="radio"]:nth-of-type(24):checked)
  ~ div
  > div
  > div:nth-of-type(19),
section:nth-of-type(2):has(input[type="radio"]:nth-of-type(24):checked)
  ~ div
  > div
  > div:nth-of-type(20),
section:nth-of-type(2):has(input[type="radio"]:nth-of-type(24):checked)
  ~ div
  > div
  > div:nth-of-type(21) {
  --cube-single-rotate-y: 0deg;
}
```

This css is repeated 4 times for every rotate state of the cube. The values for the rotations are:

- 0
- 90
- 180
- 270

### Adding interaction with Container Queries

At the start of the subject I saw a presentation on container queries and the ability to resize a container with pure css. I thought that this would also be cool to implement in my concept. So I started experimenting with container queries and rotating based on the container size. Now if you want to use the resize interaction, you have to use `overflow: hidden`. Which meant that my cube would sometimes not be visible anymore when resizing.

If only I could only make my cube resize based on the container...

And obviously you can! And since I used custom properties for the cube size, all I had to do was just change one value:

```css
:root {
  /* from */
  --cube-sides: 3rem;
  /* to */
  --cube-sides: 10cqw;
}

/* The container and query */

body > div {
  overflow: hidden;
  height: 20cqw;
  width: 20cqw;
  container-type: inline-size;
  display: flex;
  justify-content: center;
  align-items: center;
}

@container (min-width: 300px) {
  section:nth-of-type(1):has(input:nth-of-type(2):checked)
    ~ div
    > div
    > div:nth-of-type(-n + 9) {
    --cube-single-rotate-z: -90deg;
  }
}
```

Which resulted in:

Below 300px

<img width="300" alt="image" src="https://user-images.githubusercontent.com/33430669/223988991-63e406b1-a261-432d-94a9-67f19ec27a28.png">

Above 300px

<img width="445" alt="image" src="https://user-images.githubusercontent.com/33430669/223989256-d71a846b-db93-4f00-879f-3da94bfd87e8.png">

Above 600px

<img width="599" alt="image" src="https://user-images.githubusercontent.com/33430669/223989326-2ef3b904-aefc-4799-8f68-739cab00424c.png">

### One of the selectors that makes all this possible :has

So I used the :has selector a couple of times in this project to create more logical html. Before the :has selector I was only capable of nesting all of the input fields inside the same html block element so I could select html siblings if something is checked. Now you can select a parent element and see if inside that parent element an input is checked and afterwards select all the siblings of that parent. Like so:
