# Ev34j Mindstorm Tutorial

## System setup

Before you can run Java programs on the EV3 there is some setup required:

1. [Install required software](https://github.com/ev3dev-lang-java/ev34j-mindstorm-tutorial/wiki/Install-required-Mac-software) on your Mac.
2. [Create a bootable image](https://github.com/ev3dev-lang-java/ev34j-mindstorm-tutorial/wiki/Create-a-bootable-image-for-the-EV3) of the [ev3dev distro](http://www.ev3dev.org) on a micro SD card.
3. [Establish a network connection](https://github.com/ev3dev-lang-java/ev34j-mindstorm-tutorial/wiki/Establish-a-network-connection) on your EV3.
4. [Install required software](https://github.com/ev3dev-lang-java/ev34j-mindstorm-tutorial/wiki/Install-required-EV3-software) on your EV3.

## Clone the ev34j-mindstorm tutorial repo

Open a [Terminal](https://en.wikipedia.org/wiki/Terminal_(OS_X)) window and clone the
[Mindstorm Tutorial repo](https://github.com/ev3dev-lang-java/ev34j-mindstorm-tutorial) from [GitHub](https://github.com)
with:

```bash
$ cd ~
$ mkdir git
$ cd git
$ git clone https://github.com/ev3dev-lang-java/mindstorm-tutorial.git
$ cd ev34j-mindstorm-tutorial
$ ls
```

Verify that you everything it setup properly with:

```bash
$ # One-time copy of hte scripts
$ make copy-scripts
$ # Build the jar
$ make clean build
$ # Copy the jar to your EV3
$ make scp
$ # Run the app on your EV3
$ make run
```

## Build and run programs

### pom.xml
The [pom.xml](https://github.com/ev34j/ev34j-mindstorm-tutorial/blob/master/pom.xml)
contains the configuration information required to build the program. The two relevant properties are:

| Property                   | Value                                                            |
|:---------------------------|:-----------------------------------------------------------------|
| &lt;ev34j.version&gt;      | update when the underlying ev34j library changes                 |
| &lt;mainclass.name&gt;     | set this to the name of the java class that you want to execute  |

### Makefile
The [Makefile](https://github.com/ev34j/ev34j-mindstorm-tutorial/blob/master/Makefile) is provided to
make building and running programs easy. The configuration variables at the top of the *Makefile* include:

| Variable                | Value                                                       |
|:------------------------|:------------------------------------------------------------|
| EV3_HOSTNAME            | Update if you change /etc/hostname on the EV3 |
| EV3_PASSWORD            | Update if you change the default password on the EV3        |
| JAR_NAME                | No reason to change                                         |
| LOG_PROP_NAME           | No reason to change                                         |
| SSH_PREFIX              | Set to blank if you use [SSH keys](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2) instead of [sshpass](https://gist.github.com/arunoda/7790979) |

The *Makefile* has the following targets:

| Target              | Action                                                  |
|:--------------------|:--------------------------------------------------------|
| clean               | erase everything in the target directory                |
| build               | build the uber-jar file                                 |
| scp                 | copy the jar file to the EV3                            |
| run                 | execute the jar file on the EV3                         |
| debug               | execute the jar file in the debgugger mode on the EV3   |
| logging             | execute the jar file with logging enabled on the EV3    |
| kill                | kill all java processes running on the EV3              |
| copy-scripts        | copy convenient command-line scripts to the EV3         |

To build and run a program from the OSX command line:

```bash
$ cd ev34j-mindstorm
$ # Build the jar
$ make build
$ # Copy the jar to the EV3
$ make scp
$ # Run the app on the EV3
$ make run
```

You can also execute a program from the EV3 command-line:

```bash
$ make copy-scripts
$ ssh robot@ev3dev
robot@ev3dev:~$ ./run.sh
robot@ev3dev:~$ # Or you can use java directly
robot@ev3dev:~$ java -jar ev3robot-jar-with-dependencies.jar
```

**Remember** to rebuild the jar and copy it to the EV3 after making changes in the
source code. Also, if you rename your main java class or want to execute a different class, make sure you
update the **&lt;mainclass.name&gt;** value in the pom.xml.

## Building and running programs from the IntelliJ Toolbar

You can also add buttons to the IntelliJ Toolbar that make building and running your
programs very easy.

Adding toolbar buttons is a two step process:

1. [Add External Tools](https://github.com/ev34j/ev34j-mindstorm-tutorial/wiki/Add-Intellij-External-Tools) to build, copy, run and debug programs

2. [Add buttons to the Toolbar](https://github.com/ev34j/ev34j-mindstorm-tutorial/wiki/Add-Toolbar-Buttons) and map them to External Tools

## Debugging a program with IntelliJ

#### Start a program in debug mode on the EV3

You can do this multiple ways:

* Click on the **Debug program on EV3** toolbar button

* On the OSX command-line:

```bash
$ make debug
# Debug jar on EV3
sshpass -p maker ssh robot@ev3dev1 java -agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=5005 -jar ev3robot-jar-with-dependencies.jar
Listening for transport dt_socket at address: 5005
```

* On the EV3 command-line:

```bash
robot@ev3dev:~$ ./debug.sh
Listening for transport dt_socket at address: 5005
robot@ev3dev:~$ # Or
robot@ev3dev:~$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=5005 -jar ev3robot-jar-with-dependencies.jar
Listening for transport dt_socket at address: 5005
```

#### Run the IntelliJ debugger

1. [Create a remote configuration](https://github.com/ev34j/ev34j-mindstorm-tutorial/wiki/Create-a-Remote-Configuration).

2. Click on **Run** --> **Debug...** and then choose the newly created Remote configuration.

3. See the IntelliJ [documentation](https://www.jetbrains.com/help/idea/2016.1/debugging.html) for how to
set breakpoints and step through a program.

#### Stopping a program on the EV3

If you start a program with **make run** on OSX and then kill the process with **Ctrl-C**, the
program might continue to run on the EV3, i.e., killing a make process on OSX does
not kill the program on EV3. Before running the program again, you have to kill the still-running
process.

You can kill an EV3 program from the OSX command-line with:

```bash
$ make kill
# Kill java process on EV3
sshpass -p maker ssh robot@ev3dev1 ./kill.sh
```

You can kill an EV3 program from the EV3 command-line with:

```bash
robot@ev3dev:~$ ./kill.sh
```

The kill.sh script is copied to the EV3 with:

```bash
$ make copy-scripts
```

## The ev34j-mindstorm classes

The ev34j-mindstorm classes are outlined
[here](https://github.com/ev34j/ev34j-mindstorm-tutorial/wiki/Ev34j-Mindstorm-Object-Summary)
and the javadocs are [here](http://docs.ev34j.com).










