# Java

---

* [Get name of current method](#a57ba362-f8b8-45ea-83ba-00edb5197f54)
* [Double-brace (immediate) collection initialization](#bf7d687b-d7ec-4bf2-8617-f21add257ea6)

---




<div id="a57ba362-f8b8-45ea-83ba-00edb5197f54">

## Get name of current method

</div>

    String methodName = Thread.currentThread().getStackTrace()[1].getMethodName();

Note: this might be somewhat brittle, so might not be production-ready!




<div id="bf7d687b-d7ec-4bf2-8617-f21add257ea6">

## Double-brace (immediate) collection initialization

</div>

    List<string> countries = new ArrayList<string>() {{
      add("India");
      add("Switzerland");
      add("Italy");
      add("France");
      add("Germany");
    }};
