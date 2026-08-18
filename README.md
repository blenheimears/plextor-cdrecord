# plextor-cdrecord
Patches for the schily-tools version of cdrecord to support Plextor drives with locked commands (Premium 2, PX-755, PX-760)

Works on an unmodified drive with stock firmware. This allows you to use GigaRec, VariRec, etc. You do NOT need the "free your Plextor" firmware to use this.

This has only been tested on a Plextor Premium 2.

The MD5 and Blowfish code, and all new functions are public domain. Binaries and Modifications to original cdrecord functions are under the CDDL license.

Overview of the challenge-response mechanism:

Before the drive will respond to Plextor vendor commands (eg. GigaRec, VariRec, read EEPROM, etc), the computer must query the drive and retrieve a 128-bit random number which we will call the challenge. The computer then uses a value called the seed, which is 0x3d144a9c2b4a53632598f2a5c4ad03cf, to generate the key. The key is MD5(seed). It then generates a second value which I will call "clear", which is the challenge decrypted with Blowfish in ECB mode. The response is then MD5(clear). This challenge-response exchange must be done for (almost) every vendor command each time the command is issued. The exception is for quality scanning, where the exchange only needs to be done before the scan, not for every single command issued during the scan.
