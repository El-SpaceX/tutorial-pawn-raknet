> # Creditos(leia isso)

Isso aqui foi literalmente pego e traduzido do antigo forum, pois explica de uma forma extremamente boa com exemplos e tudo, obrigado bruno.    
- **Creditos**: Brunoo  
- **Github**: [Brunoo16](https://github.com/Brunoo16) 
- **Post original**: [clique aqui](https://sampforum.blast.hk/showthread.php?tid=644857) 


> # Introdução
Vou explicar o que são RPCs (Remote Procedure Calls) e fornecer seus parâmetros e IDs para uso, incluindo alguns exemplos no final do tópico.

Algumas RPCs têm os mesmos parâmetros da função à qual estão geralmente relacionadas, enquanto outras não, tornando seu uso complicado.

> # O que significa RPC?
RPC é uma sigla para Remote Procedure Calls(Chamada de Procedimento Remoto), um protocolo usado para solicitar um serviço de um cliente localizado em outro computador em uma rede, sem precisar entender os detalhes da rede. Como você já sabe, SA-MP utiliza a engine de rede RakNet.

Por exemplo, se você explodir um jogador, o servidor enviará uma RPC (0x47 - RPC_CreateExplosion) para o cliente, solicitando a criação da explosão.

O cliente também envia chamadas de procedimento remoto de volta. Suponha que você entre em um veículo; seu cliente enviará uma RPC de entrada de veículo para o servidor (RPC_EnterVehicle).

> # Tipos de RPC
- **RPCs de Saída:** RPCs enviadas pelo servidor para o cliente.
- **RPCs de Entrada:** RPCs enviadas pelo cliente para o servidor.

> # Prioridades
- **SYSTEM_PRIORITY:** Interno. Usado pelo RakNet para enviar mensagens de alta prioridade acima das mensagens de alta prioridade.
- **HIGH_PRIORITY:** Mensagens de alta prioridade são enviadas antes das mensagens de média prioridade.
- **MEDIUM_PRIORITY:** Mensagens de média prioridade são enviadas antes das mensagens de baixa prioridade.
- **LOW_PRIORITY:** Mensagens de baixa prioridade são enviadas apenas quando nenhuma outra mensagem está aguardando.

> # Confiabilidades
- **RELIABLE_SEQUENCED:** Pacotes confiáveis sequenciados são pacotes UDP monitorados por uma camada de confiabilidade para garantir que cheguem ao destino e estejam sequenciados no destino.
  - **Vantagens:** Confiança dos pacotes UDP, sequenciamento dos pacotes ordenados, sem a necessidade de esperar por pacotes antigos.
  - **Desvantagens:** Desperdício de largura de banda.

- **RELIABLE_ORDERED:** Pacotes confiáveis ordenados são pacotes UDP monitorados por uma camada de confiabilidade para garantir que cheguem ao destino e estejam ordenados no destino.
  - **Vantagens:** O pacote chegará lá e na ordem em que foi enviado.
  - **Desvantagens:** Pode exigir largura de banda significativa devido a retransmissões e acks. Os pacotes podem chegar muito tarde se a rede estiver congestionada.

- **RELIABLE:** Pacotes confiáveis são pacotes UDP monitorados por uma camada de confiabilidade para garantir que cheguem ao destino.
  - **Vantagens:** Você sabe que o pacote chegará. Eventualmente...
  - **Desvantagens:** Pode exigir largura de banda significativa devido a retransmissões e acks.

- **UNRELIABLE_SEQUENCED:** Pacotes não confiáveis sequenciados são iguais aos pacotes não confiáveis, exceto que apenas o pacote mais recente é aceito.
  - **Vantagens:** Mesma sobrecarga baixa dos pacotes não confiáveis, sem preocupação com pacotes mais antigos alterando seus dados para valores antigos.
  - **Desvantagens:** Muitos pacotes serão descartados.

- **UNRELIABLE:** Pacotes não confiáveis são enviados diretamente por UDP. Podem chegar fora de ordem ou nem chegar.
  - **Vantagens:** Esses pacotes não precisam ser reconhecidos pela rede, economizando o tamanho de um cabeçalho UDP na confirmação.
  - **Desvantagens:** Sem ordem de pacotes, pacotes podem nunca chegar.

>#  Exemplos
### RPC_ShowActor
**ID:** 171
**Parâmetros:**
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
bs.Write((float)50.0); // Ângulo.
bs.Write((float)100.0); // Saúde do ator.
pRakServer->RPC(&RPC_ShowActor, &bs, LOW_PRIORITY, RELIABLE_ORDERED, 0, pRakServer->GetPlayerIDFromIndex(23), 0, 0);
```

- **&RPC_ShowActor:** Inteiro com o ID do RPC.  
- **&bs:** Contém os dados escritos acima (bits empacotados).  
- **LOW_PRIORITY:** Prioridade do RPC.  
- **RELIABLE_ORDERED:** Confiabilidade do RPC.  
- **pRakServer->GetPlayerIDFromIndex(23):** O jogador que está prestes a receber a RPC.  

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
    PR_FLOAT, 50.0, // Ângulo.
    PR_FLOAT, 100.0 // Saúde do ator.
);
BS_RPC(bs, 23, 171, PR_LOW_PRIORITY, PR_RELIABLE_ORDERED);
BS_Delete(bs);
```

- **bs:** BitStream com os dados acima (bits empacotados).
- **23:** O id do jogador que está prestes a receber a RPC.
- **171:** ID do RPC_ShowActor.
- **PR_LOW_PRIORITY:** Prioridade do RPC.
- **PR_RELIABLE_ORDERED:** Confiabilidade do RPC.

# [Lista RPC com parâmetros](https://github.com/Brunoo16/samp-packet-list/wiki)