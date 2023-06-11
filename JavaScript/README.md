# JavaScript

---

* [ Display the type of an object](#2bdc60bb-ccd1-4bb8-a153-ced371254172)

---




<div id="2bdc60bb-ccd1-4bb8-a153-ced371254172">

##  Display the type of an object

</div>

    console.log(({}).toString.call(obj).match(/\s([a-zA-Z]+)/)[1].toLowerCase());
