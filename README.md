# Toggle Buttons

A script to convert Radio Button Lists into single-select toggle buttons or CheckBox Lists into multi-select toggle buttons

https://github.com/stadium-software/toggle-buttons/assets/2085324/92ae0e4a-1405-4c31-9441-9105bff40ece

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
let scope = this;
let pageName = window.location.pathname.replace("/", "");
let selectSingleOption = (e) => {
    let container = e.target.closest(".stadium-toggle-button");
    let activeItem = container.querySelector(".active");
    if (activeItem) activeItem.classList.remove("active");
    let parent = e.target.closest(".radio");
    let input = parent.querySelector("input");
    if (!input.checked) {
        parent.classList.add("active");
        input.checked = true;
    } else {
        parent.classList.remove("active");
        input.checked = false;
        let inputName = container.id.replace(`${pageName}_`, "").replace("-container", "");
        scope[`${inputName}SelectedOption`] = {};
    }
    input.dispatchEvent(new Event("change"));
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
