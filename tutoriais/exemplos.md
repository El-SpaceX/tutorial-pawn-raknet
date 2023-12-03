> ## Escrevendo valores
```cpp

// Exemplo base tirado de https://github.com/Romz24/samp-gravity
SetPlayerGravity(playerid, Float:gravity)
{
    //verifica se o jogador esta conectado
	if(!IsPlayerConnected(playerid)) 
        return 0;

    // Cria um BitStream com a fun��o BS_New() -> Novo_Bistream, algo assim traduzido.
    // Criaremos um BitStream para escrever os valores nele antes de enviar ao player, nesse caso, a gravidade.
	new BitStream:bs = BS_New(); 

    // A fun��o BS_WriteValue, como o nome diz, escreve os valores no BitStream.
	// Os Argumentos sao: variavel contendo o bitstream e logo em seguida um padr�o se segue: tipo, valor, tipo, valor, tipo, valor, indo a quantidade de parametros do seu RPC.
    BS_WriteValue( 
		bs, //aqui passamos a variavel que criamos que recebe o BitStream de BS_New().
		PR_FLOAT, gravity //aqui sempre indicamos, TIPO, valor, nesse caso, float e o valor da gravidade.
	); 

    /* 
    BS_RPC envia o nosso RPC.
    Argumento *bs*: � nossa variavel que criamos para receber o bitstream.
    Argumento *playerid*: � o id do jogador que ira receber o RPC(se for usado -1, o RPC � enviado para todos jogadores conectados).
    146 - ID do RPC, que seria o RPC SetGravity(146).
    */
	BS_RPC(bs, playerid, 146); 

    // Deleta o RPC(libera a memoria alocada que salvava o RPC).
	BS_Delete(bs); 
	return 1;
}
```

Essa fun��o acima, seta a gravidade para somente 1 jogador, ao inves de todos.

> ## Lendo valores
```cpp
//Essa callback interceptar RPC's enviados pelo servidor.
public OnOutcomingRPC(playerid, rpcid, BitStream:bs)
{
	if(rpcid == 146)
	{
        new Float:gravidade;
		BS_ReadValue(bs, PR_FLOAT, gravidade); // L� a gravidade do tipo float e seta na variavel.
		BS_ResetReadPointer(bs); //R eseta o ponteiro de leitura do RPC.
	}
    return 1;
}
```
Outra forma � usando os macros como ORPC, IRPC, OPacket, IPacket, ...

Neste exemplo, interceptaremos pacotes enviados do cliente respons�vel pela anima��o, movimento, etc. (exemplo do pr�prio reposit�rio do Pawn.RakNet)
```cpp
const PLAYER_SYNC = 207; //Cria uma variavel contendo o ID do pacote.

IPacket:PLAYER_SYNC(playerid, BitStream:bs) // Usa o macro em vez de utilizar a callback, o que � bom para modularizar e tornar o c�digo mais leg�vel.

{
    new data[PR_OnFootSync]; // Cria uma array com a estrutura para receber o pacote onFootData (h� estruturas para todos os pacotes, juntamente com fun��es de leitura inclusas).

    BS_IgnoreBits(bs, 8); // Ignora os 8 primeiros bits do pacote que cont�m o ID do pacote (algo que j� sabemos qual �).

    BS_ReadOnFootSync(bs, onFootData); // L� o pacote. O Pawn.RakNet inclui fun��es para ler todos os pacotes de forma simples, retornando em um array os valores. Assim, voc� n�o precisa fazer manualmente o c�digo para ler.

    printf("health: %f", data[PR_health]); // Imprime a vida do jogador contida no pacote.

    return 1;
}
```

>> # Ignorando pacotes

```c
const PLAYER_SYNC = 207; //cria uma variavel contendo o ID do nosso pacote

IPacket:PLAYER_SYNC(playerid, BitStream:bs) //usa o macro ao inves de usar a callback, bom para modular e ficar mais legivel
{
    //Ao retornarmos 0, o Pawn.RakNet ir� ignorar esse pacote, fazendo com que n�o seja processado. Ou seja, � como se nada tivesse acontecido. Para os outros jogadores, o jogador continua na mesma posi��o, mesmo que ele esteja se movendo. Se usarmos GetPlayerPos, ir� retornar a posi��o do �ltimo pacote contendo essa informa��o processado.

    return 0;
}
```