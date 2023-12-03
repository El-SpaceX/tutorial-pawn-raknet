> # Creditos(leia isso)

Isso aqui foi literalmente pego e traduzido do antigo forum, pois explica de uma forma extremamente boa com exemplos e tudo, obrigado bruno.    
- **Creditos**: Brunoo  
- **Github**: [Brunoo16](https://github.com/Brunoo16) 
- **Post original**: [clique aqui](https://sampforum.blast.hk/showthread.php?tid=644857) 


> # Introdu��o
Vou explicar o que s�o RPCs (Remote Procedure Calls) e fornecer seus par�metros e IDs para uso, incluindo alguns exemplos no final do t�pico.

Algumas RPCs t�m os mesmos par�metros da fun��o � qual est�o geralmente relacionadas, enquanto outras n�o, tornando seu uso complicado.

> # O que significa RPC?
RPC � uma sigla para Remote Procedure Calls(Chamada de Procedimento Remoto), um protocolo usado para solicitar um servi�o de um cliente localizado em outro computador em uma rede, sem precisar entender os detalhes da rede. Como voc� j� sabe, SA-MP utiliza a engine de rede RakNet.

Por exemplo, se voc� explodir um jogador, o servidor enviar� uma RPC (0x47 - RPC_CreateExplosion) para o cliente, solicitando a cria��o da explos�o.

O cliente tamb�m envia chamadas de procedimento remoto de volta. Suponha que voc� entre em um ve�culo; seu cliente enviar� uma RPC de entrada de ve�culo para o servidor (RPC_EnterVehicle).

> # Tipos de RPC
- **RPCs de Sa�da:** RPCs enviadas pelo servidor para o cliente.
- **RPCs de Entrada:** RPCs enviadas pelo cliente para o servidor.

> # Prioridades
- **SYSTEM_PRIORITY:** Interno. Usado pelo RakNet para enviar mensagens de alta prioridade acima das mensagens de alta prioridade.
- **HIGH_PRIORITY:** Mensagens de alta prioridade s�o enviadas antes das mensagens de m�dia prioridade.
- **MEDIUM_PRIORITY:** Mensagens de m�dia prioridade s�o enviadas antes das mensagens de baixa prioridade.
- **LOW_PRIORITY:** Mensagens de baixa prioridade s�o enviadas apenas quando nenhuma outra mensagem est� aguardando.

> # Confiabilidades
- **RELIABLE_SEQUENCED:** Pacotes confi�veis sequenciados s�o pacotes UDP monitorados por uma camada de confiabilidade para garantir que cheguem ao destino e estejam sequenciados no destino.
  - **Vantagens:** Confian�a dos pacotes UDP, sequenciamento dos pacotes ordenados, sem a necessidade de esperar por pacotes antigos.
  - **Desvantagens:** Desperd�cio de largura de banda.

- **RELIABLE_ORDERED:** Pacotes confi�veis ordenados s�o pacotes UDP monitorados por uma camada de confiabilidade para garantir que cheguem ao destino e estejam ordenados no destino.
  - **Vantagens:** O pacote chegar� l� e na ordem em que foi enviado.
  - **Desvantagens:** Pode exigir largura de banda significativa devido a retransmiss�es e acks. Os pacotes podem chegar muito tarde se a rede estiver congestionada.

- **RELIABLE:** Pacotes confi�veis s�o pacotes UDP monitorados por uma camada de confiabilidade para garantir que cheguem ao destino.
  - **Vantagens:** Voc� sabe que o pacote chegar�. Eventualmente...
  - **Desvantagens:** Pode exigir largura de banda significativa devido a retransmiss�es e acks.

- **UNRELIABLE_SEQUENCED:** Pacotes n�o confi�veis sequenciados s�o iguais aos pacotes n�o confi�veis, exceto que apenas o pacote mais recente � aceito.
  - **Vantagens:** Mesma sobrecarga baixa dos pacotes n�o confi�veis, sem preocupa��o com pacotes mais antigos alterando seus dados para valores antigos.
  - **Desvantagens:** Muitos pacotes ser�o descartados.

- **UNRELIABLE:** Pacotes n�o confi�veis s�o enviados diretamente por UDP. Podem chegar fora de ordem ou nem chegar.
  - **Vantagens:** Esses pacotes n�o precisam ser reconhecidos pela rede, economizando o tamanho de um cabe�alho UDP na confirma��o.
  - **Desvantagens:** Sem ordem de pacotes, pacotes podem nunca chegar.

>#  Exemplos
### RPC_ShowActor
**ID:** 171
**Par�metros:**
- WORD wActorID
- DWORD dSkinID
- float x, float y, float z
- float angle
- float health

### Exemplo com Plugin(C++)
```cpp
RakNet::BitStream bs;
bs.Write((WORD)5); // ID do ator.
bs.Write((DWORD)287); // ID da skin do ator.
bs.Write((float)0.0); // X.
bs.Write((float)0.0); // Y.
bs.Write((float)0.0); // Z.
bs.Write((float)50.0); // �ngulo.
bs.Write((float)100.0); // Sa�de do ator.
pRakServer->RPC(&RPC_ShowActor, &bs, LOW_PRIORITY, RELIABLE_ORDERED, 0, pRakServer->GetPlayerIDFromIndex(23), 0, 0);
```

- **&RPC_ShowActor:** Inteiro com o ID do RPC.  
- **&bs:** Cont�m os dados escritos acima (bits empacotados).  
- **LOW_PRIORITY:** Prioridade do RPC.  
- **RELIABLE_ORDERED:** Confiabilidade do RPC.  
- **pRakServer->GetPlayerIDFromIndex(23):** O jogador que est� prestes a receber a RPC.  

### Exemplo com Pawn.RakNet

```cpp
new BitStream:bs = BS_New();
BS_WriteValue(
    bs,
    PR_UINT16, 5, // ID do ator.
    PR_UINT32, 287, // ID da skin do ator.
    PR_FLOAT, 0.0, // X.
    PR_FLOAT, 0.0, // Y.
    PR_FLOAT, 0.0, // Z.
    PR_FLOAT, 50.0, // �ngulo.
    PR_FLOAT, 100.0 // Sa�de do ator.
);
BS_RPC(bs, 23, 171, PR_LOW_PRIORITY, PR_RELIABLE_ORDERED);
BS_Delete(bs);
```

- **bs:** BitStream com os dados acima (bits empacotados).
- **23:** O id do jogador que est� prestes a receber a RPC.
- **171:** ID do RPC_ShowActor.
- **PR_LOW_PRIORITY:** Prioridade do RPC.
- **PR_RELIABLE_ORDERED:** Confiabilidade do RPC.

# [Lista RPC com par�metros](https://github.com/Brunoo16/samp-packet-list/wiki)