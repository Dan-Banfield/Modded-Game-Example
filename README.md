# Picking Our Game
In this example, I have decided to go with the game "Pocket Frogs" by NimbleBit LLC due to its simple nature, and it being made with [Unity](https://unity.com).
//TODO: Add image.


# Resources
To mod this game, we will need the following applications:

 1. [APK Toolkit](https://xdaforums.com/t/tool-apk-toolkit-v1-3-windows.4572881/) - Free APK decompiler.
 2. [dnSpy](https://github.com/dnSpy/dnSpy) - Free, open-source .NET decompiler.

## Decompiling Our APK

 First, we need to get hold of our APK file for the game. This can be found on many websites, however I used [APKMirror](https://www.apkmirror.com/).

 Next, we must use APK Toolkit to decompile the APK like so:
 //TODO: Add image.

Once decompiled, we're interested in two files:
* libil2cpp
* global-metadata

These can be located at Decompiled\lib\armeabi-v7a and Decompiled\assets\bin\Data\Managed\Metadata respectively.

Next, let's use IL2CPP Dumper GUI to dump the native code.
//TODO: Add image.

Once dumped, we should now have a folder called "DummyDll." In here, we're interested in the file "Assembly-CSharp." Once this file has been located, open it in dnSpy to see the dumped code.
//TODO: Add image.

Now, let's look for classes of interest. I found the "Player" class which seems to hold the player's Coins, Stamps and Potions. Let's modify these values!
//TODO: Add image.

Since we're working with ARMv7, we'll need some ARMv7 hex codes.

 - BA 01 45 E3 1E FF 2F E1 is equal to the value 99857989632.

Now let's force these variables to always return this high value.
Open the libil2cpp from earlier in HxD, and grab our offset value for the address of the value we want to modify. 

Let's start with "coins." 
* This is: 0x9C384C.

Back in HxD, press Ctrl + G and enter this value. Next, grab our high-value hex code from earlier, copy it to your clipboard and press Ctrl + B to paste it (overriding the existing value). This method prevents the file from changing its size which would cause the application to crash on load. Once pasted, save the file and replace the original file in the decompiled location with this new file that we have modified.

## Recompiling Our APK

We're almost finished! The final step is to recompile the APK file and test it on our device.

Drag your decompiled folder with the modified IL2CPP lib in it back into APK Toolkit and press "Compile."