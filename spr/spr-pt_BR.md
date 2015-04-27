Especificação do arquivo SPR
---

O Arquivo SPR contém os dados dos sprites. Um sprite representa uma imagem não transparente de 32x32 píxeis.
Apenas os píxeis coloridos são armazenados do arquivo. A cor magenta (#FF00FF) é usada para indicar os píxeis transparentes.

#### Estrutura do arquivo

| Bytes   | Comentário                                                                  |
|---------|:----------------------------------------------------------------------------|
| 4       | Assinatura do arquivo                                                       |
| 2 ou 4  | A quantidade de sprites no arquivo. 2 bytes para versões abaixo de 960      |
| 4       | Endereço do sprite 1                                                        |
| 4       | Endereço do sprite 2                                                        |
| 4       | Endereço do sprite 3                                                        |
| <b>.</b>| <b>.</b>                                                                    |
| <b>.</b>| <b>.</b>                                                                    |
| <b>.</b>| <b>.</b>                                                                    |
| 4       | Endereço do sprite N                                                        |
| 1       | Sprite 1 - colorkey canal red                                               |
| 1       | Sprite 1 - colorkey canal green                                             |
| 1       | Sprite 1 - colorkey canal blue                                              |
| 2       | Sprite 1 - tamanho dos dados                                                |
| N       | Sprite 1 - dados do sprite                                                  |
| 1       | Sprite 2 - colorkey canal red                                               |
| 1       | Sprite 2 - colorkey canal green                                             |
| 1       | Sprite 2 - colorkey canal blue                                              |
| 2       | Sprite 2 - tamanho dos dados                                                |
| N       | Sprite 3 - dados do sprite                                                  |
| 1       | Sprite 3 - colorkey canal red                                               |
| 1       | Sprite 3 - colorkey canal green                                             |
| 1       | Sprite 3 - colorkey canal blue                                              |
| 2       | Sprite 3 - tamanho dos dados                                                |
| N       | Sprite 3 - dados do sprite                                                  |
| <b>.</b>| <b>.</b>                                                                    |
| <b>.</b>| <b>.</b>                                                                    |
| <b>.</b>| <b>.</b>                                                                    |
| 1       | Sprite N - colorkey canal red                                               |
| 1       | Sprite N - colorkey canal green                                             |
| 1       | Sprite N - colorkey canal blue                                              |
| 2       | Sprite N - tamanho dos dados                                                |
| N       | Sprite N - dados do sprite                                                  |

### Leitura do arquivo

- Header
    - Ler a assinatura do arquivo.
    - Ler a quantidade de sprites.
    - Armazenar a posição do cursor no stream.

```JavaScript

var signature =  // -----< ler 4 bytes do stream >-----
var spriteCount = // -----< ler 2 bytes para versões anteriores à 960 ou 4 bytes para 960 ou maior >-----
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
    - Leitura da colorkey.
    - Leitura do tamanho dos dados.
    - Leitura dos dados.

```JavaScript
    
    // Colorkey
    var red = // -----< ler 1 byte do stream >-----
    var green = // -----< ler 1 byte do stream >-----
    var blue = // -----< ler 1 byte do stream >-----
    
    var pixelDataSize = // -----< ler 2 bytes do stream >-----
    
    if (pixelDataSize == 0)
    {
        // -----< Um sprite sem píxeis coloridos, retornar um sprite vazio >-----
    }
    
    // TODO descrição da leitura dos pixels
}

```
