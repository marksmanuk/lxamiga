# lxamiga - Linux/Amiga File Transfer

A small script to interface with a Commodore Amiga home computer running the CloanTo Amiga Explorer application.

## Getting Started

Download the Perl script (lxamiga.pl) to your computer.  The only non default package you may need to install is Device::SerialPort.

On the Fedora platform this module can be installed with:
$ sudo dnf install perl-Device-SerialPort

In theory the script may also run on Windows but is untested and you'd probably be better off running the official Amiga Explorer client for Windows.

I have tested with a stock Amiga 600 and Amiga 1200 using both Serial at 19,200 baud and over Ethernet with a 3com Etherlink III PCMCIA card and AmiTCP 4.1.

### Prerequisites

You will need Perl 5 installed and Device::SerialPort.

#### Serial

* Amiga is running Exporer process, default 19,200 8N1 RTS/CTS
* Amiga is connected to Linux with null serial cable, default serial device /dev/ttyUSB0 - using USB/Serial adapter.

#### Ethernet (TCP/IP)

* Amiga is running Explorer process, default 192.168.1.200 port 356
* Amiga is connected to same network as Linux

## Example Usage
```
$ lxamiga -h
Unknown option: h
lxamiga by Mark Street <marksmanuk@gmail.com>

Usage: lxamiga [options]
        -t Use TCP/IP lan connection (dflt. serial)
        -l List available devices
        -d <volume:path> dir
        -r read file <device:volume/path>
        -s send file <file> <device:volume/path>
        -u <file> delete file
        -f <device> Name format disk
        -w <file> write output to filename
        -v Verbose

It's the same commands to use the LAN connection, just use the -t switch first:

     lxamiga -t -l

1. List available devices/volumes:

$ lxamiga.pl -l
Connected to host successfully at 19200
Read 996/996 Bytes 100.0%
14 entries:
 DH0:WORKBENCH           10234 kB    10710 kB 18/01/1996 20:18 RWED                  
 DH1:SIMULATOR           25628 kB    51510 kB 18/01/1996 20:35 RWED                  
 DH2:CREATE              14187 kB    95795 kB 18/01/1996 20:49 RWED                  
 DH3:DOCUMENTS           25220 kB    94095 kB 18/01/1996 21:05 RWED                  
 RAM:Ram Disk              605 kB      611 kB 25/03/2017 15:47 RWED                  
 DF0:Explorer              174 kB      880 kB 16/02/1993 15:40 RWED                  
 DF1:Blank                   2 kB      880 kB 25/03/2017 09:26 RWED                  
 :R:Kick.rom                 0 kB      512 kB 15/07/1993 00:00 R       AMIGA ROM Operating
 :DF0:Explorer.adf           0 kB      880 kB 16/02/1993 15:40 R                     
 :DF1:Blank.adf              0 kB      880 kB 25/03/2017 09:26 RW                    
 :DH0:WORKBENCH.hdf          0 kB    10710 kB 18/01/1996 20:18 RW      SEC:34 SUR:5 RES:2
 :DH1:SIMULATOR.hdf          0 kB    51510 kB 18/01/1996 20:35 RW      SEC:34 SUR:5 RES:2
 :DH2:CREATE.hdf             0 kB    95795 kB 18/01/1996 20:49 RW      SEC:34 SUR:5 RES:2
 :DH3:DOCUMENTS.hdf          0 kB    94095 kB 18/01/1996 21:05 RW      SEC:34 SUR:5 RES:2


2. Format disk:

$ lxamiga.pl -f df1: Blank
Connected to host successfully at 19200
Formatting df1: Blank
Formatting 100.0%
Finished.


3. Send ADF image to disk:

$ lxamiga.pl -s DragonsMegaDemoI.adf :DF1:Blank.adf
Connected to host successfully at 19200
Uploading DragonsMegaDemoI.adf to :DF1:Blank.adf
Sent 901120/901120 Bytes 100.0%


4. Read disk to ADF image:

$ lxamiga.pl -r :DF0:Explorer.adf -w AExplorer.adf
Connected to host successfully at 19200
Read 901120/901120 Bytes 100.0%
901120 bytes saved to AExplorer.adf


5. Read a file from Amiga to local filesystem:

$ lxamiga.pl -r :R:Kick.rom -w kick.rom
Connected to host successfully at 19200
Read 524288/524288 Bytes 100.0%
524288 bytes saved to kick.rom

$ md5sum kick.rom
e40a5dfb3d017ba8779faba30cbd1c8e  kick.rom (Kickstart 3.1)

6. Upload a file to the Amiga (over LAN):

$ lxamiga.pl -t -s ProTracker-3.15.lha RAM:
Connected to 192.168.1.200:356 successfully.
Uploading ProTracker-3.15.lha to RAM:/ProTracker-3.15.lha
Sent 97654/97654 Bytes 100.0%
 
$ lxamiga.pl -t -d RAM:
Connected to 192.168.1.200:356 successfully.
Read 168/168 Bytes 100.0%
4 entries:
 Clipboards              (dir) 28/10/2017 21:27 RWED                  
 ENV                     (dir) 28/10/2017 21:28 RWED                  
 T                       (dir) 28/10/2017 21:28 RWED                  
 ProTracker-3.15.lha     97654 07/04/2017 18:45 RWED                  

```
## Bugs

There is some minor functionality of Amiga Explorer that isn't implemented, e.g. rename, file attributes, etc.  If anyone particularly needs a missing feature I am happy to look into developing it or accept patches.

## History

I released a version of lxamiga many years ago which was written in C++.  This script is now a complete replacement for that legacy version.

## Authors

* **Mark Street** [marksmanuk](https://github.com/marksmanuk)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

