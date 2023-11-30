*Written by Kyle Findlay*

*Edits by Chris Fitzgerald*

The first step to using the code given is to install Gradle if you do not already have it.

*Note: for gradle to work you need Java JDK version 8 or higher, to check if you have java installed open a terminal and use the command `java -version`*

## Installation

*Note: if you have SDKMAN! you can use the following command*
```
sdk install gradle 8.4
```

*Note: if you have Homebrew you can use the command*
```
brew install gradle
```

If you do not have either of those you can install manually instead doing **Step 1** and following 2 steps from the below tutorials based on your operating system and finally **Step 4**.

**Step 1:**
Download the version of Gradle you want from [Here](https://gradle.org/releases), I suggest downloading the Binary-Only version.
The version of Gradle this project uses is 8.4 so make sure to get that version or later.

## Windows

**Step 2:**

Open up File explorer and create a new directory with the name `C:\Gradle`

Open a second File Explorer open the downloaded ZIP file and drag the `gradle-8.4` folder into the `C:\Gradle` folder

**Step 3:**

Hit the Windows key and type `Environment Variables` and hit enter

A window with the title `System Properties` should appear. Move to the `Advanced` tab and click the button labeled `Environment Variables`

Then double click the variable labeled `Path` and a new window labeled `Edit environment variable` should have opened.

Click the button that says `New` and type `C:\Gradle\gradle-8.4\bin` and click `OK` to save.

---


## Mac / Linux

**Step 2:**

Use the following commands to unzip the distribution and use the ls command to make sure it worked

```
$ mkdir /opt/gradle
$ unzip -d /opt/gradle gradle-8.4-bin.zip
$ ls /opt/gradle/gradle-8.4
LICENSE  NOTICE  bin  getting-started.html  init.d  lib  media
```

**Step 3:**

use the following command to change the PATH environment variable

```
$ export PATH=$PATH:/opt/gradle/gradle-8.4/bin
```

---

**Step 4:**

Now is the time to make sure the installation worked open up a terminal and use the command

```
gradle -v
```

it should print the current version of Gradle that you have installed

---

## Usage

### For Othello

For Othello you would cd to the directory containing the build.gradle file and run the game with the command

*Note: we use the `--console plain` flag to remove the prints gradle produces to make the game printouts more readable. Try running it without this flag to see the difference*

```
gradle run --console plain
```

---

### Setting up Gradle with your own project

To start open a terminal and simply cd to an empty directory that you would like your project to be in. Then run the command:

```
gradle init
```

Gradle will then ask you a couple of questions to build the project. The first of which is:

```
Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4]
```

The one you want to pick here is up to your project. For Othello we used `application` and chose `Java` or `Groovy` when prompted for languages. You will then be asked "Generate multiple subprojects for application?", which you should pick "no".

*Note: Gradle can be used for a whole host of other programming languages, not just Java, like C++, Groovy and Swift*

When generating an `application` Gradle will also ask you what testing framework you could like to use. For Java by default Gradle supports:

- JUnit 4
- TestNG
- Spock
- JUnit Jupiter

*Note: Gradle also supports other test frameworks you just have to edit the build.gradle to support them*

---

### Building, Running and Testing

If you have Gradle generate a build.gradle file these are the commands to build, run and test your project respectively.

*Note: for any gradle command you can use the --help flag, like this `gradle  [command] --help`, to see all the possible flags for the command with descriptions*

To build the project
```
gradle build
```

To build (if needed) and run
```
gradle run
```

To test the project
```
gradle test
```

When using gradle test Gradle generates an HTML report that can be found in `app/build/reports/tests/test` by default
