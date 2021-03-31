# open-ondd 
Open Source File Decoder for Othernet Data Broadcast

This is a Free Software (Open Source) file decoder for [Othernet](http://othernet.is/) which aims to provide a Free Software alternative to `ondd`.

With open-ondd you can receive the files that are broadcast by the Othernet file broadcast service and the time packets that are broadcast by the Othernet
time service.

All of this software is the result of a reverse engineering effort, since Othernet  (previously known as Outernet) does not publish any documentation for the protocols they use.

You can use `free-outernet.py` to receive UDP packets in real-time from
gr-outernet or with a KISS file recorded previously by
gr-outernet. `free-outernet.py` recovers the files that are transmitted by
Othernet and prints the time packets. It also prints some interesting debug info.


Things that are not implemented/supported yet:

 * X.509 signature checking. The file announcements of the Othernet file service
   are signed with the Othernet X.509 certificate to prevent spoofing. This will
   not be implemented, as I have no interest in checking the signature. Perhaps
   some Amateur Radio operators or other people whish to use the Othernet
   protocols to exchange files, so it does not make sense for me to require
   that file announcements are signed.
 * Using the time packets to set the system time. I do not know how useful it
   is for most people. `ondd` does it, because it is designed to run standalone
   without internet conetivity. If you have Internet connectivity it is much
   better to use NTP. If you do not have Internet connectivity and/or your need
   precise timing, it is better to use GPS. The Othernet time service only has a
   resolution of 1 second and it does not seem to account for round trip time to
   geostationary orbit (around 200ms).
 * Automatic decompression of received files. Previously, most (perhaps all) files sent by
   the Othernet file service were `.tar.bz2` files. `ondd` extracts the files
   automatically. Othernet no longer sends .tar.bz2 files. 

## Dependencies

To install all dependencies just run: `pip3 install -r requirements.txt`

 * [crcmod](https://pypi.python.org/pypi/crcmod)
 * [zfec](https://pypi.python.org/pypi/zfec)

## Sample KISS files

You can use some [sample KISS files](https://drive.google.com/open?id=0B2pPGQkeEAfdbXFZNThCb1BLMzg) for testing.

## About LDPC decoding for files

LDPC decoding has been [implemented by George
Hopkins](https://github.com/daniestevez/free-outernet/pull/4) by reverse
engineering `ondd`.

Previously, this was not implemented, so `free-outernet` needed to receive all
the blocks for a file to be able to reconstruct it correctly. Now, it can use
the LDPC to "fill in" the missing packets in case some of them were lost.

The performance of `free-outernet` regarding FEC and LDPC decoding should
be now the same as the performance of `ondd`.
