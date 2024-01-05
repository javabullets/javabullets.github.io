---
id: 848
title: 'procrun &#8211; Java Programs as Windows Services'
date: '2016-10-31T13:04:54+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=848'
permalink: /procrun-java-programs-as-windows-services/
timeline_notification:
    - '1477919195'
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":0,"pinterest":0,"linkedin":0,"total":0,"last_fetch":1628722433,"url":"https://www.javabullets.com/procrun-java-programs-as-windows-services/"}'
categories:
    - Java
tags:
    - java
    - procrun
---

I recently needed to run a Java program as a Windows service, and opted for [Commons-daemon procrun](https://commons.apache.org/proper/commons-daemon/procrun.html). This wrapper is used by both Tomcat and JBoss Wildfly to wrap their servers – but took a bit of figuring out how to get my application running.

This post sets out an example of using procrun to wrap a Java process.

# Download

I downloaded procrun from [download](http://www.apache.org/dist/commons/daemon/binaries/windows/). The download contains 3 different version of the procrun.exe –

- 32 bit – this is the default architecture
- amd64 – 64-bit AMD architecture
- ia64 – 64-bit Intel Itanium architecture

You need to use the right version for your JVM and chipset

# Code

The code is based on the [EchoServer ](http://docs.oracle.com/javase/tutorial/displayCode.html?code=http://docs.oracle.com/javase/tutorial/networking/sockets/examples/EchoServer.java)and [EchoClient ](http://docs.oracle.com/javase/tutorial/displayCode.html?code=http://docs.oracle.com/javase/tutorial/networking/sockets/examples/EchoClient.java)examples from oracle.

## EchoServer

```
import java.net.*;
import java.io.*;
 
public class EchoServer {
    public static void main(String[] args) throws IOException {
         
        if (args.length != 1) {
            System.err.println("Usage: java EchoServer <port number>");
            System.exit(1);
        }
         
        int portNumber = Integer.parseInt(args[0]);
         
        try (
            ServerSocket serverSocket =
                new ServerSocket(Integer.parseInt(args[0]));
            Socket clientSocket = serverSocket.accept();     
            PrintWriter out =
                new PrintWriter(clientSocket.getOutputStream(), true);                   
            BufferedReader in = new BufferedReader(
                new InputStreamReader(clientSocket.getInputStream()));
        ) {
            String inputLine;
            while ((inputLine = in.readLine()) != null) {
                out.println(inputLine);
            }
        } catch (IOException e) {
            System.out.println("Exception caught when trying to listen on port "
                + portNumber + " or listening for a connection");
            System.out.println(e.getMessage());
        }
    }
}
```

## EchoClient

The client is changed to take a shutdown parameter –

```
import java.io.*;
import java.net.*;
 
public class EchoClient {
    public static void main(String[] args) throws IOException {
         
        if (args.length != 3) {
            System.err.println(
                "Usage: java EchoClient <host name> <port number>");
            System.exit(1);
        }
 
        String hostName = args[0];
        int portNumber = Integer.parseInt(args[1]);
        String shutdown = args[2];
 
        try (
            Socket echoSocket = new Socket(hostName, portNumber);
            PrintWriter out =
                new PrintWriter(echoSocket.getOutputStream(), true);
            BufferedReader in =
                new BufferedReader(
                    new InputStreamReader(echoSocket.getInputStream()));
            BufferedReader stdIn =
                new BufferedReader(
                    new InputStreamReader(System.in))
        ) {
            String userInput;
            if (shutdown != null && !"".equals(shutdown)) {
                userInput = shutdown;
            }
            while ((userInput = stdIn.readLine()) != null) {
                out.println(userInput);
                System.out.println("echo: " + in.readLine());
            }
        } catch (UnknownHostException e) {
            System.err.println("Don't know about host " + hostName);
            System.exit(1);
        } catch (IOException e) {
            System.err.println("Couldn't get I/O for the connection to " +
                hostName);
            System.exit(1);
        } 
    }
}
```

## Prunssrv

Ive also created a simple class to stop and start the server –

```
import java.net.*;
import java.io.*;

public class Prunssrv {
    public static void prunsrvStartServer(String[] args) throws Exception {
        String[] newArgs = new String[1];
        newArgs[0] = System.getProperty("prunsrv.port"); // -Dprunsrv.port=8080

        EchoServer.main(newArgs); 
    }
    
    public static void prunsrvStopServer(String[] args) throws Exception {
        String[] newArgs = new String[2];
        newArgs[0] = System.getProperty("prunsrv.server"); // -Dprunsrv.server=localhost
        newArgs[1] = System.getProperty("prunsrv.port"); // -Dprunsrv.port=8080
        newArgs[1] = "shutdown";
        
        EchoClient.main(newArgs);
    }
}
```

Putting it all together –

1. Add the above classes and procrun.exe to a directory – C:\\procrun
2. Compile – javac \*.java
3. Create Archive – jar cvf simpleechoserver.jar \*.class \*.jar

# service.bat

You dont need to create a service.bat file – but its cleaner and simpler. Store this in your code directory.

```
<pre class="">@echo off
setlocal

set SERVICE_NAME=SimpleEchoServer

set PR_INSTALL=%~dp0%prunsrv.exe
set PR_DESCRIPTION="Simple Echo Server Service"

REM Service log configuration
set PR_LOGPREFIX=%SERVICE_NAME%
set PR_LOGPATH=%~dp0%\
set PR_STDOUTPUT=%~dp0%\stdout.txt
set PR_STDERROR=%~dp0%\stderr.txt
set PR_LOGLEVEL=Debug
 
REM Path to java installation
set PR_JVM=%JAVA_HOME%\jre\bin\server\jvm.dll
set PR_CLASSPATH=simpleechoserver.jar
 
REM Startup configuration
set PR_STARTUP=auto
set PR_STARTMODE=jvm
set PR_STARTCLASS=Prunssrv
set PR_STARTMETHOD=prunsrvStartServer
 
REM Shutdown configuration
set PR_STOPMODE=jvm
set PR_STOPCLASS=Prunssrv
set PR_STOPMETHOD=prunsrvStopServer
set PR_STOPTIMEOUT=120
 
REM JVM configuration
set PR_JVMMS=256
set PR_JVMMX=1024
set PR_JVMSS=4000

REM JVM options
set prunsrv_port=8080
set prunsrv_server=localhost

set PR_JVMOPTIONS=-Dprunsrv.port=%prunsrv_port%;-Dprunsrv.server=%prunsrv_server%

REM current file
set "SELF=%~dp0%service.bat"
REM current directory
set "CURRENT_DIR=%cd%"
 
REM start - This takes the input from installService and places it between x's
REM       - if there are none then you get xx as a null check
if "x%1x" == "xx" goto displayUsage
set SERVICE_CMD=%1
REM ahift moves to next field
shift
if "x%1x" == "xx" goto checkServiceCmd
:checkServiceCmd
if /i %SERVICE_CMD% == install goto doInstall
if /i %SERVICE_CMD% == remove goto doRemove
if /i %SERVICE_CMD% == uninstall goto doRemove
echo Unknown parameter "%SERVICE_CMD%"
:displayUsage
echo.
echo Usage: service.bat install/remove
goto end
:doRemove
echo Removing the service '%PR_INSTALL%' '%SERVICE_NAME%' ...
%PR_INSTALL% //DS//%SERVICE_NAME%
if not errorlevel 1 goto removed
echo Failed removing '%SERVICE_NAME%' service
goto end
:removed
echo The service '%SERVICE_NAME%' has been removed
goto end
:doInstall
echo Installing the service '%PR_INSTALL%' '%SERVICE_NAME%' ...
%PR_INSTALL% //IS//%SERVICE_NAME% 
goto end
:end
echo Exiting service.bat ...
cd "%CURRENT_DIR%"
```

## Key Points

- All the Procrun fields are marked with PR\_ – you can also feed these fields directly to procrun.exe using the ++ or — notation in the procrun notes, but i think this way is cleaner and easier to maintain
- The key ones are the start/stop fields
- PR\_JVMOPTIONS – allows us to pass system properties to the Windows Service
- Installing and removing –  
    %PR\_INSTALL% //IS//%SERVICE\_NAME%  
    %PR\_INSTALL% //DS//%SERVICE\_NAME%
- There are other “//” options defined in the notes

# Running service.bat

You may need to run this as administrator –

```
C:\procrun>service.bat

Usage: service.bat install/remove
Exiting service.bat ...
```

To install –

```
service.bat install
```

And uninstall –

```
service.bat remove
```

You can then test –

```
java EchoClient localhost 8080
hello
echo: hello
...
```

If you go to your Windows Services you will now see SimpleEchoServer with stop/start/restart options

# prunmgr.exe

The final trick is to use prunmgr. This the the procrun manager and allows you to see the procrun operating parameters. To get started go to your copy of prunmgr.exe and rename or copy it to the SERVICE\_NAME in your batch file

```
set SERVICE_NAME=SimpleEchoServer
```

So –

```
C:\procrun>copy prunmgr.exe SimpleEchoServer.exe
```

You then run the SimpleEchoServer.exe as administrator –

![simpleechoserver](https://glenware.files.wordpress.com/2016/10/simpleechoserver.png?resize=418%2C400)

# Reference

<https://commons.apache.org/proper/commons-daemon/procrun.html>