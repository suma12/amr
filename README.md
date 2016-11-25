# amr
Conversion of raw AMR RTP payload packets to .amr storage format

## Introduction
AMR (Adaptive Multi-Rate) and AMR-WB (Adaptive Multi-Rate Wideband) are audio codecs optimized for speech coding. They are defined by 3GPP [1,2]. RFC 4867 [3] defines format of RTP payload and file storage format.

The purpose of this project is to provide conversion from RTP payload (as captured e.g. by Wireshark) to AMR storage format (which can be played/converted to .wav/mp3 by AMR player [4]).

## Usage
```
usage: amr.py [-h] [-w] [-a] [-n N_CHAN] [-v] [-V] raw [amr]

Convert raw RTP stream to .amr

positional arguments:
  raw                   raw file
  amr                   amr file

optional arguments:
  -h, --help            show this help message and exit
  -w, --wideband        raw is AMR-WB (AMR if False)
  -a, --octet-align     raw is octet-align (bandwidth eff. if False)
  -n N_CHAN, --n-chan N_CHAN
                        number of channels (1-6)
  -v, --verbose
  -V, --version         show program's version number and exit
```

### Capture/storage of AMR RTP payload with Wireshark
On Wireshar 2.2.1 (the procedure may differ slightly in other releases)

1. Filter packets of interest (e.g. by 'rtp')
2. Telephony -> RTP -> Stream analysis
3. Save -> Forward/Reverse stream audio, Save as type (*.raw)
4. Guess information about AMR format: AMR vs AMR-WB, octet-align vs bandwidth efficient, number of channels (e.g. from SIP signalization, RTP layer) and provide them to `amr.py`.

## Unit test
The code implements BitIterator and BitMerger classes which allows read/write bit streams. You can run simple unit tests as `python -m unittest amr.TestBit`.

## Known issues 
* Neither interleaving nor CRC are supported.
* It was observed that after comfort noise speech frame (frame type 9 for AMR-WB) followed by RTP with the Marker bit set to true, an abundant amount of zero bytes is inserted into the raw stream. A fast dirty solution is implemented in the code, which skips these zero bytes.

## License
The code is licensed under [GNU Lesser General Public License, version 2.1](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.en.html).

## References
1. [3GPP TS 26.101](http://www.3gpp.org/ftp/Specs/html-info/26101.htm) (AMR)
2. [3GPP TS 26.201](http://www.3gpp.org/ftp/Specs/html-info/26201.htm) (AMR-WB)
3. RFC 4867 - RTP Payload Format and File Storage Format for the Adaptive Multi-Rate (AMR) and Adaptive Multi-Rate Wideband (AMR-WB) Audio Codecs
4. [AMR Player](http://www.amrplayer.com)
