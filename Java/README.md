# Java
----------

------------------------------------------------------------------------------

## Get name of current method:

    String methodName = Thread.currentThread().getStackTrace()[1].getMethodName();

  Note: this might be somewhat brittle, so might not be production-ready!

## Double-brace (immediate) collection initialization:

    List<string> countries = new ArrayList<string>() {{
      add("India");
      add("Switzerland");
      add("Italy");
      add("France");
      add("Germany");
    }};
