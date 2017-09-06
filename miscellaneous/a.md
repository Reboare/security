# Steganography

This is the art of hiding information within other files so that they appear to be something else.  This article will only go over the most common and obvious methods of hiding information as there are numerous possible ways of actually hiding data.

The obvious is to simply check for information encoded as text within the file.  Running cat on the file can show the most obvious examples of hidden information.  In examples like this it will be stored outside of the boundaries of the bytes marking the file type.  Running `strings` can have the same effect.

Data hidden within exif is another popular method of hiding data, but finding this is as simple as running something like [exiftool](https://www.sno.phy.queensu.ca/~phil/exiftool/).

```
cat file.png
strings file.png
exiftool file.png
```

It can also be worth doing a preliminary check with [steghide](http://steghide.sourceforge.net/).  Ocassionally this is password protected so if you're sure this method is used, then either the password must be discovered or possible brute-forced.

```
steghide extract -sf file.png
```

## Audio

* Images can be hidden within the spectrogram of an audio file, the encoding method seen [here](https://solusipse.net/blog/post/basic-methods-of-audio-steganography-spectrograms/).  The best tool for this is [Sonic Visualiser](http://www.sonicvisualiser.org/).  
* Data can be hidden within the Least-Significant bit of the audio file.
  * The [Derbycon writeup](https://ethackal.github.io/2015/10/05/derbycon-ctf-wav-steganography/) gives an example of decoding this kind of data. 
* d
* d
* d
* 


