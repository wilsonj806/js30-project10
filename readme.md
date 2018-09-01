# Notes for Project 10 Hold Shift and Check Checkboxes

# New methods/ functions:
```javascript
event.checked
event.shiftKey
this.domElement
```

## Logic/ architecture

### Solution explained

The solution that Wes provides is a lot slicker than my solution that I came up with. 

It still starts with accessing the input element that's a descendant of the element(s) with the class `.inbox` though
```javascript
const checkboxes = document.querySelectoryAll('.inobx input[type="checkbox"]');
```
We also declare a variable that keeps track of which element we're currently at in a later loop.
```javascript
let lastChecked;
```

We then add an event listener for each checkbox element, which can be seen below expanded:
```javascript
checkboxes.forEach(function (checkbox){
    checkbox.addEventListener('click', handleCheck);
});
```
The main function that handles the check boxes looks like this:
```javascript
function handleCheck(e){
    let inBetween = false; 
    if (this.checked && e.shiftKey){
    // start the shift clicking
        checkboxes.forEach(checkbox =>{ 
            if (checkbox === this || checkbox === lastChecked){
                console.log('starting checkbox sweep'); 
                inBetween = !inBetween; // if checkbox is the current one that's' checked, then set inBetween = the inverse value
            }

            if (inBetween){
                checkbox.checked = true;
            }
        });
    }
    lastChecked = this; // is the current checkbox being clicked
}
```

For the main function that handles the shift clicking, it needs to do the following:
- Declare an intermediary variable `inBetween` and set it to false
    - This variable is meant to check if the current checkbox element being examined is the last checkbox that the function examined or if it's the checkbox that triggered the event listener
- Check that the Shift key is pressed down
- Check that a checkbox is "checked"
- If both conditions are met, then the following needs to happen:
    - We initiate a `forEach()` that does the below for every checkbox element
        - First, we need to check whether or not the checkbox being examined right now is either:
            a. The checkbox that triggered the event listener
            b. **OR** the checkbox that was being examined last
        - If it is the above, then the value of `inBetween` is set to the inverse of itself
        - Finally we check if `inBetween` is true and if it is toggle `checkbox.checked = true;`
    - When the `forEach()` loop finishes, we then set the value of `lastChecked` to `this`
        - `this` here means the current DOM element being examined

### My solution/ train of thought explained

So my solution used array methods to achieve the same solution that Wes got. However, I ended up checking the solution and realized that my solution, while it would have been logically valid, was significantly more complex and decided to drop it. 

Below is what I had before I decided to check the solution:
```javascript
const domInput = [...document.querySelectorAll('input[type=checkbox]')];

let currIndex = 0;
let currTime = 0;
const currArr = [currIndex,currTime];

function inputDetect(e){
if (this.checked && e.shiftKey){ // this in the context of event listeners means the element that triggered the event listener
domInput.forEach(box =>{
// Intent was to add another eventlistener?
})
}
currIndex = domInput.findIndex(index =>index.checked ==true); // currIndex and currTime could be it's own function
currTime = e.timeStamp;
}

domInput.forEach(index => index.addEventListener('click',inputDetect));
```

The logic for this is similar to Wes's solution, but involved the following:
- First we retrieve all the DOM elements that are checkboxes and spread those values in an array
    - Note: this is overkill
- From there we declare two variables and an array:
    1. The `currIndex` variable
    2. The `currTime` variable
    3. The `currArr` variable, which takes `currIndex` and `currTime` as values in the array
- We then add an event listener that listens for the `'click'` event and then passes it into the callback function: `inputDetect`
```javascript
    domInput.forEach(index => index.addEventListener('click',inputDetect));
```
- Within the function `inputDetect` it would do the following:
    - Similar to Wes's solution, we check if the Shift key has been pressed **AND** if a checkbox has been checked
- The rest of the solution wasn't finished but the intent was to first retrieve the current index and time and then wait for the next click.
- When that happens, I would have gotten the index of the checkbox that just got clicked and then add a while loop that goes through the array and sets the state of `DOMElement.checked` to true

This solution has several problems and they are as follows:
    1. Ignoring everything else, this is kind of aggresively complex and hard to follow
    2. It would probably only work in one direction
    3. In general there are significantly simpler ways to track the state of a DOM element

