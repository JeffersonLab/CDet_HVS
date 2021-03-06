# HVS
#File: README
#Date: 24 May 2001
#Mod&Add: 23 Nov 2001. Rawprotocol has changed. Call to mainframe by it hostname not by index
 
This directory includes *.java sources files, classes and packages
for running of program RCS Calorimeter High Voltage System(HVS) 
on the Java platform version 1.3.x or better(to check Java version 
type 'java -version').
RCS Calorimeter High Voltage System Program (HVS) 
written to control and monitoring of high voltage 
mainframes of model LeCroy-1458 through the network using TCP/IP connection.

Files and directories:

Makefile - make file to compile *java files to *classes and make 
	documentaion on classes in html format.
hvs - command file to start HVS program. This file includes command string
        with parameters to start java. One from parameters(map.file) set 
        path and filename for map file location. While starting 
        HVS program try to load this map file.
HVS.java, HVS.class - main class to init and start HVmainMenu class 
HVmainMenu.java, HVmainMenu*.class - shows GUI with main menu window 
	with hvmainframe tables and status window.
hvclient.c, hvcli - source and compiled file of client program to 
	communicate with hvframe or with HVS server.
mkhvcli - include command string to compile hvclient.c
hvs.map - map file to load when HVS starts
hvframe  - directory with package 'hvframe' includes basic HVS classes:
	 HVframe.java, HVmodule.java, HVmonitor.java, HVclient.java,
	 HVmoduleTable.java, HVMessage.java, HVCommad.java  
	   
hvtools  - directory with package 'hvtools' includes utility HVS classes:
	 Cmd.java,DecimalField.java, ErrorMessages.java, FormattedDocument.java
	 HVFrameList.java, HVListDialog.java, HVListener.java, HVStatus.java,
	 Parameters.java, UpAttribute.java
hvserver - directory with package 'hvserver' includes  HVS server classes:
	 HVServer.java, HVServerProtocol.java, HVServerThread.java 
hvmap    - directory with package 'hvmap' includes  HVS detector map classes:
	 HVmapTable.java, HVmapMonitor.java
images   - directory includes image icons used by HVS program.


To compile all *.java files to class, type 'make all' in current directory.
To make documentation on classes in html format type 'make doc'
in current directory.

 
Keywords:

 hvframe, hvmainframe, mainframe - means high voltage mainframe LeCroy-1458
 hvmodule hvunit  - means high voltage unit(module) installed in mainframe.
 hvchannel, channel - means high voltage channel of module.	
 HVFrame  - means java class representing properties of the mainframe
 HVmodule  - means java class representing properties of the module.
 HVchannel  - means java class representing properties of the channel.


Start HVS program.

   To run HVS program type './hvs' from  current directory. After instalation
of the hvs package one should edit script 'hvs' to modify pathes to the 
java and to program. 
If program starts first time or file 'HVFrameList.props' with list of hvframe
hostnames not found in user directory, the window 'HV Host List' is pop-up.
You need to crate list of hvmainframes by hostname and listening port.
(see 'Add/Remove HVFrame List' below ). 
The initialization of mainframes is begin after that. 
In this time the follows process are happened:
  - Main Window with Menu, Tables and Status window are created
    (classes HVmainMenu, HVStatus are used )	
  - list of hostnames for TCP/IP connection is loaded from file 
    HVFrameList.props 
    (classes HVHostList, Parameters are used)
  - 'hvclient' threads to communicate with hvframes are started
    (one 'hvclient' for one mainframe)
    (classes HVclient, HVmessage  are used)	
  - HVFrames, HVmodules and HVchannels objects are initialized
    (get inforamation about current frame status,
    installed modules, channels properties and so on)
    (classes HVframe,HVmodule,HVchannel,HVMessage,HVCommand,HVclient are used)
  - tables with connected frames, modules and channels properties 
    and data are created
    (class HVmoduleTable is used)
  - 'hvmonitor' threads are started to moitoring  data changing
     with period in 3sec. (One 'hvmonitor' per one mainframe)
    (class HVmonitor is used)	 
  - 'server' thread is started to reply command string from external
     clients to hvmainframes.
    (classes HVServer, HVServerThread, HVServerProtocol are used)
  		
The flow chat of messages between HVS classes in file HVS_classes.ps 
is shown.

After that the screen with menu bar, tables and status window is pop-up.

Menu bar includes next menus: 'File', 'Edit', 'View', 'Map' and 'Help'.

Menu 'File' includes following items:
  'Load Voltage Set' - to load settings of demand voltage from file.
  'Save Voltage Set' - to store demand voltage settings to file.
  'Save Settings'    - to store all settable values to file 
  'Print'            - to print selected hvframe tables with channels 
	               parameters
  'Quit'             - to exit from program with confirmation

   Format of stored file with voltage settings is follows:
hostname:port slot_number parameter value1 value2 ... valueN ,
hostname:port - hostname and TCP/IP port of mainframe 
slot_number - slot number of module begins with "S" char 
              (S0,S1 in example) 
parameter - name of hvmodule properties(parameter) to store
	      (DV demand voltage in example)
value1,.. - values of parameter for every channel in module
Example: Store demand voltage(DV) of mainframe with hostname
	 host1:1090 with 2 modules in slots(0 and 1) with 12 channels each
    	    
 host1:1090 S0 DV -15.0 -14.0 -45.0 -10.0 -10.0 -10.0 -1.0 -1.0 -1.0 -1.0 -1.0 -1.0
 host1:1090 S1 DV -10.0 -10.0 -40.0 -10.0 -10.0 -10.0 -1.0 -1.0 -1.0 -1.0 -1.0 -1.0


Menu 'Edit' includes following items:
  'Add/Remove Frame List' - to open window for add or remove hostname of
	                    hvmainframes from list.
  'Enable All Channel' - to enable all channels of all hvmainframes	
  'Disable All Channel' - to disable all channels of all hvmainframes
  'Options' - not implemented yet.	
 	
'View' includes follows menu items:
  'SysInfo' - to get system information from selected hvmainframe
  'ModuleInfo' - to get information about selected hvmodule
		(model, serial number, number of channels ...)
   All this information displays in 'Status' window.

'Map' includes follows menu items:
   'View Map' - opens map window with status of hvchannels in coordinates of 
		calorimeter front view. 
	This window includes table with HVON/HVOFF  status of all channels 
	by coresponded icon and menu bar with menu: 'File', 'View', 'Help'
	'File' includes follows menu items:
	   'Print' - to print map window;
	   'Close' - close map window.
	'View' includes next menu items:
	   'Channel Status'   - to view HVON/HVOFF status of channels 
				in table cells as icon;
	   'Measured Voltage' - to view measured voltage of channels in 
				table cells;
	   'Setting Voltage   - to view demand voltage of channels in 
				table cells;
	   'Measured Current  - to view measured current of channels in 
				table cells;

	The mouse double click on selected channel cells will pop-up window 
	with all parameters for this channel and bring up to focus the table 
	with this channel in HVS main window. 

   'Load Map' - load calorimeter map (hvchannels addresses) from map file. 
	The format of map file is follows:
 	first string is list of hostnames of mainframes:
 	hostname1:port1 hostname2:port2 ...
 	next strings represented rows(Y-coordinates) of calorimeters
 	The channel address position in that strings represented 
 	columns(X-coordinates) of calorimeter:
	# x,y calorimeter channels placement
	#   X:
	#    22  ... ... ... ... ... ...   01

	# Y:
	# 32
	# 31
	# ...
	#  2
	#  1

	The channel address format is follows: 
   	#H#SS#CC - address of HVmainframe channel,
     	#H - one digit number(1-9) of mainframe by its location in 
	hostname string above (1 -is first )
     	#SS - two digit slot number(0-15)
     	#CC - two digit channel number(0-11)          

  For example: next strings represented channels for hvframe with 
  hostname1:port1 for module 0 and for channels from 0 to 11 
10000 10001 10002 10003 10004 10005 10006 10007 10008 10009 10010 10011
	
Menu 'Help' menu not implemented yet.


  Table.

Table displays values of all properties of selected module in 
selected mainframe for all channels.
Columns of table are values of properties(parameters).
Rows of table are channels of module.
Modules are presented as horizontal tabbed panel(tabs on top side), 
with printed number of slot in tab title.
To select module, move cursor on tab with given slot number 
and press left mouse button.
Mainframes are presented as vertical tabbed panel (tabs from left side),
with printed hostname:port of mainframe in tab title.
To select mainframe, move mouse cursor on tab with given hostname and
press left mouse button.
Values displayed in table cells in blue color can be edited.
To select cell for editing once click left mouse button, while
cursor placed on cell.
To edit selected cell, once click left mouse button, delete old value
and enter new value. If make double-click after cell selection,
old value will be marked and new value can be entered.

From left side of table buttons 'ON' and 'OFF' are placed for on/off
selected hvmainframe.


  Add/Remove HVFrame List
Enter in left window hostname of first hvmainframe and press 'Add' button.
The port number 1090 add to hostname after ':' by default. If another
port number is used by hvframe enter after hostname ':portnumber' and
press 'Add' button. The 'hostname:portnumber' appears in up window.
Enter next hostname(:portnumber) of next hvmainframe. If need remove
some hostname from list, select string with hostname for removing
and press 'Remove' button. This hostname disappears from list.
To exit from this window press 'Close' button. 
NOTE: If new hostname added to the hosname list, hvframe with this 
      hostname is connected and its initialization is started.

External requests.
One command at a time is strongly recommended for Ethernet controlled
mainframes when one host is controlling multiple mainframes.
To communicate with hvmainframes the program 'hvclient'(source hvclient.c)
is used. This program get as parameters the hostname, portnumber and
message for sending to given host:

 hvclient -h hostname -p portnumber -m 'message to send'

For example, next command send command message "SYSINFO"
	(get system information) to the hvmainframe with
	hostname 'hallahv1' on port 1090:

 hvclient -h hallahv1 -p 1090 -m 'sysinfo'

If 'HVS' use connections to this hvmainframe, the 'hvlient' should be started 
with another parameters :
 hvclient -h hostname -p portnumber -m 'framehostname  message to send'
 in this case hostname - name of host where HVS is running,
              portnumber = 5555(this port number used by HVS server)
	      framehostname - hostname of hvmainframe in hostname list.

For example, next command sends command message "SYSINFO" for mainframe with
	hostname=hv01 (get system information) to the server on 'host', 
	where HVS is running:

 hvclient -h host -p 5555 -m 'hv01 sysinfo'

Message may contains any enabled command-message for mainframe.
Responses from mainframe printed to standard output and may be
redirected to the file.
The server running in HVS program supports another format of 
command messages to process messages with channel addressing 
in calorimeter coordinates. 
The command message is null terminated string with ASCII characters
The format of this command message is follows:
 <command> <property> <channel(s)_address> [<channel(s)_address>] [<value>]
      <command> - command word to server, one of follows :
	          SET - set <value> for <property> for channels with
			address <channel(s)_address>. This command valid only
	 		for next <properties> : DV - demand voltage;
	             			        CE - channel enable;
	          GET - get values of <property> for channels with 
			address <channel(s)_address>. This command valid only
	 		for next <properties> : DV - demand voltage;
	             			        CE - channel enable;
	             			        MV - measured voltage;
		             			MC - measured current;
					 	ST - channel status;
	          UP -  set new bigger value of <property> for channels with 
			address <channel(s)_address> on <value>. This command 
			valid only for next <property> : DV - demand voltage;
	          DOWN -  set new lower value of <property> for channels with 
			address <channel(s)_address> on <value>. This command 
			valid only for next <property> : DV - demand voltage;

 	<property> - at this time only next properties are supported:
				DV - demand voltage;
	           		CE - channel enable;
	             		MV - measured voltage;
		             	MC - measured current;
				ST - channel status;

	<channel(s)_address> - address of one or more channels in x,y coordinates
			of calorimeter in next format: 
        (x1 and y1 are integer numbers from 1 to 22 for x and from 1 to 32 for y)
         	x1,y2 - one channel with coordinates x=x1 and y=y1
         	x1:xn,y1 - channels with coordinates x=x1,x2,..,xn and y=y1
        	x1,y1:yn - channels with coordinates x=x1 and y=y1,y2,..,yn 
         	x1:xn,y1:ym -channels with coordinates x=x1,x2,..,xn 
				and y=y1,y2,..,ym
	<value> - digital number. For CE property it is '1' (enable channel) 
			or '0' (disable channel);
		  For DV property it is digital number represented voltage 
		set up or down, or digital number ended with character '%'
		represented percents to change of current voltage set value  
		up or down. 

  1458 LeCroy HV mainframe 
(short description from 1454/1458 HV Mainframe User's Guide)
Mainframe consists from 16 slots for high voltage modules,
and has various interfaces for communication: serial RS-232,
ARCNET, ETHERNET.
To communicate with mainframe TCP/IP protocol is used in HVS program.
For using telnet, type 'telnet hvhostname',
on password prompt enter: 'lrs1450' (by default).
The commands can be entered directly from terminal.
List of generally used commands are:
 SYSINFO - returns the mainframe system information.
 LL  - returns the logical unit list in terms of slot-submodule, 
	one for each logical unit in the order of logical unit number.
 LD  -  load values for single property(for a number channels of logical unit) 
 HVON/HVOFF  - switches the high voltage on and off. 
 IMOFF - immediate turn off HV generation. 
 RC    - recall command returns the values for a given property 
	 for a channel or all channels in a module.
 SRC   - super recall command returns the values for each property given
	as an argument for all channels and all modules.
 PROP  - returns a list of the properties supported by specified logical unit 
Command Messages.
The format of  command messages for modifies values as follow:
 <command channel(s) property(s) value(s)>
 
  Slots, Modules/Submodules, Logical Units, and Channels.
The 1450 system contains multiple slots which may or may not contain modules.
A module may span more than one slot but is addressed only by a single 
slot number. Modules always contain one or more submodules. The term logical
unit is used to refer to submodules and modules with only one submodule.
Logical units generally have multiple channels. All channels of a logical
unit share identical properties and the values for these properties 
may not be channel independent

Logical Unit Specification:
Example:
Mainframe with 2 modules in slots 1 and 3 with the module 
in slot 1 containing one submodule and the module in slot 3 
containing 4 submodules results in 5 logical units for mainframe.
The slot/module specifications in this case being S1, S3S0,S3S1,S3S2,S3S3
with corresponding logical unit number specifications L0,L1,L2,L3,L4,L5,
respectively.

Channel Specification String:
 S1      All channels in module in slot 1
 S1.3    Channel 3 of slot 1
 S4S2.1  Channel 1 of submodule 2 in slot 4
 L0.3    Channel 3 of logical unit 0.
 L1      All channels of logical unit 1 

Module Properties.

The following properties are considered 'golden' and a will probably
appear in all HV modules. The attributes listed are examples only.
 CE - channel enable (label:Ch_En,units:, measured,range:0-1, format:%1s)
 DV - demand voltage (label:Target_V, units:V, range:-3000-0, format:%7.1f)
 HVL - hardware voltage limit(label:HV_LIM, units:V, measured, range:7, 
	format:%7.1f) 
 MC - measured current (label:Meas_uA, units:uA, measured, range:7 format%7.1f)
 MV - measured voltage (label:Meas_V, units:V, measured, range:7 format%7.1f)
 MCDZ - measured currnet dead zone (label:MC_Zone, units:uA, range:0-100,
        format:%7.1f)
 MVDZ - measured voltage dead zone (label:MV_Zone, units:V, range:0-100,
        format:%7.1f)
 RUP - rump Up rate (label:RUp_V/s, units:V/s, range:300, format:%7.1f)
 RDN - rump Down rate (label:RDn_V/s, units:V/s, range:300, format:%7.1f)
 ST - channel status (label:Status, measured, range:4, format:%4x)
 TC - trip current (label:Trip_uA, units:uA, range:1000 format:%7.1f)




