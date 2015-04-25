Especificação do arquivo SPR
---

O Arquivo SPR contém os dados dos sprites.

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
