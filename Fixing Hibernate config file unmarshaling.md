<h1>Hibernate configuration file unmarshlling</h1>

<p>When transitioning from a traditional classpath-based Java project to a modularized JavaFX project, challenges often arise in processing XML-based configuration files like <code>hibernate.cfg.xml</code> This is primarily due to the stricter dependency management imposed by the module system.</p>

<h2>Potential Causes</h2>
<ul>
  <li>Missing JAXB Dependencies</li>
  <p>The JAXB API and implementation are essential for XML marshalling and unmarshalling. Ensure they are correctly included in your project as dependencies.</p>
  <p>Download [JAXB Runtime module](https://mvnrepository.com/artifact/com.sun.xml.bind/jaxb-impl/2.3.1) and [JAXB API](https://mvnrepository.com/artifact/javax.xml.bind/jaxb-api/2.3.1) into your dependency descriptor file</p>
  <li>Second item</li>
  <li>Third item</li>
  <li>Fourth item</li>
</ul>
