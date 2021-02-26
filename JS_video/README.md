

# Intro to JavaScript
## Introduction
We’ve learned how to make the structure of websites, but how do we make it interactive? For that, we use JavaScript. JavaScript is a language used by websites to create functionality achievable with pure HTML. With JavaScript, we can make our website interactive and easier to use.

## Using JavaScript in webpages
First, we’ll talk about how to use JavaScript on our website. We may want to put the JavaScript in a separate file, but for now we can put it in a special HTML tag called script. Within this script tag, we can write JavaScript code which will be run when the website loads. First, we can write a simple print statement.

    <script>
        console.log("hello world");
    </script>

We can open the file in the browser and see that "hello world" is printed in the console.

    >> hello world

Now, let's try to do something a little more visual. If we want to interact with the HTML document, we have to use the Document Object Model, or DOM. Recall that we can give HTML elements both id's and classes. Before, we've used this to style elements using CSS, but we can also use them in JavaScript. Using the `document` keyword, we can get access to the elements. First, we can try modifying the style of an element.

    <div id="test">Test element</div>
    <script>
        let test = document.getElementById("test");
        test.style.color= "red";
    </script>
Here, we've created an HTML `div` with the id `test`. In the JavaScript code, we declared a variable named `test` using the `let` keyword. JavaScript has weak typing, meaning that we don't need to define the types of variables when declaring them. We've assigned to the variable `document.getElementById("test")`. This is a reference to the `div` element we created earlier. Next, we can access the styles of the element and set the value of `color` to red. Sure enough, when we open the file we can see that the text is red.

## Callbacks
What if we want this to happen interactively, however, instead of when the page loads? To do this, we need to use callbacks. A callback is a function which is called at some point in the future such as after a certain period of time or after some event.

### Timeouts
First, let's try to update the style one second after the page loads. We'll change the JavaScript code to do this.

    let test = document.getElementById("test");
    function updateColor() {
        test.style.color= "red";
    }
    setTimeout(updateColor, 1000);
Here, we've define a function called `updateColor`. Again, since JavaScript is weakly typed, we don't need to give the function a return type. Next, we use the built-in function `setTimeout` and give it the function as a parameter. JavaScript treats functions as first-class citizens, so we could use them as we could any other variable, including passing them as a parameter. We also give it the value of `1000`, which is the delay in milliseconds. This code will do the same thing as the previous example, but it will delay the style change by one second.

### Intervals
JavaScript also lets us use callbacks as repeating tasks. To do this, we can use `setInterval`.

    let test = document.getElementById("test");
    setInterval(function () {
        test.innerText += "test";
    }, 1000);
In this example, we're using the `innerText` attribute to append "test" each time the function is run. We can see that when we load the file, the element adds another "test" every second. Also, we've changed the callback function. Instead of defining a separate function, we're passing an anonymous function to `setInterval`. This makes the code shorter and easier to read.

### Event handling
Every HTML element has some built-in events we can use for interactivity. Here, we'll use the `onClick` event.

    const test = document.getElementById("test");
    test.onclick = () => {
        test.style.color = "blue";
    };
Here, we've assigned to the `onClick` attribute of test a new callback function which changes the text color of the `div` to blue. We've also again changed the callback from an anonymous function to a lambda function. This is an even more concise way of writing functions, and it's what we usually use for callbacks. Additionally, we've changed `let` to `const` to indicate that we have no plans on changing the value of that variable. This isn't required, but it makes the code easier to understand for other programmers.

## Example: Bouncing logo
Let's put some of what we learned to use. Here's all the code to produce the bouncing logo.

    <head>
        <style>
            body, html {
                height: 100vh;
                overflow: hidden;
            }
            #logo {
                background-color: #f7df1e;
                height: 100px;
                width: 100px;
                padding-right: 7px;
                font-family: sans-serif;
                font-weight: bold;
                font-size: 55px;
                display: flex;
                justify-content: flex-end;
                align-items: flex-end;
                align-content: flex-end;
            }
        </style>
    </head>
    <body>
        <div id="logo">JS</div>
        <script>
            let logo = document.getElementById("logo");
            let x = 0;
            let y = 0;
            let dx = -1;
            let dy = -1;
            let size = logo.clientWidth;
            setInterval(() => {
                x += dx;
                y += dy;
                if (x <= 0 || x >= document.body.clientWidth - size) {
                    dx *= -1;
                    x += dx;
                }
                if (y <= 0 || y >= document.body.clientHeight - size) {
                    dy *= -1;
                    y += dy;
                }
                logo.style.marginLeft = x + "px";
                logo.style.marginTop = y + "px";
            }, 10);
        </script>
    </body>
That's a lot of code, but if we break it down it's not that complicated. First, we define that style of the logo, which is a yellow square with "JS" in the bottom right corner. We also make sure the document takes up all of the vertical room of the browser.

    body, html {
        height: 100vh;
        overflow: hidden;
    }
    #logo {
        background-color: #f7df1e;
        height: 100px;
        width: 100px;
        padding-right: 7px;
        font-family: sans-serif;
        font-weight: bold;
        font-size: 55px;
        display: flex;
        justify-content: flex-end;
        align-items: flex-end;
        align-content: flex-end;
    }
Next, we define the logo element, which is pretty self-explanatory.

    <div id="logo">JS</div>
Now, we start our JavaScript code. First, let's declare all of our variables. We get the logo element in the same way we have before, then we define a few variables to help with the animation. We have `x` and `y`, which are the coordinates of the logo bouncing around on the screen. Next are `dx` and `dy`. These are the current directions of the logo, in both the horizontal and vertical direction. We start out with both variables being negative so as to not cause the logo to detect a collision right at the start. Finally is `size`, which is the width of the logo (we're assuming this is the same as the height). We use `logo.clientWidth` instead of `logo.style.width` because this will give us an exact numerical representation of the width in pixels, as opposed to some other unit.

    const logo = document.getElementById("logo");
    let x = 0;
    let y = 0;
    let dx = -1;
    let dy = -1;
    const size = logo.clientWidth;
Finally, we add the code to actually animate the logo. We have put all of this in `setInterval`, so it will repeat (in this case every 10 milliseconds). First, we add the direction variables to `x` and `y`. If they are positive, it will move down and to the right, respectively. If they are negative, it will move up and to the left. Next, we check if there has been a horizontal collision. We can do this by checking if the x-coordinate is less than 0, which would indicate that it has hit the left wall, or if the x-coordinate is greater than the total width minus the size of the logo, which would indicate it has hit the right wall (we need to subtract the size of the logo because the coordinates correspond to the top left corner of the logo). We can do the same with the y-coordinate. If a collision has been detected, we flip the direction and undo the movement for that frame so we don't go out-of-bounds. Finally, we set the location of the logo (using margins) in pixels.

    setInterval(() => {
        x += dx;
        y += dy;
        if (x <= 0 || x >= document.body.clientWidth - size) {
            dx *= -1;
            x += dx;
        }
        if (y <= 0 || y >= document.body.clientHeight - size) {
            dy *= -1;
            y += dy;
        }
        logo.style.marginLeft = x + "px";
        logo.style.marginTop = y + "px";
    }, 10);

This may seem a bit overwhelming, but it's really just what we were doing before with some added functionality. After completing this example, take a moment to make sure you understand what we just went over.
