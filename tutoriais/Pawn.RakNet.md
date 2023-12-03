> # O que é o Pawn.RakNet?
[Pawn.RakNet](https://github.com/katursis/Pawn.RakNet) é um plugin desenvolvido para o SA-MP com o propósito de permitir que o servidor interaja com a biblioteca RakNet do samp-server. Ele oferece a capacidade de interceptar e enviar pacotes e [RPCs](RPC.md), possibilitando a manipulação desses dados. Com o Pawn.RakNet, o servidor tem a flexibilidade de ignorar determinados pacotes ou RPCs, ou tratá-los de maneira personalizada, proporcionando um maior controle sobre a comunicação entre o cliente e o servidor no ambiente SA-MP.


> ## O que é possivel fazer com isso?

Pelo fato de você conseguir interceptar e enviar os RPCs, você pode criar funções para um jogador que nativamente só servem de uso global (por exemplo, SetGravity), corrigir alguns bugs, entre outras coisas. Isso depende da imaginação (logicamente, só é possível realizar ações dentro dos limites do SA-MP, não é possível alterar coisas no cliente, como a posição do cursor ou verificar arquivos). Alguns exemplos de projetos públicos:

- [TogglePlayerInvisibility](https://github.com/Brunoo16/samp-tpi)
- [Player-Gravity](https://github.com/Romz24/samp-gravity)
- [getPlayerVehicleSeat-fix](https://github.com/tec-devv/getPlayerVehicleSeat-fix)
- [samp-weapon-config](https://github.com/oscar-broman/samp-weapon-config)

Pelo fato de você poder ignorar alguns pacotes, você pode até mesmo criar um anti-cheater poderoso que rejeita pacotes vindos de cheats. Por exemplo, ignorar o pacote UPDATE_WEAPONS(204) caso o jogador tenha uma arma não autorizada pelo servidor (weapon-hack), assim, outros jogadores não veriam o jogador com essa arma.

Você também pode, por exemplo, escrever por cima de pacotes. O jogador envia um pacote indicando que está com uma determinada arma, mas você sobrescreve essa informação, fazendo com que os outros jogadores vejam o jogador com uma arma diferente daquela que ele está usando.


## Tipos

O RakNet não sabe qual valores foram guardados no RPC, por tanto, voce deve enforma-lo ao ler ou escrever, por exemplo
```cpp
new valor;
BS_Read(bs, PR_UINT16, valor); //PR_UINT 8 equivale a uint16(unsigned short) em C++
```
Tipos abaixo:   
![Tipos imagem](https://github.com/El-SpaceX/tutorial-pawn-raknet/blob/main/assets/tipos.png?raw=true)


>## [Exemplo de uso](exemplos.md)


