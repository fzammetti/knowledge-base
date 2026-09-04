# SmartmonTools

---

* [List attached drives](#5b1b6cac-eff6-4b04-a1c1-973c59d4875e)
* [List SMART attributes](#a8235c0a-eeb2-4f8a-b996-bb3aa18dd6b5)
* [Short test and then display results](#98454cc0-f492-4da0-a05c-7151dafbc778)
* [Long test and then display results](#98454cc0-f492-4da0-a05c-7151dafbc778)

---




<div id="5b1b6cac-eff6-4b04-a1c1-973c59d4875e">

## List attached drives

    smartctl --scan




<div id="a8235c0a-eeb2-4f8a-b996-bb3aa18dd6b5">

## List SMART attributes

(X) is the drive letter from the list attached drives command, like sdh

    smartctl --all /dev/sd(X)




<div id="98454cc0-f492-4da0-a05c-7151dafbc778">

## Short test and then display results

(X) is the drive letter from the list attached drives command, like sdh

    smartctl -t short /dev/sd(X)
    smartctl --all /dev/sd(X)




<div id="98454cc0-f492-4da0-a05c-7151dafbc778">

## Long test and then display results

(X) is the drive letter from the list attached drives command, like sdh

    smartctl -t long /dev/sd(X)
    smartctl --all /dev/sd(X)
