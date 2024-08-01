How to program SAM9X75-SOM modules with SAM-BA 3.8

- SAM9X75D5MN0 : 64Mb QSPI flash memory
- SAM9X75D1GN2 : 64Mb QSPI flash and 2Gb Nandflash memories
- SAM9X75D2GN4 : 64Mb QSPI flash and 4Gb Nandflash memories


This readme file explain how to program the SAM9X75 SOM modules with the right parameters.
Two options are available :
- the flash memories parameters can be explicitely written in the command line, 
- by adding the provided board description files to SAM-BA 3.8, these parameters are no more needed.

Note : please refer to SAM-BA HTML documentation for further explanation about parameters and applet commands (<sam-ba_install_path>/doc/index.html).


1. Using SAM-BA v3.8 without modification

The correct flash applets parameters must be entered in the command line.


1.1 QSPI memory

On all modules, the QSPI flash memory is connected on QSPI0 IO Set1, and the clock frequency shall be set to 50MHz.

The correct parameters to access the QSPI memory are the following:

    ./sam-ba -p serial -d sam9x75 -a qspiflash:0:1:50 -c <applet command>


1.2 Nandflash memory

For modules with a NAND flash (N2 and N4), the Nandflash memory is connected to IO Set 1 and the bus width is 8bits.

PMECC parameters for the 2Mbits Nandflash : 

  <nbSectorPerPage>  = number of sectors per page       : 2^(3)
  <spareSize>        = size of spare zone in bytes      : 128
  <eccBitReq>        = number of ECC Bits Required      : 2 (8-bit)
  <sectorSize>       = number of bytes per sector       : 512
  <eccOffset>        = ECC byte offset in spare zone    : 4
  <usePmecc>         = PMECC used                       : 1 (true)


PMECC parameters for the 4Mbits Nandflash : 

  <nbSectorPerPage>  = number of sectors per page       : 2^(3)
  <spareSize>        = size of spare zone in bytes      : 256
  <eccBitReq>        = number of ECC Bits Required      : 2 (8-bit)
  <sectorSize>       = number of bytes per sector       : 512
  <eccOffset>        = ECC byte offset in spare zone    : 152
  <usePmecc>         = PMECC used                       : 1 (true)



The correct parameters to access the Nandflash memory are the following:

On SAM9X75D1GN2 which embeds a 2Gb Nandflash:

    ./sam-ba -p serial -d sam9x75 -a nandflash:1:8:0xc0104807 -c <applet command>

On SAM9X75D2GN4 which embeds a 4Gb Nandflash:

    ./sam-ba -p serial -d sam9x75 -a nandflash:1:8:0xc2605007 -c <applet command>




2. Using SAM-BA 3.8 with QML files to expand SAM-BA capabilities adding SAM9X75 SOM boards

With this method, the "-b <board_name>" option can used in the command line, and the applet parameters are automatically retrieved from the QML files.

Please unzip the provided archive (sam-ba_3.8_sam9x75-som_addon.7z) into the SAM-BA v3.8 install folder.
This adds 3 new board description files (SAM9X75D5MN0SOM.qml, SAM9X75D1GN2SOM.qml, SAM9X75D2GN4SOM.qml) and their associated JSON files in the appropriate SAM-BA subdirectories.
The "qmldir" file in "qml\SAMBA\Device\SAM9X7" folder must be overwritten by the provided one.

Then it is easy to program the modules just by using their name :

For SAM9X75D5MN0 :

    ./sam-ba -p serial -b sam9x75d5mn0-som -a qspiflash -c <applet command>

For SAM9X75D1GN2:

    ./sam-ba -p serial -b sam9x75d1gn2-som -a qspiflash -c <applet command>
    ./sam-ba -p serial -b sam9x75d1gn2-som -a nandflash -c <applet command>

For SAM9X75D2GN4:

    ./sam-ba -p serial -b sam9x75d2gn4-som -a qspiflash -c <applet command>
    ./sam-ba -p serial -b sam9x75d2gn4-som -a nandflash -c <applet command>



