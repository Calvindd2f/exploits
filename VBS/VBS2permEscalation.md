<# USAGE

elevate.vbs /File:"test.cmd" "Welcome" NextAEscapedQuote ^^^^^^^" NextupSomeSpecialChars "!@#$%^^&*()_+-="";\|/?" "What about This?" That's Enough ^^^^^^^".

#>


# CODE ????

[elevate.vbs]
	
<# 

		Set strArg=WScript.Arguments.Named
    If Not strArg.Exists("File") Then
    Wscript.Echo "Switch /File:<File> is missing."
    WScript.Quit 1
    End If

    Set strRdlproc = CreateObject("WScript.Shell").Exec("rundll32 kernel32,Sleep")
    With GetObject("winmgmts:\\.\root\CIMV2:Win32_Process.Handle='" & strRdlproc.ProcessId & "'")
    With GetObject("winmgmts:\\.\root\CIMV2:Win32_Process.Handle='" & .ParentProcessId & "'")
        If InStr (.CommandLine, WScript.ScriptName) <> 0 Then
            strLine = Mid(.CommandLine, InStr(.CommandLine , "/File:") + Len(strArg("File")) + 8)
        End If
    End With
    .Terminate
    End With
    CreateObject("Shell.Application").ShellExecute "cmd", "/c " & chr(34) & chr(34) & strArg("File") & chr(34) & " " & strLine & chr(34), "", "runas", 1
			#>


#In case you want the VBS code without the pass argument part, use the code below.


[elevate.noArgs.vbs]
	
<#
	
      Set strArg = WScript.Arguments.Named
      If Not strArg.Exists("File") Then
       Wscript.Echo "Switch /File:<File> is missing."
        WScript.Quit 1
      End If
      CreateObject("Shell.Application").ShellExecute strArg("File"), ELAV, "", "runas", 1


#>

	
Hybrid [elevate.cmd]

<#
	 
<!-- : Begin batch script

@ECHO OFF

FSUTIL dirty query "%SYSTEMDRIVE%" >NUL || (
   ECHO(*************************************
   ECHO(Invoking UAC for Privilege Escalation
   ECHO(*************************************
   CSCRIPT //nologo "%~f0?.wsf" //job:ELAV /File:"%~f0" ^^^^^^^& Wscript.Echo hacked? "Haha Nope" --file "Moz Fir.exe" nextsomespecialsymbols "!@#$%^^&*()_+-="";\|/?"  ^^^^^^^"
   EXIT /B
)

ECHO Running as elevated user
ECHO(
ECHO(%1
ECHO(%2
ECHO(%3
ECHO(%4
ECHO(%5
ECHO(%6
ECHO(%7
ECHO(%8
ECHO(%9
ECHO(%*
PAUSE
EXIT /B

	   #>



----- Begin wsf script --->
   
   
<#
  
   <package>
   <job id="ELAV">
       <script language="VBScript">
           Set strArg=WScript.Arguments.Named

           If Not strArg.Exists("File") Then
               Wscript.Echo "Switch /File:<File> is missing."
               WScript.Quit 1
           End If

           Set strRdlproc = CreateObject("WScript.Shell").Exec("rundll32 kernel32,Sleep")
           With GetObject("winmgmts:\\.\root\CIMV2:Win32_Process.Handle='" & strRdlproc.ProcessId & "'")
               With GetObject("winmgmts:\\.\root\CIMV2:Win32_Process.Handle='" & .ParentProcessId & "'")
                   If InStr (.CommandLine, WScript.ScriptName) <> 0 Then
                       strLine = Mid(.CommandLine, InStr(.CommandLine , "/File:") + Len(strArg("File")) + 8)
                   End If
               End With
               .Terminate
           End With

          CreateObject("Shell.Application").ShellExecute "cmd", "/c " & chr(34) & chr(34) & strArg("File") & chr(34) & " " & strLine & chr(34), "", "runas", 1
       </script>
   </job>
</package>
	       
#>
