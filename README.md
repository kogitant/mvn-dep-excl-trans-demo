# prj-1
The parent pom.xm in this project has a dependency management section that declares how solr-core dependency should be imported:

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.apache.solr</groupId>
                <artifactId>solr-core</artifactId>
                <version>4.10.3</version>
                <exclusions>
                    <exclusion>
                        <groupId>org.eclipse.jetty</groupId>
                        <artifactId>jetty-server</artifactId>
                    </exclusion>
                    <exclusion>
                        <groupId>org.eclipse.jetty</groupId>
                        <artifactId>jetty-servlet</artifactId>
                    </exclusion>
                    <exclusion>
                        <groupId>org.eclipse.jetty</groupId>
                        <artifactId>jetty-servlets</artifactId>
                    </exclusion>
                    
                    <!-- no other jetty dependencies ignored -->
                    <!-- all other, non-jetty dependencies ignored --> 
                </exclusions>
            </dependency>
        </dependencies>
    </dependencyManagement>




What the above achieves **within prj-1** is that whenever prj-1 modules use solr-core as a dependency, it will not bring jetty-ser* dependencies as **transitive dependencies**.
This works exactly like it should within prj-1. This can be verified with prj-1/mod-e, which declares solr-core as a dependency like this:
 
      <artifactId>prj1-mod-e</artifactId>
    
        <dependencies>
            <dependency>
                <groupId>org.apache.solr</groupId>
                <artifactId>solr-core</artifactId>
            </dependency>
        </dependencies>

mvn dependency:tree of the module displays solr-core as a dependency, and it brings all other jetty dependencies, but **NOT jetty-ser** deps:

    [INFO] ------------------------------------------------------------------------
    [INFO] Building prj1-mod-e 1.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ prj1-mod-e ---
    [INFO] com.koant.maven.demo:prj1-mod-e:jar:1.0-SNAPSHOT
    [INFO] \- org.apache.solr:solr-core:jar:4.10.3:compile
    [INFO]    +- org.eclipse.jetty:jetty-continuation:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-deploy:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-http:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-io:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-jmx:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-security:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-util:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-webapp:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-xml:jar:8.1.10.v20130312:compile
    [INFO]    \- org.eclipse.jetty.orbit:javax.servlet:jar:3.0.0.v201112011016:compile
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary:
    [INFO] 
    [INFO] prj-1 .............................................. SUCCESS [  0.482 s]
    [INFO] prj1-mod-a ......................................... SUCCESS [  0.069 s]
    [INFO] prj1-mod-b ......................................... SUCCESS [  0.016 s]
    [INFO] prj1-mod-c ......................................... SUCCESS [  0.007 s]
    [INFO] prj1-mod-d ......................................... SUCCESS [  0.164 s]
    [INFO] prj1-mod-e ......................................... SUCCESS [  0.007 s]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------




Dependency tree of prj-1/mod-d shows which transitive dependencies would be imported without the excludes defined in the parent pom.xml:

    [INFO] ------------------------------------------------------------------------
    [INFO] Building prj1-mod-d 0.0.1-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ prj1-mod-d ---
    [INFO] com.koant.maven.demo:prj1-mod-d:jar:0.0.1-SNAPSHOT
    [INFO] \- org.apache.solr:solr-core:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-analyzers-common:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-analyzers-kuromoji:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-analyzers-phonetic:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-codecs:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-core:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-expressions:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-grouping:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-highlighter:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-join:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-memory:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-misc:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-queries:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-queryparser:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-spatial:jar:4.10.3:compile
    [INFO]    +- org.apache.lucene:lucene-suggest:jar:4.10.3:compile
    [INFO]    +- org.apache.solr:solr-solrj:jar:4.10.3:compile
    [INFO]    +- com.carrotsearch:hppc:jar:0.5.2:compile
    [INFO]    +- com.google.guava:guava:jar:14.0.1:compile
    [INFO]    +- com.google.protobuf:protobuf-java:jar:2.5.0:compile
    [INFO]    +- com.googlecode.concurrentlinkedhashmap:concurrentlinkedhashmap-lru:jar:1.2:compile
    [INFO]    +- com.spatial4j:spatial4j:jar:0.4.1:compile
    [INFO]    +- commons-cli:commons-cli:jar:1.2:compile
    [INFO]    +- commons-codec:commons-codec:jar:1.9:compile
    [INFO]    +- commons-configuration:commons-configuration:jar:1.6:compile
    [INFO]    +- commons-fileupload:commons-fileupload:jar:1.2.1:compile
    [INFO]    +- commons-io:commons-io:jar:2.3:compile
    [INFO]    +- commons-lang:commons-lang:jar:2.6:compile
    [INFO]    +- dom4j:dom4j:jar:1.6.1:compile
    [INFO]    +- joda-time:joda-time:jar:2.2:compile
    [INFO]    +- log4j:log4j:jar:1.2.17:compile
    [INFO]    +- org.antlr:antlr-runtime:jar:3.5:compile
    [INFO]    +- org.apache.hadoop:hadoop-annotations:jar:2.2.0:compile
    [INFO]    +- org.apache.hadoop:hadoop-auth:jar:2.2.0:compile
    [INFO]    +- org.apache.hadoop:hadoop-common:jar:2.2.0:compile
    [INFO]    +- org.apache.hadoop:hadoop-hdfs:jar:2.2.0:compile
    [INFO]    +- org.apache.httpcomponents:httpclient:jar:4.3.1:compile
    [INFO]    +- org.apache.httpcomponents:httpcore:jar:4.3:compile
    [INFO]    +- org.apache.httpcomponents:httpmime:jar:4.3.1:compile
    [INFO]    +- org.apache.zookeeper:zookeeper:jar:3.4.6:compile
    [INFO]    +- org.codehaus.woodstox:wstx-asl:jar:3.2.7:compile
    [INFO]    +- org.eclipse.jetty:jetty-continuation:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-deploy:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-http:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-io:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-jmx:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-security:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-server:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-servlet:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-util:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-webapp:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-xml:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty.orbit:javax.servlet:jar:3.0.0.v201112011016:compile
    [INFO]    +- org.noggit:noggit:jar:0.5:compile
    [INFO]    +- org.ow2.asm:asm:jar:4.1:compile
    [INFO]    +- org.ow2.asm:asm-commons:jar:4.1:compile
    [INFO]    +- org.restlet.jee:org.restlet:jar:2.1.1:compile
    [INFO]    +- org.restlet.jee:org.restlet.ext.servlet:jar:2.1.1:compile
    [INFO]    \- org.slf4j:slf4j-api:jar:1.7.6:compile
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------


prj1-mod-a defines the solr-core dependency like this:

        <dependencies>
            <dependency>
                <groupId>org.apache.solr</groupId>
                <artifactId>solr-core</artifactId>
                <!-- In addition to the excludes defined in parent pom dependencyManagement section, exclude these : -->
                <exclusions>
                    <exclusion>
                        <groupId>org.eclipse.jetty</groupId>
                        <artifactId>jetty-continuation</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>
        </dependencies>

What this achieves is that **in addition** to the excludes defined in the parent pom dependencyManagement section, the continuation dependency is excluded:

    [INFO] ------------------------------------------------------------------------
    [INFO] Building prj1-mod-a 1.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ prj1-mod-a ---
    [INFO] com.koant.maven.demo:prj1-mod-a:jar:1.0-SNAPSHOT
    [INFO] \- org.apache.solr:solr-core:jar:4.10.3:compile
    [INFO]    +- org.eclipse.jetty:jetty-continuation:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-deploy:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-http:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-io:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-jmx:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-security:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-util:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-webapp:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-xml:jar:8.1.10.v20130312:compile
    [INFO]    \- org.eclipse.jetty.orbit:javax.servlet:jar:3.0.0.v201112011016:compile
    [INFO]                                                                         
    [INFO] ------------------------------------------------------------------------


This works very well within prj1. This can be verified with depdendency tree of prj1-mod-b:

       [INFO] ------------------------------------------------------------------------
       [INFO] Building prj1-mod-b 1.0-SNAPSHOT
       [INFO] ------------------------------------------------------------------------
       [INFO] 
       [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ prj1-mod-b ---
       [INFO] com.koant.maven.demo:prj1-mod-b:jar:1.0-SNAPSHOT
       [INFO] \- com.koant.maven.demo:prj1-mod-a:jar:1.0-SNAPSHOT:compile
       [INFO]    \- org.apache.solr:solr-core:jar:4.10.3:compile
       [INFO]       +- org.eclipse.jetty:jetty-continuation:jar:8.1.10.v20130312:compile
       [INFO]       +- org.eclipse.jetty:jetty-deploy:jar:8.1.10.v20130312:compile
       [INFO]       +- org.eclipse.jetty:jetty-http:jar:8.1.10.v20130312:compile
       [INFO]       +- org.eclipse.jetty:jetty-io:jar:8.1.10.v20130312:compile
       [INFO]       +- org.eclipse.jetty:jetty-jmx:jar:8.1.10.v20130312:compile
       [INFO]       +- org.eclipse.jetty:jetty-security:jar:8.1.10.v20130312:compile
       [INFO]       +- org.eclipse.jetty:jetty-util:jar:8.1.10.v20130312:compile
       [INFO]       +- org.eclipse.jetty:jetty-webapp:jar:8.1.10.v20130312:compile
       [INFO]       +- org.eclipse.jetty:jetty-xml:jar:8.1.10.v20130312:compile
       [INFO]       \- org.eclipse.jetty.orbit:javax.servlet:jar:3.0.0.v201112011016:compile
       [INFO]                                                                         
       [INFO] ------------------------------------------------------------------------

As can be seen from above, prj1-mod-b depends on prj1-mod-a, which brings solr-core to prj1-mod-b.
But the depdendency excludes defined in prj1-mod-a and in the prj1 parent pom.xm are in effect and no jetty-ser* or jetty-continuation dependencies appear.



# prj-2
Things get funky in prj-2. In parent pom.xml of prj-2 there's no dependencyManagement section.
prj2-mod-d introduces prj1-mod-a as a dependency. Here's what prj2-mod-d dependencies section looks like:


    <dependencies>
        <dependency>
            <groupId>com.koant.maven.demo</groupId>
            <artifactId>prj2-mod-c</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>com.koant.maven.demo</groupId>
            <artifactId>prj1-mod-a</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>


That **prj1-mod-a** is the same one from **prj-1**, discussed above, which dependes on solr-core, but excluded

* all non-jetty dependencies 
* jetty-ser* (thanks to prj-1 parent pom.xml dependencyManagement)
* jetty-continuation (thanks to prj1-mod-a additional depdendency excludes)


So, how will the dependency tree of prj2-mod-d look like related to solr-core? Will the excludes defined in prj1 apply?
Here's what happens:

    [INFO] ------------------------------------------------------------------------
    [INFO] Building prj2-mod-d 1.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ prj2-mod-d ---
    [INFO] com.koant.maven.demo:prj2-mod-d:jar:1.0-SNAPSHOT
    [INFO] +- com.koant.maven.demo:prj2-mod-c:jar:1.0-SNAPSHOT:compile
    [INFO] |  \- com.koant.maven.demo:prj2-mod-b:jar:1.0-SNAPSHOT:compile
    [INFO] \- com.koant.maven.demo:prj1-mod-a:jar:1.0-SNAPSHOT:compile
    [INFO]    \- org.apache.solr:solr-core:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-analyzers-common:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-analyzers-kuromoji:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-analyzers-phonetic:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-codecs:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-core:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-expressions:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-grouping:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-highlighter:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-join:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-memory:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-misc:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-queries:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-queryparser:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-spatial:jar:4.10.3:compile
    [INFO]       +- org.apache.lucene:lucene-suggest:jar:4.10.3:compile
    [INFO]       +- org.apache.solr:solr-solrj:jar:4.10.3:compile
    [INFO]       +- com.carrotsearch:hppc:jar:0.5.2:compile
    [INFO]       +- com.google.guava:guava:jar:14.0.1:compile
    [INFO]       +- com.google.protobuf:protobuf-java:jar:2.5.0:compile
    [INFO]       +- com.googlecode.concurrentlinkedhashmap:concurrentlinkedhashmap-lru:jar:1.2:compile
    [INFO]       +- com.spatial4j:spatial4j:jar:0.4.1:compile
    [INFO]       +- commons-cli:commons-cli:jar:1.2:compile
    [INFO]       +- commons-codec:commons-codec:jar:1.9:compile
    [INFO]       +- commons-configuration:commons-configuration:jar:1.6:compile
    [INFO]       +- commons-fileupload:commons-fileupload:jar:1.2.1:compile
    [INFO]       +- commons-io:commons-io:jar:2.3:compile
    [INFO]       +- commons-lang:commons-lang:jar:2.6:compile
    [INFO]       +- dom4j:dom4j:jar:1.6.1:compile
    [INFO]       +- joda-time:joda-time:jar:2.2:compile
    [INFO]       +- log4j:log4j:jar:1.2.17:compile
    [INFO]       +- org.antlr:antlr-runtime:jar:3.5:compile
    [INFO]       +- org.apache.hadoop:hadoop-annotations:jar:2.2.0:compile
    [INFO]       +- org.apache.hadoop:hadoop-auth:jar:2.2.0:compile
    [INFO]       +- org.apache.hadoop:hadoop-common:jar:2.2.0:compile
    [INFO]       +- org.apache.hadoop:hadoop-hdfs:jar:2.2.0:compile
    [INFO]       +- org.apache.httpcomponents:httpclient:jar:4.3.1:compile
    [INFO]       +- org.apache.httpcomponents:httpcore:jar:4.3:compile
    [INFO]       +- org.apache.httpcomponents:httpmime:jar:4.3.1:compile
    [INFO]       +- org.apache.zookeeper:zookeeper:jar:3.4.6:compile
    [INFO]       +- org.codehaus.woodstox:wstx-asl:jar:3.2.7:compile
    [INFO]       +- org.eclipse.jetty:jetty-continuation:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-deploy:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-http:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-io:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-jmx:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-security:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-server:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-servlet:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-util:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-webapp:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-xml:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty.orbit:javax.servlet:jar:3.0.0.v201112011016:compile
    [INFO]       +- org.noggit:noggit:jar:0.5:compile
    [INFO]       +- org.ow2.asm:asm:jar:4.1:compile
    [INFO]       +- org.ow2.asm:asm-commons:jar:4.1:compile
    [INFO]       +- org.restlet.jee:org.restlet:jar:2.1.1:compile
    [INFO]       +- org.restlet.jee:org.restlet.ext.servlet:jar:2.1.1:compile
    [INFO]       \- org.slf4j:slf4j-api:jar:1.7.6:compile
    [INFO]                                                                         
    [INFO] ------------------------------------------------------------------------


Whoa! What just happebed? Well, as it turns out, the only solr-core related dependency exclude that is effecive here is this one from **prj1-mod-a**

        <artifactId>prj1-mod-a</artifactId>
    
        <dependencies>
            <dependency>
                <groupId>org.apache.solr</groupId>
                <artifactId>solr-core</artifactId>
                <!-- In addition to the excludes defined in parent pom dependencyManagement section, exclude these : -->
                <exclusions>
                    <exclusion>
                        <groupId>org.eclipse.jetty</groupId>
                        <artifactId>jetty-continuation</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>
        </dependencies>


When another project depends on prj1-mod-a, the only exclude rule it brings is the explicit exclusion rule defined in the pom.xml of prj1-mod-a.
The dependency exclude rules defined in the parent pom.xml of prj1-mod-a **do not apply!**

As a result, in the dependency tree of prj2-mod-d, **jetty-continuation** is not included, but every other transitive dependency from solr-core is included.


# prj3 and prj4
prj-3 and prj-4 mimic the scenario described above:

* prj-3 parent pom.xml is exactly like prj-1 parent pom.xml
* prj3-mod-a defines solr-core as a dependency, but relies completely on prj-3 parent pom.xml definition of it, without any exclude rules defined in prj3-mod-a

  
      <dependencies>
          <dependency>
              <groupId>org.apache.solr</groupId>
              <artifactId>solr-core</artifactId>
          </dependency>
      </dependencies>
      
* prj3-mod-b depends on solr-core and excplicitly defines all the same excludes as parent pom.xml, but in addition to that excludes jetty-continuation. This is a fix to the scenario between prj-1 and prj-2.

As a result the dependency trees look like this for prj-3 mod-a and mod-b:
    
    [INFO] ------------------------------------------------------------------------
    [INFO] Building prj3-mod-a 1.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ prj3-mod-a ---
    [INFO] com.koant.maven.demo:prj3-mod-a:jar:1.0-SNAPSHOT
    [INFO] \- org.apache.solr:solr-core:jar:4.10.3:compile
    [INFO]    +- org.eclipse.jetty:jetty-continuation:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-deploy:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-http:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-io:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-jmx:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-security:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-util:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-webapp:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-xml:jar:8.1.10.v20130312:compile
    [INFO]    \- org.eclipse.jetty.orbit:javax.servlet:jar:3.0.0.v201112011016:compile
    [INFO]                                                                         
    [INFO] ------------------------------------------------------------------------
    [INFO] Building prj3-mod-b 1.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ prj3-mod-b ---
    [INFO] com.koant.maven.demo:prj3-mod-b:jar:1.0-SNAPSHOT
    [INFO] \- org.apache.solr:solr-core:jar:4.10.3:compile
    [INFO]    +- org.eclipse.jetty:jetty-deploy:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-http:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-io:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-jmx:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-security:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-util:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-webapp:jar:8.1.10.v20130312:compile
    [INFO]    +- org.eclipse.jetty:jetty-xml:jar:8.1.10.v20130312:compile
    [INFO]    \- org.eclipse.jetty.orbit:javax.servlet:jar:3.0.0.v201112011016:compile
    [INFO]                                                                         
    [INFO] ------------------------------------------------------------------------

The main difference is the inclusion of jetty-continuation in mod-a, but not in mod-b.

So, what will happen in prj-4, where 
* prj4-mod-a depends on prj3-mod-a
* prj4-mod-b depends on prj3-mod-b

Questions:
* Will prj3 parent pom.xml dependency management excludes for solr-core apply all the way to prj4 dependency tree because prj3-mod-a does not define **additional** excludes when using solr-core?
* Will prj4-mod-b classpath include only the same solr-core transitive dependencies as prj3-mod-b classpath?


As it turns out, the answers tho those questions are **yes**, now the transitive dependency excludes work as they should:

    [INFO] ------------------------------------------------------------------------
    [INFO] Building prj4-mod-a 1.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ prj4-mod-a ---
    [INFO] com.koant.maven.demo:prj4-mod-a:jar:1.0-SNAPSHOT
    [INFO] \- com.koant.maven.demo:prj3-mod-a:jar:1.0-SNAPSHOT:compile
    [INFO]    \- org.apache.solr:solr-core:jar:4.10.3:compile
    [INFO]       +- org.eclipse.jetty:jetty-continuation:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-deploy:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-http:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-io:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-jmx:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-security:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-util:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-webapp:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-xml:jar:8.1.10.v20130312:compile
    [INFO]       \- org.eclipse.jetty.orbit:javax.servlet:jar:3.0.0.v201112011016:compile
    [INFO]                                                                         

As can be seen above, prj4-mod-a depends on prj3-mod-a which brings solr-core with only those transitive dependencies that are allowed by **prj3 parent pom.xml dependencyManagement section!**
This includes jetty-continuation dependency, which appears in the dependency tree.


    [INFO] ------------------------------------------------------------------------
    [INFO] Building prj4-mod-b 1.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ prj4-mod-b ---
    [INFO] com.koant.maven.demo:prj4-mod-b:jar:1.0-SNAPSHOT
    [INFO] \- com.koant.maven.demo:prj3-mod-b:jar:1.0-SNAPSHOT:compile
    [INFO]    \- org.apache.solr:solr-core:jar:4.10.3:compile
    [INFO]       +- org.eclipse.jetty:jetty-deploy:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-http:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-io:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-jmx:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-security:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-util:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-webapp:jar:8.1.10.v20130312:compile
    [INFO]       +- org.eclipse.jetty:jetty-xml:jar:8.1.10.v20130312:compile
    [INFO]       \- org.eclipse.jetty.orbit:javax.servlet:jar:3.0.0.v201112011016:compile
    [INFO]                                                                         

Finally, above output shows that the "overriding" dependency exclusions defined in prj3-mod-b apply to prj4-mod-b.



The main point? If you're developing multiple "projects" that have modules,
never **extend** dependency exclusion rules within one project if you wish the another project to have an expected dependency tree.

Always **override completely** the dependency exclusion rules if you need to add additional excludes.
 
