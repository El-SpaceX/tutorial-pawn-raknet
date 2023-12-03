> # O que � o Pawn.RakNet?
[Pawn.RakNet](https://github.com/katursis/Pawn.RakNet) � um plugin desenvolvido para o SA-MP com o prop�sito de permitir que o servidor interaja com a biblioteca RakNet do samp-server. Ele oferece a capacidade de interceptar e enviar pacotes e [RPCs](RPC.md), possibilitando a manipula��o desses dados. Com o Pawn.RakNet, o servidor tem a flexibilidade de ignorar determinados pacotes ou RPCs, ou trat�-los de maneira personalizada, proporcionando um maior controle sobre a comunica��o entre o cliente e o servidor no ambiente SA-MP.


> ## O que � possivel fazer com isso?

Pelo fato de voc� conseguir interceptar e enviar os RPCs, voc� pode criar fun��es para um jogador que nativamente s� servem de uso global (por exemplo, SetGravity), corrigir alguns bugs, entre outras coisas. Isso depende da imagina��o (logicamente, s� � poss�vel realizar a��es dentro dos limites do SA-MP, n�o � poss�vel alterar coisas no cliente, como a posi��o do cursor ou verificar arquivos). Alguns exemplos de projetos p�blicos:

- [TogglePlayerInvisibility](https://github.com/Brunoo16/samp-tpi)
- [Player-Gravity](https://github.com/Romz24/samp-gravity)
- [getPlayerVehicleSeat-fix](https://github.com/tec-devv/getPlayerVehicleSeat-fix)
- [samp-weapon-config](https://github.com/oscar-broman/samp-weapon-config)

Pelo fato de voc� poder ignorar alguns pacotes, voc� pode at� mesmo criar um anti-cheater poderoso que rejeita pacotes vindos de cheats. Por exemplo, ignorar o pacote UPDATE_WEAPONS(204) caso o jogador tenha uma arma n�o autorizada pelo servidor (weapon-hack), assim, outros jogadores n�o veriam o jogador com essa arma.

Voc� tamb�m pode, por exemplo, escrever por cima de pacotes. O jogador envia um pacote indicando que est� com uma determinada arma, mas voc� sobrescreve essa informa��o, fazendo com que os outros jogadores vejam o jogador com uma arma diferente daquela que ele est� usando.


## Tipos

O RakNet n�o sabe qual valores foram guardados no RPC, por tanto, voce deve enforma-lo ao ler ou escrever, por exemplo
```cpp
new valor;
BS_Read(bs, PR_UINT16, valor); //PR_UINT 8 equivale a uint16(unsigned short) em C++
```
Tipos abaixo:   
![Tipos imagem](https://github.com/El-SpaceX/tutorial-pawn-raknet/blob/main/assets/tipos.png?raw=true)


>## [Exemplo de uso](exemplos.md)


