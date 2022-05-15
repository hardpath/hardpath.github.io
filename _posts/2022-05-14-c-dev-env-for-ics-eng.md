---
title: "C/C++ Cross-platform Development Environment for ICS Engineers"
date: 2022-05-14 10:00:00 +0000
categories: ot cyber
folder: "2022-05-14-c-dev-env-for-ics-eng"
tn_ref: 
tn_rev: ""
abstract: >
  Set up a cross-platfrom C/C++ development environment for ICS engineers.
---
<table style="width:35%">
  <tr>
    <td style="vertical-align:bottom">
<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Support Me on Ko-fi', '#29abe0', 'Z8Z37OFYG');kofiwidget2.draw();</script> 
    </td>
  </tr>
</table>

## 1 INTRODUCTION

Working in Industrial Control Systems (ICS) whilst doing cybersecurity research
can sometimes be challenging. Allow me to elaborate.

If you work on ICS, very likely you have a computer running Microsoft Windows.
However, if at the same time you work in Cybersecurity, you probably have at
least one virtual machine running Linux. Well, this is my case, anyway[^1].

When it comes to software development, usually to automate some task or to
analyse a big amount of data, I need it to run on both Windows and Linux.
Furthermore, development can happen in either environment depending on the task
at hand.

Just one more detail - I use C/C++ for most of my development.

This article describes how to have development environments as similar as
possible regardless of the operating system.

It is assumed that you have a basic knowledge of Git and C/C++ code development.

## 2 REQUIRED SOFTWARE

I'm running Windows 10 and a Debian 11 Virtual Machine at the time of this
writing.

The following software will be required:

- **Git** (Windows and Linux)
- **MSYS2** (Windows)
- **Visual Studio Code** (Windows and Linux)

### 2.1 Git Installation (Windows and Linux)

I will not detail how to install Git. [Here](https://git-scm.com/downloads),
you will find all you need. The Windows installer is straightforward, and in
Linux, `apt` will do everything for you.

I share the most relevant setting when running the installation wizard on
Windows in the screenshots below.

![git1](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0201.png)
Ensure this option is selected.

![git2](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0202.png)
For me, VI is still my prefered editor.

![git3](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0203.png)

![git4](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0204.png)

![git5](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0205.png)
This one is important also.

### 2.2 MSYS2 Installation (Windows only)

[This](https://www.msys2.org) will provide you with the tools and libraries to
compile code on Windows, and you will find the steps to install **MSYS2**.

Before installing, check steps 3 to 5 [here](https://code.visualstudio.com/docs/cpp/config-mingw).

### 2.3 Visual Studio Code (Windows and Linux)

[Here](https://code.visualstudio.com/docs/setup/setup-overview), you will find
all you need to download and install Visual Studio Code (VS Code).
For Windows, you can discard all references to WSL (Windows Subsystem for
Linux).

These are the seetings I use when installing VS Code on Windows.

![vscode](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0206.png)

## 3 ADDITIONAL CONFIGURATIONS

### 3.1 Bash (Windows)

For bash, I use the one made available by Git - **Git Bash**.

Right-click anywhere on your desktop and select `Git Bash here`.

In the bash window, if you enter the commands `gcc` and `make` you should get
`gcc.exe: fatal error: no input files` and `bash: make: command not found`,
respectively. This tells you that `gcc` is there but `make` was not found.
Relax, it is there.

In the `C:\msys64\mingw64\bin` folder[^2] (I'm assuming you installed MSYS2 in
`C:\msys64`), you should be able to find the file `mingw32-make.exe` - that's
`make`.

To use `make` from the command line, you have two options.

The first option is to create a copy of that executable and rename it
`make.exe`.

The other option, which I would recommend, is to create an alias.

`alias make='mingw32-make'`

Obviously, you don't want to create this alias manually every time you open a
bash window. So, you should add that alias to the `.bash_aliases` file, which
is called from the `.bashrc`.

Yes, I know, there are no such files there. I strongly recommend you to create
those. This will get you started.

On the bash window, enter the command `cd` - this will take you to your home
directory.

There, create the `.bashrc` file with the following contents:

    if [ -f ~/.bash_aliases ]; then
        . ~/.bash_aliases
    fi

Now you need to create the `.bash_aliases` file. This file will hold all your
aliases. For this alias in particular, I would recommend this:

    if [ `uname` != 'Linux' ]
    then
        alias make='mingw32-make'
        alias more="less -de"
    fi

This way, you can use the same `.bash_aliases` on both Windows and Linux, and
the alias will be created only if required. The `more` alias has nothing to do
with `make` - consider it a bonus.

### 3.2 Bash on Linux

After opening a terminal, `gcc` and `make` commands should tell you that both
exist and are ready to be used.

So, no aliases are required. You can still use the `.bash_alaises` file above -
that if statement will discard those aliases on Linux.

### 3.3. Visual Studio Code (Windows)

You will see in the next section that we will be using the terminal inside
VS Code very often.

By default on Windows, the terminal is a DOS command prompt. To make it
consistent with the terminal we have on Linux, we need this default to be the
Git Bash terminal.

For that, in VS Code, press `CTRL + SHIFT + P` to open the Command Palette,
type `Terminal: Select Default` and then select `Terminal: Select Default Profile`.

Several options will be presented. Select `Git Bash`.

Now, every time you open a new terminal, you will get the Git Bash.

### 3.4. Visual Studio Code (Linux)

On Linux, nothing needs to be done for now regarding VS Code.
So, let's do some coding and see what else will be needed.

## 4. Hello World Project

Before doing anything else, allow me to show where my code is on my machines.

I have the folder structure shown in the figure below on both Windows and Linux
environments.

![folders](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0401.png)

The location of the `source` folder is not relevant. However, you need to know
where in your hard drive it is because we will define some environment
variables based on that.

The `source` folder will hold the following:

- `github`: holds the project repository clones from GitHub.
- `include`: my header files, typically associated with my libraries
- `lib`: my compiled libraries
- `repos`: projects repositories only stored locally

I will assume you will have the same folder structure in place from now on.
However, if that is not the case, please adapt the instructions below
accordingly.

### 4.1 Create Git Repository

I use **GitHub** to host my source code, which must be compiled on both Linux
and Windows. I will skip all the details regarding working with GitHub and how
to create a repository.

Instead, I leave here only a screenshot.

![git](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0402.png)

### 4.2 Coding in Linux

On a terminal, change to the `github` folder inside the `source` folder.

There, clone the `helloworld` repository.

You should now have a `helloworld` folder inside the 'github' folder which
contains a `README.md` file.

Now, enter the command `code .` (there is a dot at the end).

In VS code, create a `main.cpp` file with the following.

    #include <iostream>

    using namespace std;

    int main(int argc, char **argv)
    {
        cout << "Well... that was easy!" << endl;
        return 0;
    }

You might consider the following an overkill, but I always use a `Makefile`[^3].

I have the below as a "template" - it will get you covered in most cases.

    TARGET = hworld
    SRC = main

    OBJS = $(addsuffix .o, $(SRC))
    HEADERS = $(addsuffix .h, $(SRC))
    LIBS = 

    CXXFLAGS = -Wall -Wno-unknown-pragmas -I$(INCPATH)
    LDFLAGS = -L$(LIBPATH)

    all: $(TARGET)

    $(TARGET): ${OBJS}
      $(CXX) $(OBJS) -o $@ $(LDFLAGS) $(addprefix -l, $(LIBS))

    debug: CXXFLAGS += -g
    debug: $(TARGET)

    %.o: %.cpp %.h
      $(CXX) $(CXXFLAGS) -c $< -o $@

    clean:
      @rm -f $(TARGET)
      @rm -f *.o
      @echo "Clean done."

    showvars:
      @echo "   TARGET: $(TARGET)"
      @echo "      SRC: $(SRC)"
      @echo "     OBJS: $(OBJS)"
      @echo "  HEADERS: $(HEADERS)"
      @echo "     LIBS: $(LIBS)"
      @echo " CXXFLAGS: $(CXXFLAGS)"
      @echo "  LDFLAGS: $(LDFLAGS)"

So, go ahead and create that `Makefile` in VS Code.

VS Code might notify that an extension for Makefile is available. It is up to
you, but I found no use for it at the time of writing.

All ready to compile the code. In VS Code, open a terminal and type the command
`make` there.

If everything goes right, it will not work. You should have the following error:

`g++: error: missing argument to ‘-L’`

If you noticed, the   Makefile   uses several variables; some of them are not
defined in the `Makefile` but are environment variables.

That is the case of `INCPATH` and `LIBPATH`. If you enter the command
`make showvars` in the terminal, you will see that `CXXFLAGS` has nothing after
`-I`, and `LDFLAGS` only has `-L`.

We do not need these flags for this simple project, but this will allow me to
show another tweak.

Let's use the `.bash_aliases` file to define those environment variables.

In the `.bash_aliases` file above, add the following at the end.

    # Exports
    export LIBPATH="/<wherever_the_source_folder_is>/source/lib"
    export INCPATH="/<wherever_the_source_folder_is>/source/include"

After, on VS Code, close the terminal and open a new one.
Now, the `CXXFLAGS` and `LDFLAGS` should contain the folders defined above.
Please check running the command `make showvars`.

And now, the code should compile without any errors. If that is the case, you
should have a file `hworld`. So, in the terminal, go ahead and run it - enter
the command `./hworld`.

Easy, isn't it?

Before moving to Windows, let's ensure we can debug this code. To be honest,
that is the main reason I use VS Code.

To be able to debug the code, it has to be compiled with the option `-g`. As I
told you, the Makefile have you covered.

Clean the build by running the command `make clean` and then build it with
`make debug` command.

In the `main.cpp` file, insert a breakpoint in line 7, as shown below.

![break](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0403.png)


In the **Run and Debug** pane, you can see that you will need to create a
`launch.json` file.

![launch](/assets/images/posts/2022-05-14-c-dev-env-for-ics-eng/img0404.png)

Let's create a `launch.json`. First, ensure that you have the file `main.cpp`
open, and then press that link highlighted in the figure above.

After, in the command pallet, select `C++ (GDB/LLDB)`.

A file is created. Replace its contents with the below.

    {
      "version": "0.2.0",
      "configurations": [
        {
          "name": "Debug",
          "type": "cppdbg",
          "request": "launch",
          "program": "<full_path_to_the_binary>/hworld",
          "args": [],
          "stopAtEntry": false,
          "cwd": "${workspaceFolder}",
          "environment": [],
          "externalConsole": false,
          "MIMode": "gdb",
          "setupCommands": [
            {
              "description": "Enable pretty-printing for gdb",
              "text": "-enable-pretty-printing",
              "ignoreFailures": true
            }
          ],
          "miDebuggerPath": "/usr/bin/gdb"
        }
      ]
    }

Save the file.

The file will be saved in the `.vscode.` folder created automatically.

If you go back to the **Run and Debug** pane, you can start debugging by
pressing the "play" button next to **Debug**.

Although there is the possibility to build the project automatically when
starting debugging, I prefer to do that manually by running the `make` command
in the terminal.

In the Source Control pane, you can see that Git is tracking `launch.json`
file, the object file and the binary (executable).

I would advise you to ignore all those files.
For that, add a `.gitignore` file with the following contents:

    .vscode

    *.o
    *.exe

    hworld

Note that you will need to restart VS Code for the Source Control pane to be
updated.

And that's all. Ensure you are also tracking the `.gitignore` file, and then
commit and push the changes to GitHub. 

### 4.3 Coding in Windows

On Windows, Clone the GitHub repository into the `source/github` folder.

One way to do that is to use the Git Bash. Open Windows Explorer on the
`source/github` folder and right-click and select **Git Bash Here**.

If you run the command `make showvars` you will see that you have the same
problem with `CXXFLAGS` and `LDFLAGS` as before.

So, we need to add those EXPORTs to the `.bash_aliases` file.
To be able to use the `.bash_aliases` in both Windows and Linux, I would
suggest the following changes:

At the beginning of the file, add the following to detect the Operating System.

    # os: Operating System
    os=`uname -s`
    os=${os:0:5}

Then, in the end, add the following.

    if [ $os = "MINGW" ]
    then
        export LIBPATH="/<windows_path_to_source>/source/lib"
        export INCPATH="/<windows_path_to_source>/source/include"
    else
        export LIBPATH="/<linux_path_to_source>/source/lib"
        export INCPATH="/<linux_path_to_source>/include"
    fi

Use the command `pwd` to find exactly where the source folder is.

And that's it.

In the Git Bash you can run the command `code .` to open the folder in VS Code,
and the steps to debug are the same as Linux.

I hope you have found this useful.


---
[^1]: Enabling WSL on Windows has brought me problems running Virtual Machines. I can work without WSL, but I can't work without VMs.
[^2]: With all due respect to Linux fans, it is easier for me to pronounce *folder* than *directory*; please allow me this nuance.
[^3]: Some might prefer to use CMake to manage the build process. That is also a viable approach. However, if you want to go through this route, the remaining post will not be very relevant.