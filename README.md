# Easy-Utilities
A PowerShell GUI for single click diag info

# Screenshot and Examples

![Screenshot 2024-08-30 154205](https://github.com/user-attachments/assets/0b9af352-9b58-4057-a344-0086d16bbb48)

The GUI contains single click buttons to invoke some of the most common commands one might use for troubleshooting, and clearing useless files

It is especially useful in two areas:

It calls Get-ComputerInfo and automatically excludes all of the (usually) unnecessary info. This is done bt calling sysparams.ps1 which is included with the file. This invokes a web request to call a condensed list of system properties from my github using Invoke-WebRequest:
![cap](https://github.com/user-attachments/assets/6aa4b98e-9c29-46a0-95d0-1ce2b9aa1a30)
This allows the program to automatically exclude any properties that are not contained in this link https://raw.githubusercontent.com/Kylebrody/Powershell-automation/main/infofilter.txt. Then when Get-ComputerInfo is called it will only contain the most commonly needed information, without having to specify anything in the shell. As with all of the commands, it automatically writes a .txt to the directory the .ps1 files are in, and opens it. If a file with that name exists in the directory, it will be replaced.

![ASD](https://github.com/user-attachments/assets/0557ab4b-932f-4603-95b4-cc8d21f8d535)

The second thing this script does is allows you to truly clear chrome cache. As of now, chrome stores multiple cache files in multiple places. One set of cache files is stored in default, and additional sets of cache are set in each individual chrome profile the user might have set up. This script will parse out every user profile and append it to the path to clear all of it regardless of how many chrome profiles are present. EXAMPLE -

Chrome function:
``` $p = Get-Process -Name "chrome"
        $cArray = @(get-childitem "$env:localappdata\Google\Chrome\User Data\profile***" -Name)
        Stop-Process -InputObject $p

        for ($i = 0; $i -lt $cArray.Count; $i++){
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\Cache\Cache_Data" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\DawnCache" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\DawnGraphiteCache" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\DawnWebGPUCache" -Recurse -Force -EA SilentlyContinue -Verbose 
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\Code Cache" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\GPUCache" -Recurse -Force -EA SilentlyContinue -Verbose   
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\History" -Recurse -Force -EA SilentlyContinue -Verbose 
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\History-Journal" -Recurse -Force -EA SilentlyContinue -Verbose
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\InterestGroups" -Recurse -Force -EA SilentlyContinue -Verbose  
            Remove-Item -path "$env:localappdata\Google\Chrome\User Data\$($cArray[$i])\Network Action Predictor" -Recurse -Force -EA SilentlyContinue -Verbose                                                         
        }
        
        Remove-Item -Path "$env:localappdata\Google\Chrome\User Data\Default\Cache" -Recurse -Force -EA SilentlyContinue -Verbose```
Edge funtion:
    {   
        $p = Get-Process -Name "msedge"
        Stop-Process -InputObject $p
        Stop-Process 
        Remove-Item -Path "$env:localappdata\Microsoft\Edge\User Data\Default\Cache\Cache_Data" -Recurse -Force -EA SilentlyContinue -Verbose
    }```

Who would have thought Edge would have the EDGE? (Badda Bing)

# Why am I outputting .txt files?
Firstly I did it just to do it because it seemed cool. Additionally, I made this script when I was thinking about my days in customer facing help desk. This provides a very easy way for people my family/friends who are not comfortable with CMD/Shell to send a technician troubleshooting info (assuming I can't RDP, and they know how to right click/send me an email. In a future update just for me, I will update this script to auto email w/ instructions on how to configure it). Beleieve me, they ask me to look at their PCs a LOT.

#TODO

--Add suppport for more than cache w/ browsers
--Add a button to auto purge .txt files after use
--add controls for more specific system info, i.e. just uptime, last updated, bios etc
--Make the UI look better
--Automate emailing the txt files
--Probably more


