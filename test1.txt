@echo off

:start
cls

echo.............................................................................
echo.  Enter 1, 2 OR 3 etc. To select your task, or 9 to EXIT.                  .
echo.............................................................................

echo 1 - Display User/PC Info
echo 2 - Check/Create Directory Structure
echo 3 - Create Logfile
echo 4 - Backup Logfile
echo 5 - File Comparison
echo 6 - View File Attributes
echo 7 - View Folder permissions
echo 8 - List Non-System Processes
echo.
echo 9 - Exit
echo.
echo Type your preferred option and then press ENTER.

rem setting result as the variable that will store the user's input
set /p result=Chosen Option:

if %result%==1 (
rem output the user's/pc information to the screen
echo ----------------------------------------------
echo Hello %USERNAME%, you are currently logged into
echo %COMPUTERNAME%
echo it is %TIME% . on %DATE%
echo -----------------------------------------------

pause
cls
goto start
)

if %result%==2 (
rem Error checking
if NOT EXIST C:\batch\logs  echo "The following directory has been created-> C:\batch\logs directory"
if NOT EXIST C:\Backup\logs echo "The following directory has been created-> C:\batch\logs directory"

rem Creating a new directory 
mkdir C:\batch\logs
mkdir  C:\Backup\logs

pause
cls
goto start
)

if %result%==3 (
rem Copying the output of option 1 to a txt file
echo ----------------------------------------------  >> C:\OpSystems\Logfile.txt
echo Hello %USERNAME%, you are currently logged into >> C:\OpSystems\Logfile.txt
echo %COMPUTERNAME%                                  >> C:\OpSystems\Logfile.txt
echo it is %TIME% . on %DATE%                        >> C:\OpSystems\Logfile.txt
echo ----------------------------------------------  >> C:\OpSystems\Logfile.txt

rem Moving the logfile to another folder
MOVE C:\OpSystems\logfile.txt  C:\batch\logs

rem Error Checking
if exist Logfile.txt (echo Logfile.txt HAS BEEN CREATED) else (echo LogFile.txt already exists)

pause
cls
goto start
)

if %result%==4 (
rem Making a copy of the contents in the logs folder
xCOPY /s C:\batch\logs  C:\Backup\logs

rem Error Checking
if EXIST     C:\Backup\logs\logfile.txt echo BACKUP of logfile.txt is COMPLETE
if NOT EXIST C:\Backup\logs\logfile.txt echo complete task 3 first before this one

pause
cls
goto start
)

if %result%==5 (
rem Error Checking
if NOT EXIST C:\batch\logs\logfile.txt  echo complete task 3 first before this one
if NOT EXIST C:\Backup\logs\logfile.txt echo complete task 4 first before this one 

rem Comparing the 2 files
fc c:\batch\logs\logfile.txt c:\Backup\logs\logfile.txt

pause
cls
goto start
)

if %result%==6 (
if EXIST c:\batch\logs\logfile.txt (
echo The file attribute[s] of the following text file are/is:
attrib c:\batch\logs\logfile.txt
echo ---------------------------------------------------
echo       R =  Read-only file attribute.
echo       A =  Archive file attribute.
echo       S =  System file attribute.
echo       H =  Hidden file attribute.
echo       O =  Offline attribute.
echo       I =  Not content indexed file attribute.
echo       X =  No scrub file attribute.
echo       V =  Integrity attribute.
echo       P =  Pinned attribute.
echo       U =  Unpinned attribute.
echo       B =  SMR Blob attribute.
echo ---------------------------------------------------
)ELSE echo Create the LogFile first by choosing option 3, then you can view its attributes

pause
cls
goto start
)

if %result%==7 (
rem Viewing folder permissions for the backup folder
icacls c:\backup

pause
cls
goto start
)

if %result%==8 (
tasklist /fi "Status eq RUNNING"
pause
cls
goto start
)

if %Result%==9 (
choice /M "Are you sure you want to quit"

if ERRORLEVEL 2 goto start
if ERRORLEVEL 1 goto end
)

:end