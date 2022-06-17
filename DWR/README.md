# DWR
----------

## Dynamically adding converts and creators to DWR's config at runtime

    import org.directwebremoting.ServerContext;
    import org.directwebremoting.extend.ConverterManager;
    import org.directwebremoting.extend.CreatorManager;
    import org.directwebremoting.convert.BeanConverter;
    import org.directwebremoting.create.NewCreator;
    import org.directwebremoting.impl.StartupUtil;

    // Note: DWR servlet MUST have initialized before this!

    ServerContext serverContext = StartupUtil.getSingletonServerContext();

    ConverterManager converterManager = serverContext.getContainer().getBean(ConverterManager.class);
    BeanConverter converter = new BeanConverter();
    converter.setJavascript("Person");
    converterManager.addConverter("com.company.app.Person", converter);

    CreatorManager creatorManager = serverContext.getContainer().getBean(CreatorManager.class);
    NewCreator creator = new NewCreator();
    creator.setClassName("com.company.app.PersonDelegate");
    creator.setJavascript("PersonDelegate");
    creator.setScope("application");
    creatorManager.addCreator(creator.getJavascript(), creator);
