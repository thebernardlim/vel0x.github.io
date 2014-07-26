---
layout: post
title: A Useful PNG Image With Many Different Chunks
date: 2014-07-26 12:00:00
tags: [png, image]
---

In the course of some experimentation at work with image compression, I was
drawn to PNG images. I had known that PNG images used "chunks" for storing data
and that some were optional. If a program didn't understand a chunk which was
marked as optional then it could safely ignore it. An example of this is Adobe
Fireworks, which adds layer information to PNG images so that you can edit PNG
images easily without it being converted to a flat, 2D image. 

A PNG chunk is stored as a tuple. The first value is a length, which is a 4 byte 
integer. This length specifies how long the data value will be. If the length is
0 then the data value does not appear. The third value is this data and the
fourth value is a 4 byte CRC. It is the second value which is of interest
though. This second value stores the chunk type. The type is given by a 4
character name, with all characters being restricted to the range [A-Za-z]. 

The first bit of each byte of the character also stores information (remember, if
the first bit of a character is set then the character is lower case, if it is 
not set it is the same character but uppercase). 

The first bit of the first byte specifies if the chunk is ancillary or not. If
it is set, decoders can safely ignore this chunk. 

The first bit of the second byte denotes whether the chunk is a private chunk,
when it is set, or a public chunk. Public chunks types are well known and
maintained by *The Registration Authority for PNG*[^pngauthority].

The first bit of the third byte is currently reserved and must be set to 0.

The first bit of the fourth byte is used to signal information to PNG editors.
It is a flag informing editors of whether it is safe to copy this chunk even if
it has no idea what the chunk does. This is to stop errors from occurring in
chunk copying should they be expecting a certain ordering. 

This research into PNG chunks was somewhat hindered by the fact that I did not
have an image which I could test things with. With that in mind, I created an
image which can be used for others to test various PNG decoders, etc. The image
is presented below.

![A PNG image of a badly drawn fish which contains many different
chunks.](/images/posts/2014-07-26-PNG-With-Many-Chunks.png)

This PNG has the following headers:

| Chunk Name | Description |
|----------|-----------|
| *IHDR* | Specifies the height, width and some colour information. The chunk in this image says that it is 850x700 pixels, uses 8 bits/sample, uses true colour and is non-interlaced. |
| *sTER* | States that the image is stereoscopic and contains two images contained within the single file. It doesn't actually as that would make the file slightly too complicated, but I've not seen a viewer have an image with it yet. For the sake of interest, it is set to Cross-fuse layout. |
| *sCAL* | Gives the physical scale of the image. In this case it is 1m x 1m. This means that the fish, which takes up around 70% of the image, would be 70cm in real life. |
| *gAMA* | The gamma correction value. Set to 0.45455 |
| *cHRM* | The chromacities of the file. |
| *PLTE* | The palette of the file. |
| *tRNS* | The colour of pixels which should be rendered as transparent. Set to white in this image. |
| *bKGD* | This parameter allows you to specify a background colour which should be used for displaying the PNG on. |
| *pHYs* | Specifies how the pixels are mapped to reality. For example, on a monitor which doesn't have square pixels, a square image may be created. This information can be stored so that it can be rendered as intended later. |
| *IDAT* | The actual pixel data. This can be compressed by algorithms as specified by the IHDR chunk. |
| *tEXt* | Allows you to store text in the image as key=value items. |
| *zTXt* | Similar to tEXt, but this one is compressed. |
| *tIME* | A timestamp containing the last time the PNG was modified. |
| *IEND* | This specifies the end of the PNG data stream. It has no data. |



[^pngauthority]:http://www.w3.org/TR/PNG/#4Concepts.Registration
