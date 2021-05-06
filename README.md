# Addendum: What happens if you use different audio encodings while importing?

[(Index)](https://github.com/multiplealiases/Table-of-Contents-Databending-Series/blob/main/README.md)

(This is an addendum to *[Required Reading](https://github.com/multiplealiases/Databending-In-Audacity-Required-Reading/blob/main/README.md)*. References will be made to later articles, but this is more a "look at the pretty pictures" article than a tutorial.)

Normally, I'd advise using unsigned 8-bit PCM, but I like breaking my own rules if it means that you gain an understanding for why I do things the way I do. Nothing stopping you from breaking my personal "rules", of course, but it's better to understand than to blindly follow. There's possible effects in here that I'll outright admit I don't particularly like, but might be of interest to others.

As of this edit, I know 2 places where this matters, Audacity and FFmpeg.  I'll show both for completeness.


## And on to today's test subject
Here's today's test subject. This one's been gathering dust for a month; I've been waiting to use this for something.

![enter image description here](https://i.postimg.cc/qkBN0xcq/quino-al-m-BQIf-Klvow-M-unsplash.jpg)
[Photo](https://unsplash.com/photos/mBQIfKlvowM) by [Quino Al](https://unsplash.com/@quinoal) on [Unsplash](https://unsplash.com)

## Doing absolutely nothing in Audacity


Alright. First thing is to open the JPEG you get from the link in Irfanview, and export it to planar RAW. I'll assume you know how that goes. Import as raw in Audacity, and we see a dialog box that'll be familiar to us by now.

![enter image description here](https://i.postimg.cc/x8mwdd9B/import-raw-data.png)

Now, expand the "Encoding" list.

![enter image description here](https://i.postimg.cc/Hj1W3KS8/expanded-encoding.png)

It's a large list, yes, but let's not get bogged down in details just yet.

For the sake of example, let's do "Signed 16-bit PCM". We'll see the waveform, like so:

![enter image description here](https://i.postimg.cc/RZ0H0wvH/audacity-imported-16-bit-pcm.png)

Now, do absolutely nothing to this "audio". Just save it in the same format as it was imported in. Push it back into Irfanview, and...

![enter image description here](https://i.postimg.cc/KjLF5Gtb/sunset-s16le.jpg)

We haven't even touched it, and it's already decided to corrupt itself. Talk about taking initiative. I think it's some rounding error issue going on here, but I wouldn't know myself here.  I'll just go through a few other formats, doing the exact same thing:

### Signed 8-bit PCM

![enter image description here](https://i.postimg.cc/rFw3d9bL/sunset-s8.jpg)

This one is why I prefer unsigned, not signed. Stuff like these scattered red dots occurs with signed formats. It gets worse later on, trust me.

### Signed 24-bit PCM

![enter image description here](https://i.postimg.cc/Ch0h0kSM/sunset-s24le.jpg)

Looks can be deceiving. Later, when we mess with the "audio" in this format, it's not gonna look this normal.

### Signed 32-bit PCM

![enter image description here](https://i.postimg.cc/jxhsS2SH/sunset-s32le.jpg)

### GSM 6.10/Full Rate codec

![enter image description here](https://i.postimg.cc/nHBFN08w/sunset-gsm.jpg)

### 12-bit DWVM 

![enter image description here](https://i.postimg.cc/2CPjHgtZ/sunset-12dwvw.jpg)

### 16 kbs NMS ADPCM

![enter image description here](https://i.postimg.cc/tXNJmpVM/sunset-16kbs-adpcm.jpg)

I didn't expect that. I write by the seat of my pants, databending while writing, and I wasn't at all aware of this effect. It gets stronger the higher the first number goes!

### 24 kbs NMS ADPCM

![enter image description here](https://i.postimg.cc/R9V6y6dQ/sunset-24kbs-adpcm.jpg)

### 32 kbs NMS ADPCM

![enter image description here](https://i.postimg.cc/xfjCwhvT/sunset-32kbs-adpcm.jpg)

### VOX ADPCM

![enter image description here](https://i.postimg.cc/4f7fR9Ln/sunset-vox-adpcm.jpg)

Yet another viable effect. It's definitely leaning into that "digital psychedelics" aesthetic of the last 3, but it's got its own take on it.

## Doing the ol' Paulstretch
(This section uses techniques from [*The Effect of Paulstretch on Planar RGB Images*](https://github.com/multiplealiases/Planar-RGB-and-Paulstretch/blob/main/README.md). For further information, please refer to that article.)

Let's go back to this screen with our signed 16-bit PCM:

![enter image description here](https://i.postimg.cc/RZ0H0wvH/audacity-imported-16-bit-pcm.png)

Just do a Paulstretch with a Time Resolution of 50 seconds and a Stretch Factor of 1.0:


![enter image description here](https://i.postimg.cc/Dyk25M5g/paulstretch-settings.png)

![enter image description here](https://i.postimg.cc/rFr92K1T/audacity-paulstretched-16-bit-pcm.png)

Save as usual, and get Irfanview to open it...

![enter image description here](https://i.postimg.cc/rMdm27HQ/sunset-s16le-ps50.jpg)

For reference, this is what the same image, Paulstretched in unsigned 8-bit PCM (i.e. my preferred format) with the same settings looks like:

![enter image description here](https://i.postimg.cc/R9g9FGty/sunset-u8-ps50.jpg)

More pictures, in 3... 2... 1...

### Signed 8-bit PCM

![enter image description here](https://i.postimg.cc/02X37wRK/sunset-s8-ps50.jpg)


### Signed 24-bit PCM

![enter image description here](https://i.postimg.cc/TxNH5mKp/sunset-s24le-ps50.jpg)

### Signed 32-bit PCM

![enter image description here](https://i.postimg.cc/38t3GZkL/sunset-s32le-ps25.jpg)
I swear, these four (signed 8, 16, 24, and 32 bit) all have a severely... overamplified, almost deep-fried (in the meme sense) look to them.

### 16kbs NMS ADPCM

![enter image description here](https://i.postimg.cc/KbHHZnJF/sunset-16kbs-adpcm-ps50.jpg)
### 24kbs NMS ADPCM

![enter image description here](https://i.postimg.cc/2kMc2DV3/sunset-24kbs-adpcm-ps50.jpg)
### 32kbs NMS ADPCM

![enter image description here](https://i.postimg.cc/2rp9WrsK/sunset-32kbs-adpcm-ps50.jpg)
I'm being slightly disingenuous with these NMSes here. I don't apply the "Amplify" effect to prevent the "audio" from clipping for these articles, so some images do end up looking very bright.

### VOX ADPCM

![enter image description here](https://i.postimg.cc/YkLyjhsF/sunset-vox-adpcm-ps50.jpg)

## Playing with FFmpeg

(This section uses techniques from [*(Ab)using Lossy Audio Compression to Databend Images*](https://github.com/multiplealiases/Databending-With-FFmpeg/blob/main/README.md). For further information, please refer to that article.)

I'll assume you already know how to add FFmpeg to PATH temporarily. Let's run `ffmpeg -formats`, which will display every format FFmpeg has support for.

~~~
File formats:
 D. = Demuxing supported
 .E = Muxing supported
 --
 D  3dostr          3DO STR
  E 3g2             3GP2 (3GPP2 file format)
  E 3gp             3GP (3GPP file format)
 D  4xm             4X Technologies
  E a64             a64 - video for Commodore 64
 D  aa              Audible AA format files
 D  aac             raw ADTS AAC (Advanced Audio Coding)
 D  aax             CRI AAX
 DE ac3             raw AC-3
 D  ace             tri-Ace Audio Container
 D  acm             Interplay ACM
 D  act             ACT Voice file format
 D  adf             Artworx Data Format
 D  adp             ADP
 D  ads             Sony PS2 ADS
  E adts            ADTS AAC (Advanced Audio Coding)
 DE adx             CRI ADX
 D  aea             MD STUDIO audio
 D  afc             AFC
 DE aiff            Audio IFF
 D  aix             CRI AIX
 DE alaw            PCM A-law
 D  alias_pix       Alias/Wavefront PIX image
 DE alp             LEGO Racers ALP
 DE amr             3GPP AMR
 D  amrnb           raw AMR-NB
 D  amrwb           raw AMR-WB
  E amv             AMV
 D  anm             Deluxe Paint Animation
 D  apc             CRYO APC
 D  ape             Monkey's Audio
 DE apm             Ubisoft Rayman 2 APM
 DE apng            Animated Portable Network Graphics
 DE aptx            raw aptX (Audio Processing Technology for Bluetooth)
 DE aptx_hd         raw aptX HD (Audio Processing Technology for Bluetooth)
 D  aqtitle         AQTitle subtitles
 DE argo_asf        Argonaut Games ASF
 D  argo_brp        Argonaut Games BRP
 DE asf             ASF (Advanced / Active Streaming Format)
 D  asf_o           ASF (Advanced / Active Streaming Format)
  E asf_stream      ASF (Advanced / Active Streaming Format)
 DE ass             SSA (SubStation Alpha) subtitle
 DE ast             AST (Audio Stream)
 DE au              Sun AU
 D  av1             AV1 Annex B
 DE avi             AVI (Audio Video Interleaved)
 D  avisynth        AviSynth script
  E avm2            SWF (ShockWave Flash) (AVM2)
 D  avr             AVR (Audio Visual Research)
 D  avs             Argonaut Games Creature Shock
 DE avs2            raw AVS2-P2/IEEE1857.4 video
 D  avs3            raw AVS3-P2/IEEE1857.10
 D  bethsoftvid     Bethesda Softworks VID
 D  bfi             Brute Force & Ignorance
 D  bfstm           BFSTM (Binary Cafe Stream)
 D  bin             Binary text
 D  bink            Bink
 D  binka           Bink Audio
 DE bit             G.729 BIT file format
 D  bmp_pipe        piped bmp sequence
 D  bmv             Discworld II BMV
 D  boa             Black Ops Audio
 D  brender_pix     BRender PIX image
 D  brstm           BRSTM (Binary Revolution Stream)
 D  c93             Interplay C93
(...)
~~~

Okay, that's a bit much. But the ones we want are a subset of the raw audio formats, which are:

~~~
 DE s16le           PCM signed 16-bit little-endian
 DE s24le           PCM signed 24-bit little-endian
 DE s32le           PCM signed 32-bit little-endian
 DE u8              PCM unsigned 8-bit
 DE alaw            PCM A-law
 DE mulaw           PCM mu-law
~~~

We don't have that many (at least those I can recognize), unfortunately. Just ignore the "little-endian" part; it's one of those computery things involving the order in which digits in numbers are written, and which side is the "end". As long as the start format and end formats are the same endianness, it really doesn't matter which one is which.

Let's pick "s16le". We'll push the raw image data through an audio codec at a very high bitrate, and seeing what happens. My image here is named "sunset.raw"; change it if you're using something else. The commands to run are as follows:

~~~
ffmpeg -f s16le -r 48000 -ac 1 -i sunset.raw -c:a libvorbis -b:a 240k sunset-step1-s16le-libvorbis-240k.mkv
~~~

~~~
ffmpeg -i sunset-step1-s16le-libvorbis-240k.mkv -f s16le -r 48000 sunset-end-s16le-libvorbis-240k.raw
~~~

And throw the resulting image into Irfanview...

![enter image description here](https://i.postimg.cc/p20j1hFL/sunset-end-s16le-libvorbis-240k.jpg)

For reference, the same image in u8 (unsigned 8-bit PCM), keeping all else equal, looks like this:

![enter image description here](https://i.postimg.cc/CY006Pwd/sunset-end-u8-libvorbis-240k.jpg)

And here's possibly the last image dump for this article, depending on if I discover more techniques:

### Signed 8-bit PCM

![enter image description here](https://i.postimg.cc/FFb3pPb2/sunset-end-s8-libvorbis-240k.png)

It's extremely subtle, but just like when we imported it into Audacity, it's suffering from these small red pixels scattered over the image. I don't like signed encodings for this reason.

### Signed 24-bit PCM (s24le)

![enter image description here](https://i.postimg.cc/33Fmmbrp/sunset-end-s24le-libvorbis-240k.jpg)

### Signed 32-bit PCM (s32le)

![enter image description here](https://i.postimg.cc/ZTY3cF0Q/sunset-end-s32le-libvorbis-240k.jpg)

### U-Law (mulaw)

![enter image description here](https://i.postimg.cc/mBMQ2g0m/sunset-end-mulaw-libvorbis-240k.jpg)

### A-Law (alaw)

![enter image description here](https://i.postimg.cc/D7T1WKhn/sunset-end-alaw-libvorbis-240k.jpg)

## Conclusion

And that wraps up this medium-length diversion into different encodings and their effects. I'd suggest moving onto the next article, [*The Effect of Paulstretch on Planar RGB Images*](https://github.com/multiplealiases/Planar-RGB-and-Paulstretch/blob/main/README.md).

---

This work (excluding the images that are explicitly credited to [Unsplash](https://unsplash.com))  is licensed under a
[Creative Commons Attribution 4.0 International License][cc-by].

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
