# JavaScript

---

* [Display the type of an object](#2bdc60bb-ccd1-4bb8-a153-ced371254172)
* [List all functions on the window object](#386a032f-27b7-487b-abc0-a2e2a76102ed)

---




<div id="2bdc60bb-ccd1-4bb8-a153-ced371254172">

##  Display the type of an object

</div>

    console.log(({}).toString.call(obj).match(/\s([a-zA-Z]+)/)[1].toLowerCase());




<div id="386a032f-27b7-487b-abc0-a2e2a76102ed">

##  List all functions on the window object

</div>

    // Execute this line of code in the immediate line of browser dev tools' Console tab
    Object.keys(window).filter(x => typeof(window[x]) === "function")
