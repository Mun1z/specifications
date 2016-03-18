File Specification SPR
---

The SPR file contains data of sprites. A sprite is a non-transparent image of 32x32 pixels.
Only the colored pixels are stored in the file. The magenta color (#FF00FF) is used to indicate the transparent pixels.

#### File Structure

| Bytes        | Comment                                                                     |
|--------------|:----------------------------------------------------------------------------|
| 4 (u32)      | File Signature                                                              |
| 2/4 (u16/u32)| sprite number in the file. 2 bytes for versions below 960                   |
| 4 (u32)      | Sprite Address 1                                                            |
| 4 (u32)      | Sprite Address 2                                                            |
| 4 (u32)      | Sprite Address 3                                                            |
| <b>.</b>     | <b>.</b>                                                                    |
| <b>.</b>     | <b>.</b>                                                                    |
| <b>.</b>     | <b>.</b>                                                                    |
| 4 (u32)      | sprite address N                                                            |
| 1 (u8)       | Sprite 1 - colorkey channel red                                             |
| 1 (u8)       | Sprite 1 - colorkey channel green                                           |
| 1 (u8)       | Sprite 1 - colorkey channel blue                                            |
| 2 (u16)      | Sprite 1 - data size                                                        |
| N            | Sprite 1 - sprite data                                                      |
| 1 (u8)       | Sprite 2 - colorkey channel red                                             |
| 1 (u8)       | Sprite 2 - colorkey channel green                                           |
| 1 (u8)       | Sprite 2 - colorkey channel blue                                            |
| 2 (u16)      | Sprite 2 - data size                                                        |
| N            | Sprite 3 - sprite data                                                      |
| 1 (u8)       | Sprite 3 - colorkey channel red                                             |
| 1 (u8)       | Sprite 3 - colorkey channel green                                           |
| 1 (u8)       | Sprite 3 - colorkey channel blue                                            |
| 2 (u16)      | Sprite 3 - data size                                                        |
| N            | Sprite 3 - sprite data                                                      |
| <b>.</b>     | <b>.</b>                                                                    |
| <b>.</b>     | <b>.</b>                                                                    |
| <b>.</b>     | <b>.</b>                                                                    |
| 1 (u8)       | Sprite N - colorkey channel red                                             |
| 1 (u8)       | Sprite N - colorkey channel green                                           |
| 1 (u8)       | Sprite N - colorkey channel blue                                            |
| 2 (u16)      | Sprite N - data size                                                        |
| N            | Sprite N - sprite data                                                      |

### File reading

- Header
    - Read the signature file.
    - Read the number of sprites.
    - Storing the cursor position on stream.

```JavaScript

var signature =  // -----< read 4 bytes unsigned stream >-----
var spriteCount = // -----< read 2/4 bytes unsigned stream. 2 for versions < 960 , or 4 bytes to 960 or higher >-----
var headSize =  // -----< get the current position of the stream >-----

```

- Address
    - Check if the id provided is valid.
    - Set the cursor position in the stream according to the id provided. The address of a sprite can be read within the limits from 0 to the number of sprites, however, address 0 represents a sprite emptiness, no color pixel and need not be read from the file .
    - Read the sprite address
    - Set the cursor position in the stream according to the read address.

```JavaScript

function readSprite(id)
{
    // Check if the id is valid
    if (id > spriteCount)
    {
        // -----< Should send a message error or returns in a empty sprite >-----
    }
    
    // The id 0 represent a empty sprite
    if (id == 0)
    {
        // -----< return a empty sprite >-----
    }
    
    // The sprite address 1 is 0, then subtract 1 from the supplied id.
    // Each address has 4 bytes in length, then we multiply by 4.
    // The first address is read immediately after reading the 'head'.
    var position = ((id - 1) * 4) + headSize;
    
    // -----< Set the position of the stream by the variable position >-----
    
    var address = // -----< read 4 bytes from the stream >-----
    
    if (address == 0)
    {
        // -----< return a empty sprite ??? >-----
    }
    
    // -----< Set the stream position in variable address >-----

```

- Data
    - Read the colorkey.
    - Read the data size
    - Read the data. Only colored pixels we can read. The first 2 bytes represent the amount of
      transparent pixels to the next color pixel. The next 2 bytes the amount of
      colored pixel to the next pixel transparent. Then, the 3 bytes of pixel color is read.

```JavaScript
    
    // Colorkey
    var red = // -----< read 1 byte stream >-----
    var green = // -----< read 1 byte stream >-----
    var blue = // -----< read 1 byte stream >-----
    
    var pixelDataSize = // -----< read 2 bytes from the stream >-----
    
    if (pixelDataSize == 0)
    {
        // -----< A sprite without data ??? Return an empty sprite >-----
    }
    
    var read = 0;
    var currentPixel = 0;
    
    while (read < pixelDataSize)
    {
        var transparentPixels = // -----< read 2 bytes unsigned stream >-----
        var coloredPixels = // -----< read 2 bytes unsigned stream >-----
         
        currentPixel += transparentPixels;
        
        // reads the colored pixels
        for (var i = 0; i < coloredPixels; i++)
        {
            var red = // -----< read 1 of the stream >-----
            var green = // -----< read 1 of the stream >-----
            var blue = // -----< read 1 of the stream >-----
            
            currentPixel++;
        }
        
        // read += ( transparentPixels(u16) + coloredPixels(u16) ) + ( coloredPixels * ( red(u8) + green(u8) + blue(u8) ) )
        read += 4 + (coloredPixels * 3);
    }
}

```
