# Java

---

* [Get name of current method](#a57ba362-f8b8-45ea-83ba-00edb5197f54)
* [Double-brace (immediate) collection initialization](#bf7d687b-d7ec-4bf2-8617-f21add257ea6)
* [Log names of columns in a JDBC ResultSet](#80ed6874-df8e-46ff-9891-7021aab1da8b)
* [Modify the log level on the Log4J Log instance of a class programmatically](#32de3b09-2eec-444d-8b8a-318980fa3e1b)
* [Parameterized logging](#f89dcb74-b985-4386-bdbc-788e0bdfa036)

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

Note: this is no longer the best way to do this (maybe it never was!).  There are methods on collections classes now
that allow you to do this in a cleaner way, so you should probably look those up instead.




<div id="80ed6874-df8e-46ff-9891-7021aab1da8b">

## Log names of columns in a JDBC ResultSet

</div>

    // rs is a JDBC ResultSet
    java.sql.ResultSetMetaData rsmd = rs.getMetaData();
    for (int i = 1; i <= rsmd.getColumnCount(); i++ ) {
      System.out.println(rs.getColumnName(i));
    }




<div id="32de3b09-2eec-444d-8b8a-318980fa3e1b">

## Modify the log level on the Log4J Log instance of a class programmatically

</div>

    import org.apache.logging.log4j.Level;
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    Logger log4jLogger = LogManager.getLogger("<FULLY_QUALIFIED_CLASS_NAME>");
    ((org.apache.logging.log4j.core.Logger)log4jLogger).setLevel(Level.INFO);




<div id="f89dcb74-b985-4386-bdbc-788e0bdfa036">

## Parameterized logging

</div>

You can use placeholders in log statements like so:

    LOG.debug("Value is {}", value);

The benefit is that the logging framework will format the message only if the log level is enabled, avoiding the
overhead of discarding premature string concatenation, as well as avoiding the need for code guards around log
statements.
