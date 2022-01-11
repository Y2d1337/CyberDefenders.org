# DeepDive Challenge
![This is an image](/DeepDive/Images/DeepDivehead.png)

## Challenge Link
https://cyberdefenders.org/labs/78

## Challenge Information
- Image Size: 	 526 MB
- Image Format: .mem file
- Tags: 
    -  	Volatility 
 

## Scenario
> You have given a memory image for a compromised machine. Analyze the image and figure out attack details.

## Tools Used
    • Volatility
        ◦ Volatility is an open-source memory forensics framework for incident response and malware analysis.     
         
## Questions:  
### 1 What profile should you use for this memory sample?
To determine the image profile i used volatility plugin `imageinfo`  

#### Explanation:
>imageinfo- Identify information for the image

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem imageinfo`

![q1](/DeepDive/Images/q1.png)

> **Flag: Win7SP1x64_24000**

### 2 What is the KDBG virtual address of the memory sample?
To get the KDBG virtual address i used volatility plugin `kdbgscan`

#### Explanation:
>kdbgscan - Search for and dump potential KDBG values

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 kdbgscan`

![q2](/DeepDive/Images/q2.png)

> **Flag: 0xf80002bef120**

### 3 There is a malicious process running, but it's hidden. What's its name?
To find the hidden process we need to use `psxview`,this plugin compares the active processes indicated within `psActiveProcessHead` with any other possible sources within the memory image.

A False within the column indicates that the process is not found in that area

#### Explanation:
>psxview - Find hidden processes with various process

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 psxview`

![q3](/DeepDive/Images/q3.png)

> **Flag:  vds_ps.exe**

### 4 What is the physical offset of the malicious process?
`psxview` plugin shows the physical address of each process, so we can looked at 3rd questions output/

![q4](/DeepDive/Images/q4.png)

> **Flag:  0x000000007d336950**

### 5 What is the full path (including executable name) of the hidden executable?
To find the full path of the hidden executable we need to use `filescan` plugin

#### Explanation:
>filescan - Pool scanner for file objects

#### Commandline:
`vol.py -f /home/sansforensics/Desktop/banking-malware.vmem --profile=Win7SP1x64_24000 filescan | grep vds_ps.exe` 

![q5](/DeepDive/Images/q5.png)

> **Flag:  C:\Users\john\AppData\Local\api-ms-win-service-management-l2-1-0\vds_ps.exe**



### 6 Which malware is this?
### 7 The malicious process had two PEs injected into its memory. What's the size in bytes of the Vad that contains the largest injected PE? Answer in hex, like: 0xABC
### 8 This process was unlinked from the ActiveProcessLinks list. Follow its forward link. Which process does it lead to? Answer with its name and extension
### 9 What is the pooltag of the malicious process in ascii? (HINT: use volshell)
### 10 What is the physical address of the hidden executable's pooltag? (HINT: use volshell)