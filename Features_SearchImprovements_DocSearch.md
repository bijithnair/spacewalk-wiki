## Documentation Search Implementation Notes

Docs in regard to spacewalk/satellite can exist in 2 ways.

 1. Link to www.redhat.com/manuals/satellite  (Spacewalk)
 1. Locally residing jsp files  (Satellite)
 
Our goal is to create a lucene index representing this info and allow it to be searchable through the spacewalk search-server.
### Approach

We will use "nutch" to generate the lucene index.  This index will be packaged into a RPM and will be part of the process for building the docs. The search-server will use this data as another regular lucene index.  

### General Notes

* First case: Docs reside at http://www.redhat.com/docs/manuals/satellite/

 * Will need to perform a web crawl with nutch
 * Download nightly build at:  http://hudson.zones.apache.org/hudson/job/Nutch-trunk/
 * untar build to something like /tmp/nutch-nightly
 * create a urls directory:  mkdir /tmp/nutch-nightly/urls
  * *NOTE* this needs to be a directory, the tutorial from nutch still shows this as a text file, I had problems with this failing until I created the directory 
 * create a urls.txt file consisting of the below line:   vim /tmp/nutch-nightly/urls/urls.txt
  * http://www.redhat.com/docs/manuals/satellite/
 * update "http.agent.name" *the crawl will fail if this is not done*
  * edit conf/nutch-site.xml add an entry for "http.agent.name"

    <property>
      <name>http.agent.name</name>
      <value>Spacewalk</value>
      <description>HTTP 'User-Agent' request header. MUST NOT be empty - 
      please set this to a single word uniquely related to your organization.
    
      NOTE: You should also check other related properties:
    
            http.robots.agents
            http.agent.description
            http.agent.url
            http.agent.email
            http.agent.version
    
      and set their values appropriately.
    
      </description>
    </property>
 * additional entries to conf/nutch-site.xml to make the crawl quicker (at the expense of being rude to our target host)

    
    <property>
      <name>fetcher.server.delay</name>
      <value>0</value>
      <description>The number of seconds the fetcher will delay between 
       successive requests to the same server.</description>
    </property>
    
    <property>
      <name>fetcher.threads.per.host</name>
      <value>50</value>
      <description>This number is the maximum number of threads that
        should be allowed to access a host at one time.</description>
    </property>
 * Edit conf/crawl-urlfilter.txt.  We are setting it to only accept links for redhat.com and we are telling it to skip pdf documents (we can add pdf support back in later, taking it out now to simplify things and to go quicker)

    # skip image and other suffixes we can't yet parse
    -\.(gif|GIF|jpg|JPG|png|PNG|ico|ICO|css|sit|eps|wmf|zip|ppt|mpg|xls|gz|rpm|tgz|mov|MOV|exe|jpeg|JPEG|bmp|BMP|pdf)$
    
    # accept hosts in MY.DOMAIN.NAME
    +^http://([a-z0-9]*\.)*redhat.com/
 * Run the crawl:
  * /tmp/nutch-nightly/bin/nutch crawl urls -dir crawl.data -depth 5 -threads 50 &> crawl.log
   * this takes on the order of ~15 minutes to complete with the above changes, or about 90 minutes without the changes.

* Working with the output from nutch
 * End goal is to take the output and feed it into the search-server and have that perform the search
 * For a first cut to see if the data is valid, we can use the search interface nutch provides.
  * From the above run copy crawl.data to /tmp/redhat.com-test-data
  * Copy the .war file from the nutch build tomcat's webapps directory
   * cp nutch-nightly.war $TOMCAT_HOME/webapps/nutch.war
  * Restart tomcat, go into $TOMCAT_HOME/webapps/nutch/WEB-INF/classes
  * Edit nutch-site.xml

    <?xml version="1.0"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    
    <!-- Put site-specific property overrides in this file. -->
    
    <configuration>
    <property>
    <name>searcher.dir</name>
    <value>/tmp/redhat.com-test-data/</value>
    </property>
    
    </configuration>
    
  * It looks like the nightly builds have a problem with "search.jsp".  Below is the error message I was receiving

    ERROR [jsp] - Servlet.service() for servlet jsp threw exception
    org.apache.jasper.JasperException: /search.jsp(172,22) Attribute value  language + "/include/header.html" is quoted with " which must be escaped when used within the value
  * This is the workaround,  edit $TOMCAT_HOME/webapps/nutch/search.jsp and escape the quotes. Line 172 for my nightly build.

     <jsp:include page="<%= language + \"/include/header.html\"%>"/>
   * This issue appears to have been around since September 2008, I've seen 2 posts relating to it, but as of 11/21 it still hasn't been fixed in the nightly build. 
   * Restart tomcat once more
   * Now go to the nutch webapp, example: http://127.0.0.1:8080/nutch 
   * You can search through the data to see what will be available to our search app.

 * Integrating output from nutch into our search-server.  Copy the generated indexes into the search-server.
  * "mkdir /usr/share/rhn/search/indexes/docs"
  * "cp /tmp/nutch-nightly/crawl.data/index/* /usr/share/rhn/search/indexes/docs"
  * to test, go to : http://localhost/rhn/help/Search.do  (assuming localhost is running a devel version of spacewalk)
 * Adding "Summary Content" for matches
  * "cp /tmp/nutch-nightly/crawl.data/segments /usr/share/rhn/search/indexes/docs"
  * "mkdir /usr/share/rhn/search/nutch"
  *  nutch must be told where to find it's plugin directory
   * Edit /tmp/nutch-nightly/conf/nutch-site.xml, add in the below change

    <property>
      <name>plugin.folders</name>
       <value>/usr/share/rhn/search/nutch/plugins</value>
       <description>Directories where nutch plugins are located.  Each
       element may be a relative or absolute path.  If absolute, it is used
       as is.  If relative, it is searched for on the classpath.</description>
     </property>
  * "cp /tmp/nutch-nightly/conf /usr/share/rhn/search/nutch"
  * "cp /tmp/nutch-nightly/plugins /usr/share/rhn/search/nutch"
  * At this point if you do a search, the surrounding text from the match should be displayed in the webui.
### Issues/Tasks



|   *Issue*  |  *Status*  |  *Notes*  |
| --- | --- | --- |
|   -----   |    |    |
|   add content from a surrounding match to webui  |  done  |  requires extra "segments" directory to be included with indexes  |
|   indexes from www.redhat.com/manuals/satellite need the URLs to be restricted more, i.e. the crawl is picking up entries we don't care about.   |  done  |    |
|   Need to crawl/index in multiple languages.  First non-english language selected will be Japanese.   |  done  |  supports all languages present in translated docs  |
|   Webui needs to recognize language and pass it through to search server request.   |  done  |  the locale is passed in the xmlrpc call as an argument   |
|   Need to setup a nutch configuration that will parse JSP docs  |  done  |  each language has it's own set of configuration files   |
|   -----   |    |    |
# Outstanding Items as of 2/5/09

 1. API docs are not currently being indexed/searched.  

 Estimate: ~3 days

  