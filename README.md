```

__  __  ___ _____ _____  _              _           _     _
|  \/  |/ _ \_   _|_   _|/ \   _ __   __| |_ __ ___ (_) __| |
| |\/| | | | || |   | | / _ \ | '_ \ / _` | '__/ _ \| |/ _` |
| |  | | |_| || |   | |/ ___ \| | | | (_| | | | (_) | | (_| |
|_|  |_|\__\_\|_|   |_/_/   \_\_| |_|\__,_|_|  \___/|_|\__,_|
   _   _       _   _           _                            
  | \ | | __ _| |_(_)_   _____| |                           
  |  \| |/ _` | __| \ \ / / _ \ |                           
  | |\  | (_| | |_| |\ V /  __/_|                           
  |_| \_|\__,_|\__|_| \_/ \___(_)                           


```

## What is going on here?

Well this repository lets you compile pahos mqtt C client
for your android device!

## Why? What for?

Standard busybox implementation for android devices do have this tool in their implementation!

## Wait what ??? what is a busybox?

here you go...

## OK I get it... so whats MQTT again?

Oh ok wait this is what WIKI says about MQTT

"MQTT is a machine-to-machine (M2M)/"Internet of Things" connectivity protocol. It was designed as an extremely lightweight publish/subscribe messaging transport. It is useful for connections with remote locations where a small code footprint is required and/or network bandwidth is at a premium."

## Wow that seems pretty cool for IOT and other interesting things right?

Yes exactly, but not only is it interesting for IOT it can also be very handy for chat applications, home automation, news,updates or social media feeds.

## Nice I like it.. tell me why "Native"

Native simply means that the tool is running without a gui i.e graphical interface "Natively" on the system in our case we execute it from the Android Terminal emulator available from here:

But it doesn't need to be called from the Terminal Emulator you can build your own wrapper such as an APK and call the bin from there.

## Do Other applications use Mqtt?

Yes .. the mostfamous one would be facebook who uses mqtt for their messenger implementation. The difference here is that Mqtt client features are embedded within the function calls deep inside the messengers app source code, whereas MqttAndroidNative can be accesed system wide on the adroid system.


## Perfect I want to try it what should I do?

First you need to be on a Linux machine!

## Huh? What is Linux?

Please stop reading now and hang yourself!

## I love Linux lets do it!

**Dependancies**

* git
* android-ndk

cd into your git development foldeer in my case its just git

```
cd git

```

clone this repository

```
git clone https://github.com/drittesmoskaubrot/MQTTAndroid.git

```
now just enter into the freshly cloned directory

```
cd MQTTAndroid

```

Quickly check out files and directories!


```
ls

```

The out put should be something similiar to this:

```
captainflint@CaptainFlint:~/git/MQTTAndroidNative$ ls -la
total 48
drwxrwxr-x 6 captainflint captainflint 4096 Aug 18 13:36 .
drwxrwxr-x 3 captainflint captainflint 4096 Aug 18 08:56 ..
-rwxrwxr-x 1 captainflint captainflint 5312 Aug 18 12:58 compile.sh
drwxrwxr-x 8 captainflint captainflint 4096 Aug 18 13:36 .git
-rw-rw-r-- 1 captainflint captainflint  642 Aug 18 08:56 header1
-rw-rw-r-- 1 captainflint captainflint  672 Aug 18 08:56 header2
drwxrwxr-x 3 captainflint captainflint 4096 Aug 18 12:40 lib
drwxrwxr-x 2 captainflint captainflint 4096 Aug 18 12:40 output
-rw-rw-r-- 1 captainflint captainflint    1 Aug 18 13:36 README.md
drwxrwxr-x 3 captainflint captainflint 4096 Aug 18 12:40 src
-rwxrwxr-x 1 captainflint captainflint  480 Aug 18 08:56 test_exec.sh

```

If you want you go ahead and check out the sources and other interesting things here for yourself
but I will focus on the build process!

In order to ensure that the build process runs without errors we will have to
change a few things in "compile.sh"

open compile.sh with you favorite text editor I am using vim.

```
vim compile.sh

```

Search for the following lines of code!

```
#  ----> These PATHS need to be changed if NDK directory has different Path or another host has version of NDK

           # ----> change me
     NDK="/home/captainflint/Desktop/Android/android-ndk/"
           # ----> change me
     TOOLCHAIN="$NDK/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/bin/arm-linux-androideabi-gcc"
           # ----> change me
     PLATFORM="$NDK/platforms/android-24/arch-arm"

# END ------>

```

The current PATH set up is for my Machine you most likely
have to change those Paths corresponding to your enviroment!

Once you have changed the enviroment variable simply
execute compile.sh

* make it executable

```
chmod +x compile.sh

```

compile.sh take 2 arguments

* 1. the path to your desired output location
* 2. the path to the src file

I have two files that can be compile straight away which are the standard native
implementations of a subscribe and/or publish mqtt client.
You can have a look at these files in:

```
src/workspace/MQTTAndroidNative_pub.c
src/workspace/MQTTAndroidNative_sub.c

```
So lets compile both of these files with the compile shell script.

Lets say I would like to compile  MQTTAndroidNative_pub.c and my desired output
folder is output, then I simply execute

```
./compile.sh output/MQTTAndroidNative_pub src/workspace/MQTTAndroidNative_pub.c

```

Thats all really the compilation should start immediately wait until it finishes!
the binary can be found now in output and all corresponding share objects are in
lib/ and thats all folks!


## Sorry before you finish up I compiled everything successful but how do I get these files onto my phone?

Ah good question!

On Android devices we have basically two Options to execute and run applications

Deploy your files in:

1. /data/data directories
2. /system/bin directory

Be aware that the library paths have to to be exported if you want to deploy and run you code in custom locations.
such as data/data/<your name of directory>  
If you choose this location you will have to export the path to the libraries
i.e

```
export LD_LIBRARY_PATH="lib/"

```
just change "lib/" to path/to/your/lib

But If you want your Client to be available System wide just copy all files in the lib folder to your
Android Device to system/lib  

```
su

cp -r lib/* /system/lib/

```

And copy your executable binary to /System/bin

```
cp <the name of your file> /system/bin

```
