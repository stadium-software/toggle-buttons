# Toggle Buttons

A script to convert Radio Button Lists into toggle buttons

## Sample applications
This repo contains one Stadium 6.7 application
[ToggleButtons.sapz](Stadium6/ToggleButtons.sapz?raw=true)

# Version 
1.0 Initial

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

## Global Script Setup
1. Create a Global Script called "ToggleButton"
3. Drag a *JavaScript* action into the script
4. Add the Javascript below into the JavaScript code property
```javascript
/* Stadium Script Version 1.0 */
let selectOption = (e) => {
    let parent = e.target.closest(".stadium-button-toggle");
    if (parent.querySelector(".stadium-toggle-button.active")) parent.querySelector(".stadium-toggle-button.active").classList.remove("active");
    e.target.classList.add("active");
    let input = e.target.closest(".radio").querySelector("input");
    if (!input.checked) {
        input.checked = true;
        input.dispatchEvent(new Event("change"));
    }
};
let options = {
        childList: true,
        subtree: true,
    },
    observer = new MutationObserver(initToggle);

initToggle();

function initToggle() {
    observer.disconnect();
    let toggles = document.querySelectorAll(".stadium-button-toggle");
    for (let i = 0; i < toggles.length; i++) {
        let buttons = toggles[i].querySelectorAll(".stadium-toggle-button");
        for (let j = 0; j < buttons.length; j++) {
            buttons[j].remove();
        }
        let radios = toggles[i].querySelectorAll(".radio");
        for (let j = 0; j < radios.length; j++) {
            let button = document.createElement("button");
            button.classList.add("btn", "btn-lg", "btn-default", "stadium-toggle-button");
            let input = radios[j].querySelector("input");
            if (input.checked) {
                button.classList.add("active");
            }
            console.log(input.getAttribute("disabled"));
            if (input.getAttribute("disabled") !== null) {
                button.setAttribute("disabled", "disabled");
            }
            button.addEventListener("click", selectOption);
            let buttonText = document.createTextNode(radios[j].querySelector("label").textContent);
            button.appendChild(buttonText);
            radios[j].appendChild(button);
        }
        observer.observe(toggles[i], options);
    }
}
```

## Page Setup
1. Drag *RadioButtonList* control to a page
2. Add a class called "stadium-button-toggle" to the control classes property

## Page.Load Setup
1. Drag the Global Script called "ToggleButton" into the Page.Load event handler

# Styling
Various elements in this module can be styled using the two CSS files in this repo

## Applying the CSS

**Stadium 6.6 or higher**
1. Create a folder called "CSS" inside of your Embedded Files in your application
2. Drag the two CSS files from this repo [*toggle-button-variables.css*](toggle-button-variables.css) and [*toggle-button.css*](toggle-button.css) into that folder
3. Paste the link tags below into the *head* property of your application
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/toggle-button.css">
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/toggle-button-variables.css">
``` 

![](images/ApplicationHeadProp.png)

**Versions lower than 6.6**
1. Copy the CSS from the two css files into the Stylesheet in your application

## Customising CSS
1. Open the CSS file called [*toggle-button-variables.css*](toggle-button-variables.css) from this repo
2. Adjust the variables in the *:root* element as you see fit
3. Overwrite the file in the CSS folder of your application with the customised file

## CSS Upgrading
To upgrade the CSS in this module, follow the [steps outlined in this repo](https://github.com/stadium-software/samples-upgrading)
