#jdk-version-switch.md


if you are a Java developer, it is normal to have multiple Java versions installed on your machine to support different build environments. When a Java program is compiled, the build environment sets the oldest JRE version the program can support. Now, if you run this program on a Linux machine where an unsupported Java version is installed, you will encounter an exception.

For example, if your program is compiled on Java 11, it can't be run on a machine where Java 8 is installed. But the good thing is you can install multiple Java versions on your machine and quickly change the default JRE version.

In this tutorial, I'll explain how to change the default Java version on a Linux machine. First of all, run the following command to check the current Java version:

$ java -version
openjdk version "1.8.0_191"
OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-2ubuntu0.18.10.1-b12)
OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)

As you can see above, the default Java version is currently set to OpenJDK JRE 1.8. Now, let's run the following command to see all available Java versions:

$ sudo update-alternatives --config java

Running the above command displays a list of installed Java JDKs and JREs allowing you to select the one as you want to set as default.

There are 2 choices for the alternative java (providing /usr/bin/java).
  Selection    Path                                            Priority   Status
------------------------------------------------------------
  0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode
* 2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode

Press <enter> to keep the current choice[*], or type selection number:

When prompted, select the Java version you would like to use. If the list does not include your desired Java version, you can always install it.

Now you can verify the default Java version as fellows:

$ java -version
openjdk version "11.0.2" 2019-01-15
OpenJDK Runtime Environment (build 11.0.2+9-Ubuntu-3ubuntu118.10.3)
OpenJDK 64-Bit Server VM (build 11.0.2+9-Ubuntu-3ubuntu118.10.3, mixed mode, sharing)

That's it. The default Java version is changed to OpenJDK 11.
Bonus: Use Script to Switch Java Version

If you frequently switch between different Java versions, it is a good idea to write a short script to automate the process. Here is the script I used for switching to OpenJDK 8 on my machine.

java8.sh

sudo update-java-alternatives -s java-1.8.0-openjdk-amd64
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/
export PATH=$PATH:$JAVA_HOME

Similarly, you can create scripts for other Java versions installed on your machine. The next step is to add these scripts as aliases to .bashrc file.

...
# Java Alias
alias java8='source /opt/java/switch/java8.sh'
alias java11='source /opt/java/switch/java11.sh'

Next, run the following command to load the changes of .bashrc file:

$ source ~/.bashrc

Now if you want to switch to Java 8, just type the following command in your terminal:

$ java8

Read Next: How to install Java on Ubuntu 18.04

✌️ Like this article? Follow me on Twitter and LinkedIn. You can also Subscribe to RSS Feed.

Last Updated: February 18, 2020
You might also like...

    How to install Nginx on Ubuntu 18.04
    Remove the last character of a string in Java
    How to reverse the elements of a stream in Java
    How to extract digits from a string in Java
    How to install MySQL on Ubuntu 18.04

Reference:

https://attacomsian.com/blog/change-default-java-version-ubuntu

