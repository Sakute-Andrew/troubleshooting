# Hibernate configuration file unmarshlling

When transitioning from a traditional classpath-based Java project to a modularized JavaFX project, challenges often arise in processing XML-based configuration files like <code>hibernate.cfg.xml</code> This is primarily due to the stricter dependency management imposed by the module system

## Potential Causes

* **1. Missing JAXB Dependencies**

    The JAXB API and implementation are essential for XML marshalling and unmarshalling. Ensure they are correctly included in your project as dependencies.

    You can see this type of error:

    ```log
        INFO: HHH000412: Hibernate ORM core version [WORKING]
        org.hibernate.internal.util.config.ConfigurationException: Unable to perform unmarshalling at line number 0 and column 0 in RESOURCE hibernate.cfg.xml. Message: null
            at org.hibernate.orm.core@5.6.0.Final/org.hibernate.boot.cfgxml.internal.JaxbCfgProcessor.unmarshal(JaxbCfgProcessor.java:134)
            at org.hibernate.orm.core@5.6.0.Final/org.hibernate.boot.cfgxml.internal.JaxbCfgProcessor.unmarshal(JaxbCfgProcessor.java:66)
            at org.hibernate.orm.core@5.6.0.Final/org.hibernate.boot.cfgxml.internal.ConfigLoader.loadConfigXmlResource(ConfigLoader.java:57)
            at org.hibernate.orm.core@5.6.0.Final/org.hibernate.boot.registry.StandardServiceRegistryBuilder.configure(StandardServiceRegistryBuilder.java:254)
            at org.hibernate.orm.core@5.6.0.Final/org.hibernate.boot.registry.StandardServiceRegistryBuilder.configure(StandardServiceRegistryBuilder.java:243)
            at edu.avadev/edu.avadev.infrastructure.config.hibernate.HibernateUtil.<clinit>(HibernateUtil.java:20)
            at edu.avadev/edu.avadev.Main.start(Main.java:24)
            at javafx.graphics@19.0.2/com.sun.javafx.application.LauncherImpl.lambda$launchApplication1$9(LauncherImpl.java:847)
            at javafx.graphics@19.0.2/com.sun.javafx.application.PlatformImpl.lambda$runAndWait$12(PlatformImpl.java:484)
            at javafx.graphics@19.0.2/com.sun.javafx.application.PlatformImpl.lambda$runLater$10(PlatformImpl.java:457)
            at java.base/java.security.AccessController.doPrivileged(AccessController.java:399)
            at javafx.graphics@19.0.2/com.sun.javafx.application.PlatformImpl.lambda$runLater$11(PlatformImpl.java:456)
            at javafx.graphics@19.0.2/com.sun.glass.ui.InvokeLaterDispatcher$Future.run(InvokeLaterDispatcher.java:96)
            at javafx.graphics@19.0.2/com.sun.glass.ui.win.WinApplication._runLoop(Native Method)
            at javafx.graphics@19.0.2/com.sun.glass.ui.win.WinApplication.lambda$runLoop$3(WinApplication.java:184)
            at java.base/java.lang.Thread.run(Thread.java:1589)
        Caused by: javax.xml.bind.JAXBException: Implementation of JAXB-API has not been found on module path or classpath.
        - with linked exception:

    ```

    Download  [JAXB Runtime](https://mvnrepository.com/artifact/com.sun.xml.bind/jaxb-impl/2.3.1) module and [JAXB API](https://mvnrepository.com/artifact/javax.xml.bind/jaxb-api/2.3.1) into your dependency descriptor file.

* **2. Incorrect module declaration**

    The module descriptor ```module-info.java``` which located in ```src``` package might not correctly export necessary packages or require essential modules. This can hinder access to XML-related classes. This often happends with project, which uses [Java modules](https://www.oracle.com/corporate/features/understanding-java-9-modules.html) to configure project.

    In this case, you might simply forget to add module of ```org.hibernate.core``` to the module descriptor file. Here's example:

    ```java
    module edu.avadev {

    requires javafx.controls;
    requires javafx.fxml;

    requires java.naming;
    requires java.persistence;
    requires java.sql;

    requires  org.hibernate.orm.core;
    requires  org.xerial.sqlitejdbc;


    exports edu.avadev;
    exports edu.avadev.domain.enteties;
    exports edu.avadev.presentation.main;

    opens edu.avadev.domain.enteties to org.hibernate.orm.core;

    }
    ```
* **3.  **
