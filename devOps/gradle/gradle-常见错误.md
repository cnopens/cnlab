# gradle-常见错误.md


## Gradle: Could not get unknown property 'classesDir' for main classes

buildscript {
    repositories {
        mavenCentral()
        maven {
            url "https://plugins.gradle.org/m2/"
        }
    }
    dependencies {
        classpath "org.apache.meecrowave:meecrowave-gradle-plugin:1.2.6"
        classpath "gradle.plugin.aspectj:gradle-aspectj:0.1.6"
    }
}

plugins {
    id 'java'
}

project.ext {
    aspectjVersion = '1.9.2'
}

apply plugin: 'aspectj.gradle'
apply plugin: "org.apache.microwave.microwave"

java {
    sourceCompatibility = JavaVersion.VERSION_11
    targetCompatibility = JavaVersion.VERSION_11
}

dependencies {
    compile("org.apache.meecrowave:meecrowave-core:1.2.6")
    compile("org.apache.meecrowave:meecrowave-specs-api:1.2.6")
}

meecrowave {
    httpPort = 9090
    // most of the meecrowave core configuration
}


## Could not find org.junit.jupiter:junit-jupiter:.




## config wrapper 

Modifty the gradle/gradle-wrapper.properties

Windows:

distributionUrl=file\:/d:/gradle-2.2.1-all.zip
linux:

distributionUrl=file\:/tmp/gradle-2.2.1-all.zip


192.168.31.33