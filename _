
REM COPY AutoWipeOri
CLS & SET Src=D
FOR %n IN (d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z) DO ( IF NOT %Src%==%n: ( IF EXIST %n:\WINSETUP (
	ROBOCOPY %n: "A:\WipeLog" *.pdf /mov
	ROBOCOPY /mir "%Src%:" %n:
))) & CLS & ECHO Copy Selesai

REM COPY CERTIFICATE WIPE
CLS & FOR %n IN (d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z) DO (
	ROBOCOPY %n: "A:\WipeLog" *.pdf /mov
) & CLS & ECHO Copy Selesai 

REM COPY @MYPC10
CLS & SET Src=D
SET Lbl=@MyPC 10 ALL Exp 2023-09-06
FOR %n IN (d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z) DO ( IF NOT %Src%==%n: ( IF EXIST %n:\SMS (
	ROBOCOPY /mir %Src%: %n:
	ATTRIB -r %n:\MediaLabel.txt
	ECHO Label=%Lbl% > %n:\MediaLabel.txt
	Label %n: %Lbl%
))) & CLS & ECHO Copy Selesai

REM COPY AUTOPILOT or @MYPC10
CLS & SET Src=X
FOR %n IN (d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z) DO ( IF NOT %Src%==%x: (
	ATTRIB -r %n:\autorun.inf
	ROBOCOPY /MIR %Src%: %n:
)) & CLS & ECHO Copy Selesai

REM COPY WIPE
CLS & SET Src=X
FOR %n IN (d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z) DO ( IF NOT %Src%==%n: ( IF EXIST %n:\WINSETUP (
	ROBOCOPY /mir %Src%: %n:
))) & CLS & ECHO Copy Selesai

REM FORMAT FLASHDISK
CLS
diskpart 
list disk 
EXIT 

CLS & SET fs=ntfs
FOR %D IN (2,3,4,5,6,7,8,9,10) DO (
(
ECHO select disk %D
ECHO clean
ECHO create partition primary
ECHO select partition 1
ECHO format fs=%fs% quick override
ECHO active
) > C:\Temp\DP%D.txt
diskpart /s C:\Temp\DP%D.txt & DEL /q C:\Temp\DP%D.txt
)
CLS & ECHO Format Selesai

CLS 
SET OscdimgDir=C:\OS 
SET Label=Autopilot 21H2 
SET Src=C:\OS\Autopilot 21H2 
SET Des=C:\OS\Autopilot 21H2.iso 
 
OSCDIMG -m -o -u2 -udfver102 -bootdata:2#p0,e,betfsboot.com#pEF,e,befisys.bin "A:\Temp2\Windows 10 Enterprise 21H2 x64" "A:\Temp2\Windows 10 Enterprise 21H2 x64.iso"
OSCDIMG -m -o -u2 -udfver102 "A:\WipeWin10 2207" "A:\WipeWin10 2207 new.iso"
OSCDIMG -d -n -o -oc -oi -h -k -m -l"O365" "D:\O365" "D:\O365.iso"
OSCDIMG -m -o -u2 -udfver102 "B:\Wonderware 2014 R2" "B:\Wonderware 2014 R2.iso"

CLS
SET Lbl=AutopilotWipeOri 2211
SET Src=X:\AutopilotWipeOri 2211
SET Des=X:\AutopilotWipeOri 2211.iso
OSCDIMG -m -o -u2 -udfver102 -l"%Lbl%" "%Src%" "%Des%"
CLS & ECHO Create ISO Selesai

FOR %n IN (d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z) DO (
	ROBOCOPY U: %n: HASH.bat
)

dism /Get-WimInfo /WimFile:D:\sources\install.esd 
dism /export-image /SourceImageFile:F:\sources\install.esd /SourceIndex:5 /DestinationImageFile:"D:\WIM_Compare\install.wim" /Compress:max /CheckIntegrity 
Dism /Split-Image /ImageFile:"C:\OS\Autopilot 21H2\sources\install.wim" /SWMFile:C:\OS\install.swm /FileSize:1000 
DISM /Mount-image /imagefile:D:\WIM_Compare\Win10_1909_x64-01-3-Windows-10-Enterprise.wim /Index:1 /MountDir:D:\WIM_Compare\PMI 
DISM /Mount-image /imagefile:D:\WIM_Compare\install.wim /Index:1 /MountDir:D:\WIM_Compare\ORI 
dism /unmount-Wim /MountDir:D:\WIM_Compare\ORI /discard 
dism /cleanup-wim 
DISM /image:C:\ /optimize-image /boot 
Dism /Capture-Image /ImageFile:D:\WipeOriWim\install.wim /CaptureDir:C:\ /Name:"Windows 10 Pro" 
 
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSET\Policies\Microsoft\FVE /v RDVDenyWriteAccess /t REG_DWORD /d 0 /f 
 
FOR %x IN (01,02,03,04,05,06,07,08,09,10,11,12,13,14,15,16,17,18) DO ( 
REN "A:\Operating System\Autopilot Add\Recovery.zip.0%x" Recovery.0%x 
) 
 
DISM /Online /Export-Driver /Destination:"C:\Project\Wipe 2023\DriverComputerWipe" 
DISM /Online /Add-Driver /Driver:D:\DriverComputerWipe /Recurse 
PNPUTIL /Add-Driver D:\DriverComputerWipe\*.inf /subdirs /install 
