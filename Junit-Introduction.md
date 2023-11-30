### Written by Reddy

# Junit testing

Junit is an open source framework used for unit testing in Java. It is the most common tool used for Java Unit testing. Unit testing in short, is a form of whitebox testing used for testing the internal code of the program. It focuses on the paths of the code and is used to test each method or class.

## Targeted Audience

The tutorial assumes you are a beginner to Junit testing and is designed accordingly.

## Prerequisite Knowledge

The tutorial assumes you have sufficient knowledge in Java and the testing process for the java programming language. In this tutorial, we will be using the Gradle build system.

We will begin by finding our build.gradle file from our directory and adding the following to allow gradle to use Junit to run our tests. You need to simply add this to your gradle file.

`tasks.named('test') {`
    `// Use JUnit Platform for unit tests.`
    `useJUnitPlatform()`
`}`



 The second step would be to add dependencies on your build.gradle file again simply copy paste the following to your build.gradle file. Note: you don’t have to use implementation 'com.google.guava:guava:32.1.1-jre' For other projects as this is relevant to only the othello program we use in this tutorial.


Note: update your gradle or check to ensure your gradle version is 8 or later 


 `   // Use JUnit Jupiter for testing.`
    `testImplementation 'org.junit.jupiter:junit-jupiter:5.9.3'`

    `testRuntimeOnly 'org.junit.platform:junit-platform-launcher'`



#  ## Let’s evaluate Junit

Junit is a free open source framework so it’s free and can be used by anyone, the public website has all the information we need for understanding and learning Junit, It integrates seamlessly into build systems like Gradle and Maven(we used Gradle for this tutorial). It’s easy to use annotations* such as @test, @beforeeach @aftereach @ afterall @beforeall makes it easy to write tests. Junit5 allows for meta-annotations which makes it easier for developers to compose their composed annotations and use it throughout their tests saving them time and makes it readable.










              *To learn about all the annotations go to (https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations)