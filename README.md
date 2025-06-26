# Toggle Buttons

A script to convert RadioButtonList controls into single-select toggle buttons or CheckBoxList controls into multi-select toggle buttons

https://github.com/stadium-software/toggle-buttons/assets/2085324/6f9071b7-86fd-4344-9579-cf0842bcecda

# Version 
1.1 Made single-select work like Radio Button List (removed ability to de-select)

1.1.1 Updated readme to 6.12+; Converted px to rem

1.1.2 Adjusted border color for selected items

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

## Global Script Setup
1. Create a Global Script called "ToggleButton"
3. Drag a *JavaScript* action into the script
4. Add the Javascript below unchanged into the JavaScript code property
```javascript
/* Stadium Script Version 1.1 */
let selectSingleOption = (e) => {
    let container = e.target.closest(".stadium-toggle-button");
    let parent = e.target.closest(".radio");
    let input = parent.querySelector("input");
    if (!input.checked) {
        let activeItem = container.querySelector(".active");
        if (activeItem) activeItem.classList.remove("active");
        parent.classList.add("active");
        input.checked = true;
        input.dispatchEvent(new Event("change"));
    }
};
let selectOption = (e) => {
    let parent = e.target.closest(".checkbox");
    let input = parent.querySelector("input");
    parent.classList.toggle("active");
    input.checked = !input.checked;
    input.dispatchEvent(new Event("change"));
};
let initToggle = () => {
    observer.disconnect();
    let toggles = document.querySelectorAll(".stadium-toggle-button");
    for (let i = 0; i < toggles.length; i++) {
        let buttons = toggles[i].querySelectorAll(".toggle-button");
        for (let j = 0; j < buttons.length; j++) {
            buttons[j].remove();
        }
        let inputContainer = toggles[i].querySelectorAll(".checkbox, .radio");
        for (let j = 0; j < inputContainer.length; j++) {
            let button = document.createElement("button");
            button.classList.add("btn", "btn-lg", "btn-default", "toggle-button");
            let input = inputContainer[j].querySelector("input");
            if (input.checked) {
                inputContainer[j].classList.add("active");
            }
            if (input.getAttribute("disabled") !== null) {
                button.setAttribute("disabled", "disabled");
            }
            if (toggles[i].classList.contains("check-box-list-container")) {
                button.addEventListener("click", selectOption);
            } else {
                button.addEventListener("click", selectSingleOption);
            }
            let buttonText = document.createTextNode(inputContainer[j].querySelector("label").textContent);
            button.appendChild(buttonText);
            inputContainer[j].appendChild(button);
        }
        observer.observe(toggles[i], options);
    }
};
let options = {
        childList: true,
        subtree: true,
    },
    observer = new MutationObserver(initToggle);
initToggle();
```

## Single-Select Setup
1. Drag *RadioButtonList* control to a page
2. Add a class called "stadium-toggle-button" to the control classes property

## Multi-Select Setup
1. Drag *CheckBoxList* control to a page
2. Add a class called "stadium-toggle-button" to the control classes property

## Page.Load Setup
1. Drag the Global Script called "ToggleButton" into the Page.Load event handler

## CSS
The CSS below is required for the correct functioning of the module. Variables exposed in the [*toggle-button-variables.css*](toggle-button-variables.css) file can be [customised](#customising-css).

### Before v6.12
1. Create a folder called "CSS" inside of your Embedded Files in your application
2. Drag the two CSS files from this repo [*toggle-button-variables.css*](toggle-button-variables.css) and [*toggle-button.css*](toggle-button.css) into that folder
3. Paste the link tags below into the *head* property of your application
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/toggle-button.css">
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/toggle-button-variables.css">
``` 

### v6.12+
1. Create a folder called "CSS" inside of your Embedded Files in your application
2. Drag the CSS files from this repo [*toggle-button.css*](toggle-button.css) into that folder
3. Paste the link tag below into the *head* property of your application
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/toggle-button.css">
``` 

### Customising CSS
1. Open the CSS file called [*toggle-button-variables.css*](toggle-button-variables.css) from this repo
2. Adjust the variables in the *:root* element as you see fit
3. Stadium 6.12+ users can comment out any variable they do **not** want to customise
4. Add the [*toggle-button-variables.css*](toggle-button-variables.css) to the "CSS" folder in the EmbeddedFiles (overwrite)
5. Paste the link tag below into the *head* property of your application (if you don't already have it there)
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/toggle-button-variables.css">
``` 
6. Add the file to the "CSS" inside of your Embedded Files in your application

**NOTE: Do not change any of the CSS in the 'toggle-button.css' file**

## Upgrading Stadium Repos
Stadium Repos are not static. They change as additional features are added and bugs are fixed. Using the right method to work with Stadium Repos allows for upgrading them in a controlled manner. 

How to use and update application repos is described here: [Working with Stadium Repos](https://github.com/stadium-software/samples-upgrading)