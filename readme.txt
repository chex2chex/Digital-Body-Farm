#Distributed Digital Body Farm (DDBF)
The DDBF is designed to monitor how files decay after deletion. It intercepts delete system calls before file deletion, records file information, and then monitors
each sector occupied by the file on disk post deletion. It contains a filesystem filter driver, a Windows service application, an SQLIte database, a Python 
executable, and a central repository.


DISCLAIMER: 
This program is designed for research purposes only. It has been tested on Windows 10/11 and NTFS 3.1, however, we do not provide any guarantees of 
its suitability to your environment. Please exercise caution as you use it. We do not accept any liabilities including loss of data or damage to systems/programs 
arising from its use.


INSTALLATION
(1) Turn off driver signing or turn on test signing to allow the filter driver to load.
(2) Unzip the Project folder.
(3) Fire up command propmt in administrative mode and navigate to the Project folder.
(4) Type bodyfarm.exe -install to install the program.
	(a) You may have to create an exception in your Antivirus program. 
	(b) If you get an error, its probably because your Antivirus is preventing the installation.
	(c) If the installation is successful, a Virtual Hard Disk (VHD) called BODYFARM (Z:) will be created.
	(d) To check if it installed successfully, type sc query sectorinfo (sectorinfo is the name of the service the program runs).
	(e) Upon installation, a log file (delete.log) will be created in a folder (logs) in the project folder to log program execution information.
(5) To uninstall type bodyfarm.exe -uninstall

RUNNING THE PROGRAM
(1) To start : Type bodyfarm.exe -start (The VHD (Z:) will be mounted)
(2) To stop: Type bodyfarm.exe -stop (The VHD (Z:) will be unmounted)
(3) To check program/service state (RUNNING/STOPPED): Type sc query sectoinfo

NOTE
(1) Once the program starts up, a log file (delete.log) and folder (logs) will be created in drive Z: to hold information about every file 
    deleted from the system.
(2) An SQLite database file (sector.db) is also created in Z: to hold files of interest - specified in the config.ini file.
(3) To add more files to the list, just add the file extension to the config.ini file. You will have to restart the 
    program for the change to take effect.
(4) The program is currently set to insert files into the database if they are 10 MB and below. This setting can be adjusted in the config.ini file.

THE CHECKER PROGRAM
(1) The checker (Body_Farm_Checker.exe) is a python script compiled as an executable program. 
(2) The executable does not require python to be installed. However, it needs to run with admin privileges.
(3) You may have to create an exception in your Antivirus program.
(4) The checker runs once in 24 hours. The hour and minute must be set in the config.ini file (before starting the program).
(5) Once run, the checker updates the database if it is found that any sectors/files have been overwritten on disk. 
(6) After checking is complete, the database file is saved in the project folder as a compressed file. A copy is also sent to the repository.
(7) The checker creates a log file (bodyfarmchecker.log) in the logs folder in Z:, the log is updated each time the checker runs. 