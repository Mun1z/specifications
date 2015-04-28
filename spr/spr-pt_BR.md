Especificação do arquivo SPR
---

O Arquivo SPR contém os dados dos sprites. Um sprite representa uma imagem não transparente de 32x32 píxeis.
Apenas os píxeis coloridos são armazenados no arquivo. A cor magenta (#FF00FF) é usada para indicar os píxeis transparentes.

#### Estrutura do arquivo

| Bytes        | Comentário                                                                  |
|--------------|:----------------------------------------------------------------------------|
| 4 (u32)      | Assinatura do arquivo                                                       |
| 2/4 (u16/u32)| Quantidade de sprites no arquivo. 2 bytes para versões abaixo de 960        |
| 4 (u32)      | Endereço do sprite 1                                                        |
| 4 (u32)      | Endereço do sprite 2                                                        |
| 4 (u32)      | Endereço do sprite 3                                                        |
| <b>.</b>     | <b>.</b>                                                                    |
| <b>.</b>     | <b>.</b>                                                                    |
| <b>.</b>     | <b>.</b>                                                                    |
| 4 (u32)      | Endereço do sprite N                                                        |
| 1 (u8)       | Sprite 1 - colorkey canal red                                               |
| 1 (u8)       | Sprite 1 - colorkey canal green                                             |
| 1 (u8)       | Sprite 1 - colorkey canal blue                                              |
| 2 (u16)      | Sprite 1 - tamanho dos dados                                                |
| N            | Sprite 1 - dados do sprite                                                  |
| 1 (u8)       | Sprite 2 - colorkey canal red                                               |
| 1 (u8)       | Sprite 2 - colorkey canal green                                             |
| 1 (u8)       | Sprite 2 - colorkey canal blue                                              |
| 2 (u16)      | Sprite 2 - tamanho dos dados                                                |
| N            | Sprite 3 - dados do sprite                                                  |
| 1 (u8)       | Sprite 3 - colorkey canal red                                               |
| 1 (u8)       | Sprite 3 - colorkey canal green                                             |
| 1 (u8)       | Sprite 3 - colorkey canal blue                                              |
| 2 (u16)      | Sprite 3 - tamanho dos dados                                                |
| N            | Sprite 3 - dados do sprite                                                  |
| <b>.</b>     | <b>.</b>                                                                    |
| <b>.</b>     | <b>.</b>                                                                    |
| <b>.</b>     | <b>.</b>                                                                    |
| 1 (u8)       | Sprite N - colorkey canal red                                               |
| 1 (u8)       | Sprite N - colorkey canal green                                             |
| 1 (u8)       | Sprite N - colorkey canal blue                                              |
| 2 (u16)      | Sprite N - tamanho dos dados                                                |
| N            | Sprite N - dados do sprite                                                  |

### Leitura do arquivo

- Header
    - Ler a assinatura do arquivo.
    - Ler a quantidade de sprites.
    - Armazenar a posição do cursor no stream.

```JavaScript

var signature =  // -----< ler 4 bytes não assinados do stream >-----
var spriteCount = // -----< ler 2/4 bytes não assinados do stream. 2 para versões anteriores à 960 ou 4 bytes para 960 ou maior >-----
var headSize =  // -----< obter a posição atual do stream >-----

```

- Address
    - Checar se o id fornecido é válido. 
    - Definir a posição do cursor no stream de acordo com o id fornecido. O endereço de um sprite pode ser lido nos limites entre 0 e a quantidade de sprites, no entanto, o endereço 0 representa um sprite vazio, sem nenhum pixel colorido e não precisa ser lido do arquivo.
    - Ler o endereço do sprite.
    - Definir a posição do cursor no stream de acordo com o endereço lido.

```JavaScript

function readSprite(id)
{
    // Checa se o id é válido
    if (id > spriteCount)
    {
        // -----< Deve lançar um erro ou retornar um sprite vazio >-----
    }
    
    // O id 0 representa um sprite vazio
    if (id == 0)
    {
        // -----< Retornar um sprite vazio >-----
    }
    
    // O endereço do sprite 1 é 0, então subtraimos 1 do id fornecido.
    // Cada endereço tem 4 bytes de comprimento, então multiplicamos por 4.
    // O primeiro endereço é lido logo após a leitura do 'head'.
    var position = ((id - 1) * 4) + headSize;
    
    // -----< Definir a posição do stream pela variável position >-----
    
    var address = // -----< ler 4 bytes do stream >-----
    
    if (address == 0)
    {
        // -----< Retornar um sprite vazio ??? >-----
    }
    
    // -----< Definir a posição do stream de pela variável address >-----

```

- Data
    - Ler a colorkey.
    - Ler o tamanho dos dados.
    - Ler os dados. Apenas píxeis coloridos são lidos. Os primeiros 2 bytes representam a quantidade de
      píxeis transparentes até o próximo pixel colorido. Os 2 bytes seguintes reprentam a quantidade de
      píxeis colororidos até o próximo pixel transparente. Em seguida, são lidos os 3 bytes da cor do pixel.

```JavaScript
    
    // Colorkey
    var red = // -----< ler 1 byte do stream >-----
    var green = // -----< ler 1 byte do stream >-----
    var blue = // -----< ler 1 byte do stream >-----
    
    var pixelDataSize = // -----< ler 2 bytes do stream >-----
    
    if (pixelDataSize == 0)
    {
        // -----< Um sprite sem dados ??? Retornar um sprite vazio >-----
    }
    
    var read = 0;
    var currentPixel = 0;
    
    while (read < pixelDataSize)
    {
        var transparentPixels = // -----< ler 2 bytes não assinados do stream >-----
        var coloredPixels = // -----< ler 2 bytes não assinados do stream >-----
         
        currentPixel += transparentPixels;
        
        // lê os píxeis coloridos
        for (var i = 0; i < coloredPixels; i++)
        {
            var red = // -----< ler 1 do stream >-----
            var green = // -----< ler 1 do stream >-----
            var blue = // -----< ler 1 do stream >-----
            
            currentPixel++;
        }
        
        // read += ( transparentPixels(u16) + coloredPixels(u16) ) + ( coloredPixels * ( red(u8) + green(u8) + blue(u8) ) )
        read += 4 + (coloredPixels * 3);
    }
}

```
