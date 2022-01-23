# Spotlight Challenge - ***Work in progress***
![This is an image](/Spotlight/Images/spotlighthead.png)

## Challenge Link
https://cyberdefenders.org/blueteam-ctf-challenges/34

## Challenge Information
- Image Size: 	 55 MB
- Image Format: .ad1 file
- Tags: 
    - MAC Forensics
    - OSX Forensics 
    - Autopsy  
    - Steganography
 
## Scenario
> Spotlight is a MAC OS image forensics challenge where you can evaluate your DFIR skills against an OS you usually encounter in today's case investigations.

## Tools Used
    •Mac_apt
        ◦ DFIR (Digital Forensics and Incident Response) tool to process Mac computer full disk images (or live machines) and extract data/metadata useful for forensic investigation.
    •DB Browser for SQLite
        ◦ DB Browser for SQLite (DB4S) is a high quality, visual, open source tool to create, design, and edit database files compatible with SQLite.
    •Steghide
        ◦ Steghide is a steganography program that is able to hide data in various kinds of image- and audio-files. The color- respectivly sample-frequencies are not changed thus making the embedding resistant against first-order statistical tests.     
    •Plist Explorer
        ◦ This tool may be used to parse the data content of a .plist file.
    • FSEventsParser
        ◦ FSEventsParser can be used to parse FSEvents files from the '/.fseventsd/' on a live system or FSEvents files extracted from an image.
  
  
  
## Intro:
If you are not familiar with MacOS forensic i recommend to read the following articles

- https://medium.com/about-developer-blog/macos-forensics-diy-style-3369868505dd

- https://davidkoepi.wordpress.com/2013/07/06/macforensics4/

- http://thexlab.com/faqs/maintscripts.html
          
## Questions:  
### 1 What version of macOS is running on this image?
MacOS version are located in file named `SystemVersion.plist`

`root\System\Library\CoreServices\SystemVersion.plist`

![q1](/Spotlight/Images/q1.png)

> **Flag: 10.15**


### 2 What "competitive advantage" did Hansel lie about in the file AnotherExample.jpg? (two words)
To find the file i used powershell command `gci -recurse -filter "AnotherExample.jpg" -File -ErrorAction SilentlyContinue`

![q2a](/Spotlight/Images/q2a.png)

In the folder i saw two files created at the same time..

![q2c](/Spotlight/Images/q2c.png)

`secret` and the `AnotherExample.jpg`

The contant of the secret file was `!Our newest phone will have helicopter blades and six cameras and <"flip phone"> technology!`

With all that i assumed that there something hidden inside that jpg (Steganography)

I open `AnotherExample.jpg` with `HxD Editor`

![q2b](/Spotlight/Images/q2b.png)

Scrool down to the end and saw the hidden text

> **Flag: flip phone**


### 3 How many bookmarks are registered in safari?

Safari folder located at `root\Users\hansel.apricot\Library\Safari`, the bookmarks file named `bookmarks.plist`

![q3](/Spotlight/Images/q3.png)

I counted the sites with notepad++

> **Flag: 13**


### 4 	What's the content of the note titled "Passwords"?
To find the answer i needed to know where the notes application files are located.

I came across with this article

https://www.swiftforensics.com/2018/02/reading-notes-database-on-macos.html

From the article i understand that the notes files are located at `\root\Users\hansel.apricot\Library\Group Containers\group.com.apple.notes` and the notes itself stored in `NoteStore.sqlite-wal` file.

i open `NoteStore.sqlite-wal` with `HxD Editor` 

![q4](/Spotlight/Images/q4.png)

> **Flag: Passwords**


### 5 Provide the MAC address of the ethernet adapter for this machine.

The `daily.out` file contains information relating to disk usage and networking.

The file located at `/private/var/log/'

![q9](/Spotlight/Images/q9.png)
	
> **Flag: 00:0C:29:C4:65:77**


### 6 Name the data URL of the quarantined item.
To find the answer i needed to know where the quarantined items are located.

From https://eclecticlight.co/2020/10/29/quarantine-and-the-quarantine-flag/

I found the location `\root\Users\sneaky\Library\Preferences` and the file `com.apple.LaunchServices.QuarantineEventsV2`

Opened the file with SQLbroswer 

![q6](/Spotlight/Images/q6.png)

> **Flag: https://<i></i>futureboy.us/stegano/encode.pl**

### 7 What app did the user "sneaky" try to install via a .dmg file? (one word)
One of the things i does when i doing digital forensics, is always checking the deleted files on the system :)

The location of deleted files is `root\Users\sneaky\.Trash`

![q7](/Spotlight/Images/q7.png)

After i finished this question i found another evidence related to the user tried to install .dmg file

The file `.zsh_history`

#### Explanation:
>All the commands you type in your ZSH session will be automatically stored in the history file for reuse. To view all the commands stored in the . zsh_history file

![q7](/Spotlight/Images/zhistory.png)

We can see the attempts to install of `silenteye-0.4.1b-snowleopard.dmg`

> **Flag: silenteye**


### 8 What was the file 'Examplesteg.jpg' renamed to?
Before we can start, we need to understand where we can see the system event log.

I found video of SANS DFIR Summit on youtube that explain it

https://www.youtube.com/watch?v=bv5gu5reKEA

In the video they recommending using `FSEventsParser` tool to parse `FSEvents` files from the '\root\.fseventsd'

>Usage: FSEParser_V4.exe -s SOURCE -o OUTDIR -t SOURCETYPE [folder|image] [-c CASENAME -q REPORT_QUERIES]

Open the output file `FSEvents.sqlite` with `SQL browser`, then i filter `ExampleSteg.jpg` to see all the events.

I wrote down the first event id and clear all filters

![q8a](/Spotlight/Images/q8a.png)

Then i filtered the event id and follow the event until the next "renamed" flag

![q8b](/Spotlight/Images/q8b.png)

> **Flag: GoodExample.jpg**

### 9 How much time was spent on mail.zoho.com on 4/20/2020?



![q8b](/Spotlight/Images/q8b.png)

> **Flag: 20:58**

### 10 What's hansel.apricot's password hint? (two words)

> **Flag: Family Opinion**

### 11 The main file that stores Hansel's iMessages had a few permissions changes. How many times did the permissions change?

> **Flag: 7**

### 12 What's the UID of the user who is responsible for connecting mobile devices?

> **Flag: 213**

### 13 Find the flag in the GoodExample.jpg image. It's hidden with better tools.

> **Flag: helicopter**

### 14 What was exactly typed in the Spotlight search bar on 4/20/2020 02:09:48

> **Flag: term**

### 15 What is hansel.apricot's Open Directory user UUID?

> **Flag: 5BB00259-4F58-4FDE-BC67-C2659BA0A5A4**