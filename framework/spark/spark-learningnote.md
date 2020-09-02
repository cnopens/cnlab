#spark-learningnote.md

## tips and problems
Spark integration with spring boot web starter?

 am trying to integrate Spark application with spring boot but since spark core also has jetty server and servlet packages, they are conflicting with spring boot web starter servlet packages. 

 

I found the solution a long time ago, but couldn't post this. I had to exclude a bunch of packages from spark-core that contain servlet packages

 compile(group: 'org.apache.spark', name: 'spark-core_2.11', version: '1.6.0') {
    exclude group: 'org.slf4j', module: 'slf4j-simple'
    exclude group: 'org.eclipse.jetty', module: 'jetty-server'
    exclude group: 'com.sun.jersey', module: 'jersey-server'
    exclude group: 'com.sun.jersey', module: 'jetty-core'
    exclude group: 'org.eclipse.jetty.orbit', module: 'javax.servlet'
    //exclude group: 'org.eclipse.jetty', module: 'jetty-server'
}

