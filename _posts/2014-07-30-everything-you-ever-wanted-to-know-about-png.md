---
layout: single
title: Everything You Ever Wanted to Know About PNGs 
date: 2014-07-30 12:00:00
tags: [png, image]
---

For many years I have believed that PNG is the greatest image file format for
lossless images that ever existed. I had a rough idea how it worked. There were
chunks, some of which were required, some were optional, and one of the required
ones had all the data in it. Other than that PNG was a mystical black box. 

In my current internship I've been trying to optimise the transfer of images to
a mobile device. I've tried different formats, compression methods and have some weird
and wonderful ideas for future work. One of the things I looked at was how to
squeeze every single byte out of a PNG file. When researching this I learned
there was a lot more to the PNG file format than I never expected. In order to
save others from the hours of sleuthing when trying to learn about PNG, I
decided to write a blog post giving an overview of the format. If you want to
know more about PNG, read on. 

## The PNG File Format

The very first part of a PNG image is the PNG file signature. The signature is
there to identify a file as a PNG file as well as allowing detection of file
transfer problems. The header is always equal to the following 8 byte sequence:
`89 50 4E 47 0D 0A 1A 0A`

The first two bytes are used to identify PNG files on systems which use the
first two bytes to uniquely identify files. The first byte is deliberately not
an ASCII character to reduce the chance that the file is recognised as a text
file. It also allows detection of problems where some transfers can accidentally
clear the highest bit in a byte. 

Bytes 2, 3, and 4 serve as a further identifier. These bytes are the text `PNG`
when treated as ASCII characters. If you open a PNG file in a text editor then
you will see these characters at the beginning of the file. 

The next two bytes are a carriage return and line feed. This is a further check
to ensure that a bad file transfer hasn't altered the file.

The next byte stops the file from being displayed on MS-DOS (A ctrl-Z
        character). 

The final byte is the opposite of the carriage return and line feed check. 

Any software which deals with PNG files must confirm that the header is exactly
as expected. If it is not exactly the same then the software should abort. 

### Chunks

Following the header there is a sequence of chunks. Chunks are the core of the
PNG format with all further information being contained within a chunk. Each
chunk is stored as a tuple of either 3 or 4 values and are laid out as follows.

#### First Value

The first value is a length, which is a 4 byte integer. This length specifies 
how long the data portion of this chunk will be.

#### Second Value

The second value is the chunk identifier. This chunk identifier is a 4
character ASCII sequence. This sequence not only identifies the chunk, but also
contains some other information. Remember, the difference between an uppercase
and lowercase ASCII character is if you set bit 5 or not. 

    01000001 -> A
    01100001 -> a

If the first character is lower case then it means that the chunk is ancillary.
A decoder can safely ignore this chunk if it doesn't know what it is. If it is
uppercase then it is a critical chunk, which the encoder must be able to handle. 

If the second character is lowercase then the chunk is private. This means that
it was created by someone or some company and they don't expect anyone else to
be able to deal with it. If it is uppercase then it is a public chunk. Public
chunks are chunks which are well known. These are maintained by *The
Registration Authority for PNG*[^pngauthority].

If the fourth character is lowercase then this chunk may be copied when
modifying the file. If it is uppercase then the chunk must not be copied. The
reason for this being that the chunk may be dependent on the data in the image.
If the image changes then this chunk should also be updated, but cannot be if
the editor does not understand it. 

#### Third Value

The third value in the chunk tuple is the data of the chunk. If the length
specified by the first value is 0, then this value does not appear. 

#### Fourth Value

The fourth value is a 4 byte CRC.

### Public chunks

From this point you have everything you need to know about the basic format of
PNG images. However, it is useful to know some of the ones which are more common
in PNG files, including some which must always appear. 

#### Required Chunks

##### IHDR

This chunk must appear, as can be seen from the uppercase first character, and
it must be the first chunk in the file. The chunk has the following fields:

* Width - 4 bytes
* Height - 4 bytes
* Bit depth - 1 byte
* Colour type - 1 byte
* Compression method - 1 byte
* Filter method - 1 byte
* Interlace method - 1 byte

The width and height fields are obvious as to their function. 

The bit depth is the number of bits per sample, or per palette index if using one. The possible
values are 1, 2, 4, 8 and 16, but restrictions are in place depending on the
colour type selected[^colourtyperestrictions]. 

The colour type is set to either 0, 2, 3, 4, or 6, with these numbers corresponding to greyscale, truecolour,
indexed-colour, greyscale with alpha or truecolour with alpha. 

The compression method is always set to 0 and means that deflate compression
with a sliding window of 32K will be used. 

The filtering method is an identifier for a preprocessing step which makes the
compression more efficient. This is always set to 0, but each row of pixels is filtered
with one of 5 basic filter types. An example of this is an image which is
greyscale and moves from black to white and is 256 pixels wide. The data
representation of the image would look something like:

    000 001 002 003 004 ... 253 254 255

This sequence is difficult to compress. However, if we apply the *sub* filter to the image then this changes the data to be a
difference from the previous neighbour. The sequence now looks like the
following:

    000 001 001 001 001 ... 001 001 001

This data is obviously very easy to compress. Filtering can hugely boost the
compression ratio of the image. 

The final field is the interlace method. This can be set to 0, which means that
there is no interlacing, or 1, which means use Adam7 interlacing. When using
interlacing, the image is made up of multiple images of smaller resolution.
These images can be composed together to create the complete image. Further
information on the Adam7 interlacing algorithm can be found here:
<http://en.wikipedia.org/wiki/Adam7_algorithm>
A pictorial representation, found on the Wikipedia page is the following: 

![Pictorial representation of the Adam7 interlacing
algorithm](http://upload.wikimedia.org/wikipedia/commons/2/27/Adam7_passes.gif)

##### PLTE

This chunk represents the palette of the image. It can contain anywhere between
1 and 256 entries, with each entry being a 3 byte RGB tuple. This chunk is
marked as required, but only must appear if the colour type, specified in the
*IHDR* chunk is 3. It can optionally appear if the colour type is 2 or 6, but
must not appear for the remaining 2. There should also only be one *PLTE* chunk. 


##### IDAT

This chunk is the meat of PNG files. It contains the image data, after being
filtered and run through the compression algorithm. The compression algorithm,
as specified previously, is always the deflate algorithm. This runs on the
filtered data. The filters are applied to each row independently, using basic
rules to determine which one to use as trying all possible combinations would
take an extremely large amount of time. Going into the individual filtering
algorithms is beyond the scope of this post, but should you wish to learn more,
have a look here: <http://www.w3.org/TR/PNG/#9Filters> Alternatively, if this
post gets enough interest I may write one later. 
         
##### IEND

This chunk ends a PNG file. It has no data.

#### Ancillary Chunks

There are many ancillary chunks and it would do no one any favours to describe
them all. Here are a few of the most common. 

##### tRNS

This chunk specifies the transparency options of the file. If this chunk is
present, it specifies a colour which is to be rendered as transparent. This may
be done by specifying the colour directly, or in the case of there being a
palette, it specifies the index. 

##### gAMA

This chunk specifies the gamma information. Since all displays are not exactly
the same, we occasionally need to include further information to help display
the image as intended in various locations. For further information on gamma in
PNG look here: <http://www.libpng.org/pub/png/spec/1.2/PNG-GammaAppendix.html>

##### tEXt

This chunk can contain information in a key-value format. The key and value are
separated by a null byte. Only one key-value pair is allowed per chunk. The key
can be anything up to 79 bytes in length, however the following are predefined
and should be used if appropriate: *Title, Author, Description, Copyright, CreationTime, Software, Disclaimer, Warning, Source, Comment*

##### tIME

This chunk specifies the last modification time of the image. The layout is as
follows:


| Unit      | Size (b)  | Notes                             |
|--------   |---------- |--------------------------------   |
| Year      | 2 bytes   | Full year, not just two digits    |
| Month     | 1 byte    | 1-12                              |
| Day       | 1 byte    | 1-31                              |
| Hour      | 1 byte    | 0-23                              |
| Minute    | 1 byte    | 0-59                              |
| Second    | 1 byte    | 0-60 (accounts for leap seconds)  |


##### bKGD

This chunk specifies a background colour to be used when displaying the image. 

##### pHYs

This chunk gives the relationship between pixels and real world measurements.
For example, specifying that the image was scanned at 300dpi, which allows us to
then print the image at the correct size once more. 

### Conclusion

So with all the information there about PNG files, what can you do with it? Well
the first thing is, you can optimise any PNG images you have. The tool
[Pngcrush](http://pmt.sourceforge.net/pngcrush/) is designed to make your PNG
files smaller by removing unnecessary chunks. It also optimises the compression
of the data by modifying the filters used and the compression level. 

The next thing you can do is to start adding extra information to your images.
Whether these are on your website, used in an application, or just part of a
photo album, you can add much more data to your images without relying on an
extra storage location. To play around with viewing PNG chunks and adding and
removing them, I recommend the
[TweakPNG](http://entropymine.com/jason/tweakpng/) tool. 

Finally, one of the things I really depended on when learning about PNG images
was a good reference. I originally used my own, but I discovered that there was
a very handy PNG image with many different chunks bundled with the libpng source
code. This image is below. All I can say now, is have fun exploring!

![libpng sample image](/images/posts/2014-07-30-libpng-test.png)


[^pngauthority]:http://www.w3.org/TR/PNG/#4Concepts.Registration

[^colourtyperestrictions]:http://www.w3.org/TR/PNG/#table111
