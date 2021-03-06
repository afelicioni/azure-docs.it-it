---
title: "Pianificazione della capacità del cluster di Service Fabric | Documentazione Microsoft"
description: "Considerazioni sulla pianificazione della capacità del cluster Service Fabric. Nodetypes, livelli di affidabilità e durata"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/05/2017
ms.author: chackdan
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: c38b337d67b518f68be6dc8255fa97b78ba89dae
ms.contentlocale: it-it
ms.lasthandoff: 05/09/2017


---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Considerazioni sulla pianificazione della capacità del cluster Service Fabric
La pianificazione della capacità è un passaggio importante per qualsiasi distribuzione di produzione. Ecco alcuni aspetti da considerare nell'ambito di tale processo.

* Numero di tipi di nodo con cui il cluster deve iniziare
* Proprietà di ciascun tipo di nodo (dimensione, primario, per Internet, numero di VM e così via)
* Caratteristiche di affidabilità e durabilità del cluster

Ogni aspetto verrà ora esaminato brevemente.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Numero di tipi di nodo con cui il cluster deve iniziare
Prima di tutto è necessario stabilire per che cosa verrà usato il cluster che si sta creando e quali tipologie di applicazioni si intende distribuire nel cluster. Se lo scopo del cluster non è chiaro, è molto probabile che non si sia ancora pronti per iniziare il processo di pianificazione della capacità.

Stabilire il numero di tipi di nodo con cui il cluster deve iniziare.  Di ogni tipo di nodo viene eseguito il mapping a un set di scalabilità di macchine virtuali. Ogni tipo di nodo può quindi essere aumentato o ridotto in modo indipendente, avere diversi set di porte aperte e avere metriche per la capacità diverse. Quindi la decisione relativa al numero di tipi di nodo si basa essenzialmente sulle considerazioni seguenti:

* L'applicazione ha più servizi, alcuni dei quali devono essere pubblici o per Internet? Le applicazioni tipiche contengono un servizio gateway front-end che riceve l'input da un client e uno o più servizi back-end che comunicano con i servizi front-end. In questo caso quindi si finirà per avere almeno due tipi di nodi.
* I servizi (che costituiscono l'applicazione) hanno esigenze diverse per l'infrastruttura, ad esempio cicli di CPU più elevati o una quantità maggiore di RAM? Ad esempio, si supponga che l'applicazione che si vuole distribuire contenga un servizio front-end e un servizio back-end. Il servizio front-end può essere eseguito in VM più piccole (ad esempio, di dimensioni D2), con porte aperte a Internet,  Il servizio back-end è invece a elevato utilizzo di calcolo e deve essere eseguito in VM più grandi (ad esempio di dimensione D4, D6, D15) senza connessione a Internet.
  
  In questo esempio, anche se è possibile decidere di inserire tutti i servizi in un unico tipo di nodo, è consigliabile inserirli in un cluster con due tipi di nodo,  perché in questo modo ogni tipo di nodo può avere proprietà distinte, ad esempio la connettività Internet o le dimensioni delle VM. Il numero di VM può essere ridimensionato anche in modo indipendente.  
* Poiché non è possibile prevedere cosa accadrà in futuro, è importante basarsi sulle condizioni attuali per decidere il numero di tipi di nodo necessari alle applicazioni per iniziare. Sarà sempre possibile aggiungere o rimuovere i tipi di nodi in seguito. Un cluster di Service Fabric deve avere almeno un tipo di nodo.

## <a name="the-properties-of-each-node-type"></a>Proprietà di ogni tipo di nodo
I **tipi di nodo** possono essere paragonati ai ruoli in Servizi cloud, poiché definiscono le dimensioni delle VM, il numero di VM e le relative proprietà. Ogni tipo di nodo definito in un cluster di Service Fabric viene configurato come set di scalabilità di macchine virtuali distinto. Un set di scalabilità di macchine virtuali è una risorsa di calcolo di Azure che è possibile usare per distribuire e gestire una raccolta di macchine virtuali come set. Essendo definito come set di scalabilità di macchine virtuali distinto, ogni tipo di nodo può essere aumentato o ridotto in modo indipendente, avere diversi set di porte aperte e avere metriche per la capacità diverse.

Vedere [questo documento](service-fabric-cluster-nodetypes.md) per altri dettagli sulla relazione dei tipi di nodo con il set di scalabilità di macchine virtuali, su come connettersi a una delle istanze tramite RDP, aprire nuove porte e così via.

Il cluster può avere più di un tipo di nodo, ma il tipo di nodo primario, ovvero il primo che si definisce nel portale, deve essere costituito da almeno cinque macchine virtuali per i cluster usati per i carichi di lavoro di produzione (o da almeno tre VM per i cluster di test). Se si sta creando il cluster con un modello di Resource Manager, nella definizione del tipo di nodo sarà presente un attributo **Primario**. Nel tipo di nodo primario vengono inseriti i servizi di sistema di Service Fabric.  

### <a name="primary-node-type"></a>Tipo di nodo primario
Nel caso di un cluster con più tipi di nodo, è necessario sceglierne uno come primario. Ecco le caratteristiche di un tipo di nodo primario:

* Le **dimensioni minime delle macchine virtuali** per il tipo di nodo primario sono determinate dal **livello di durabilità** scelto. Il valore predefinito per il livello di durabilità è Bronze. Per sapere che cos'è il livello di durabilità e su quali valori può essere impostato, scorrere verso il basso.  
* Il **numero minimo delle macchine virtuali** per il tipo di nodo primario è determinato dal **livello di affidabilità** scelto. Il valore predefinito per il livello di affidabilità è Silver. Per sapere che cos'è il livello di affidabilità e su quali valori può essere impostato, scorrere verso il basso. 

 

* I servizi di sistema di Service Fabric (ad esempio, il servizio Cluster Manager o il servizio Image Store) vengono inseriti nel tipo di nodo primario e quindi l'affidabilità e la durabilità del cluster vengono determinate dal valore del livello di affidabilità e dal valore del livello di durabilità selezionato per il tipo di nodo primario.

![Screenshot di un cluster con due tipi di nodo ][SystemServices]

### <a name="non-primary-node-type"></a>Tipo di nodo non primario
Per un cluster con più tipi di nodo, è presente un tipo di nodo primario, mentre gli altri saranno non primari. Ecco le caratteristiche di un tipo di nodo non primario:

* Le dimensioni minime delle macchine virtuali per questo tipo di nodo sono determinate dal livello di durabilità scelto. Il valore predefinito per il livello di durabilità è Bronze. Per sapere che cos'è il livello di durabilità e su quali valori può essere impostato, scorrere verso il basso.  
* Il numero minimo di VM per questo tipo di nodo può essere uno. È tuttavia consigliabile scegliere questo numero in base al numero di repliche dell'applicazione o dei servizi che si vuole eseguire in questo tipo di nodo. Il numero di VM in un tipo di nodo può essere aumentato dopo avere distribuito il cluster.

## <a name="the-durability-characteristics-of-the-cluster"></a>Caratteristiche di durabilità del cluster
Il livello di durabilità viene usato per indicare al sistema i privilegi delle VM rispetto all'infrastruttura di Azure sottostante. Nel tipo di nodo primario questo privilegio consente a Service Fabric di sospendere le richieste di infrastruttura a livello di VM (ad esempio il riavvio di una VM, il re-imaging di una VM o la migrazione di una VM) che hanno effetto sui requisiti relativi al quorum per i servizi di sistema e i servizi con stato. Nei tipi di nodo non primari questo privilegio consente a Service Fabric di sospendere le richieste di infrastruttura a livello di macchina virtuale (ad esempio il riavvio di una VM, il re-imaging di una VM, la migrazione di una VM e così via) che hanno effetto sui requisiti relativi al quorum per i servizi con stato in esecuzione.

Questo privilegio viene espresso con i valori seguenti:

* Gold: i processi dell'infrastruttura possono essere sospesi per una durata di 2 ore per ogni dominio di aggiornamento. La durata Gold può essere abilitata solo per gli SKU VM con tutti i nodi come D15_V2, G5 e così via.
* Silver: i processi dell'infrastruttura possono essere sospesi per una durata di 30 minuti per ogni dominio di aggiornamento. Attualmente non è abilitato per l'uso. Una volta abilitato, sarà disponibile in tutte le VM standard con una sola memoria centrale o più.
* Bronze: nessun privilegio. Questa è la modalità predefinita.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Caratteristiche di affidabilità del cluster
Il livello di affidabilità viene usato per impostare il numero di repliche dei servizi di sistema che si vuole eseguire in questo cluster nel tipo di nodo primario. I servizi di sistema nel cluster sono tanto più affidabili quanto più elevato è il numero di repliche.  

Il livello di affidabilità può avere i valori seguenti:

* Platinum: esegue i servizi di sistema con un totale di set di repliche di destinazione pari a 9
* Gold: esegue i servizi di sistema con un totale di set di repliche di destinazione pari a 7
* Silver: esegue i servizi di sistema con un totale di set di repliche di destinazione pari a 5
* Bronze: esegue i servizi di sistema con un totale di set di repliche di destinazione pari a 3

> [!NOTE]
> Il livello di affidabilità scelto determina il numero minimo di nodi che deve avere il tipo di nodo primario. Il livello di affidabilità non ha effetto sulla dimensione massima del cluster. È possibile, ad esempio, avere un cluster a 20 nodi, in esecuzione con affidabilità Bronze.
> 
> 

 È possibile scegliere di aggiornare l'affidabilità del cluster passando da un livello a un altro. Così facendo si attivano gli aggiornamenti del cluster necessari per modificare il totale di set di repliche dei servizi di sistema. Attendere che l'aggiornamento in corso venga completato prima di apportare altre modifiche al cluster, come l'aggiunta di nodi.  È possibile monitorare lo stato di avanzamento dell'aggiornamento in Service Fabric Explorer oppure eseguendo [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps).


## <a name="primary-node-type---capacity-guidance"></a>Tipo di nodo primario: guida alla capacità

Ecco le indicazioni per pianificare la capacità del tipo di nodo primario

1. **Numero di istanze delle VM per eseguire qualsiasi carico di lavoro di produzione in Azure: ** è necessario specificare una dimensione minima del tipo di nodo primario pari a 5.
2. **Numero di istanze delle VM per eseguire carichi di lavoro di test in Azure:** è possibile specificare una dimensione minima del tipo di nodo primario pari a 1 o 3. Il cluster a un nodo viene eseguito con una configurazione speciale, pertanto la scalabilità orizzontale di tale cluster non è supportata. Il cluster a un nodo non ha alcun livello di affidabilità, di conseguenza nel modello di Resource Manager è necessario rimuovere/non specificare tale configurazione (non è sufficiente non impostare il valore di configurazione). Se si configura il cluster a un nodo tramite il portale, la configurazione viene gestita automaticamente. I cluster a uno e tre nodi non sono supportati per l'esecuzione di carichi di lavoro di produzione. 
3. **SKU di VM:** nel tipo di nodo primario vengono eseguiti i servizi di sistema, quindi lo SKU della VM scelto deve tenere in considerazione il carico massimo totale che si prevede di inserire nel cluster. Per comprendere questo concetto, si pensi all'analogia seguente: il tipo di nodo primario è come i polmoni che forniscono ossigeno al cervello e quindi, se il cervello non riceve abbastanza ossigeno, il corpo ne risente. 

Poiché le esigenze in termini di capacità di un cluster dipendono dal carico di lavoro che si prevede di eseguire nel cluster, non è possibile offrire una guida valida per un carico di lavoro specifico, ma ecco alcune indicazioni generali per poter iniziare.

Per i carichi di lavoro di produzione 


- Lo SKU per le VM consigliato è Standard D3 o Standard D3_V2 o equivalente con un'unità SSD locale di almeno 14 GB.
- La versione minima supportata dello SKU per le VM è Standard D1 o Standard D1_V2 o equivalente con un'unità SSD locale di almeno 14 GB. 
- Gli SKU per VM con core parziali, ad esempio Standard A0, non sono supportati per i carichi di lavoro di produzione.
- Lo SKU Standard A1 non è supportato per i carichi di lavoro di produzione per motivi di prestazioni.


## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Tipo di nodo non primario: guida alla capacità per carichi di lavoro con stato

Vedere quanto segue per i carichi di lavoro che usano Reliable Collections o Reliable Actors di Service Fabric. Vedere qui per altre informazioni sui [modelli di programmazione](service-fabric-choose-framework.md).

1. **Numero di istanze delle VM:** è consigliabile eseguire i carichi di lavoro di produzione con stato con un numero di repliche di destinazione di almeno 5 istanze. Nello stato stabile viene quindi creata una replica (da un set di repliche) in ogni dominio di errore e in ogni dominio di aggiornamento. L'intero concetto di livello di affidabilità per il tipo di nodo primario è un modo per specificare questa impostazione per i servizi di sistema.

Per i carichi di lavoro di produzione, la dimensione minima consigliata per il tipo di nodo non primario è quindi 5, se si eseguono carichi di lavoro con stato.

2. **SKU di VM:** in questo tipo di nodo sono in esecuzione i servizi dell'applicazione, quindi lo SKU per le VM scelto deve tenere in considerazione il carico massimo che si prevede di inserire in ogni nodo. Poiché le esigenze in termini di capacità del tipo di nodo dipendono dal carico di lavoro che si prevede di eseguire nel cluster, non è possibile offrire una guida valida per un carico di lavoro specifico, ma ecco alcune indicazioni generali per poter iniziare.

Per i carichi di lavoro di produzione 

- Lo SKU per le VM consigliato è Standard D3 o Standard D3_V2 o equivalente con un'unità SSD locale di almeno 14 GB.
- La versione minima supportata dello SKU per le VM è Standard D1 o Standard D1_V2 o equivalente con un'unità SSD locale di almeno 14 GB. 
- Gli SKU per VM con core parziali, ad esempio Standard A0, non sono supportati per i carichi di lavoro di produzione.
- Lo SKU Standard A1 in particolare non è supportato per i carichi di lavoro di produzione per motivi di prestazioni.


## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Tipo di nodo non primario: guida alla capacità per carichi di lavoro senza stato

Vedere quanto segue per i carichi di lavoro senza stato

**Numero di istanze delle VM:** per i carichi di lavoro di produzione senza stato, la dimensione minima supportata per il tipo di nodo non primario è pari a 2. Ciò consente di eseguire due istanze senza stato dell'applicazione e consente al servizio di resistere alla perdita di un'istanza di una VM. 

> [!NOTE]
> Se il cluster è in esecuzione in una versione di Service Fabric precedente alla 5.6, a causa di un difetto del runtime (corretto nella versione 5.6), passando a un tipo di nodo non primario inferiore a 5, l'integrità del cluster viene meno, finché non si chiama il [comando Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/servicefabric/vlatest/Remove-ServiceFabricNodeState) con il nome di nodo appropriato. Per altre informazioni, vedere [Aumentare o ridurre il numero di istanze del cluster di Service Fabric](service-fabric-cluster-scale-up-down.md).
> 
>

**SKU di VM:** in questo tipo di nodo sono in esecuzione i servizi dell'applicazione, quindi lo SKU per le VM scelto deve tenere in considerazione il carico massimo che si prevede di inserire in ogni nodo. Poiché le esigenze in termini di capacità del tipo di nodo dipendono completamente dal carico di lavoro che si prevede di eseguire nel cluster, non è possibile offrire una guida valida per un carico di lavoro specifico, ma ecco alcune indicazioni generali per poter iniziare.

Per i carichi di lavoro di produzione 


- Lo SKU per le VM consigliato è Standard D3 o Standard D3_V2 o equivalente. 
- La versione minima supportata dello SKU per le VM è Standard D1 o Standard D1_V2 o equivalente. 
- Gli SKU per VM con core parziali, ad esempio Standard A0, non sono supportati per i carichi di lavoro di produzione.
- Lo SKU Standard A1 in particolare non è supportato per i carichi di lavoro di produzione per motivi di prestazioni.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Passaggi successivi
Una volta completata la pianificazione della capacità e configurato un cluster, leggere quanto segue:

* [Sicurezza di un cluster di Service Fabric](service-fabric-cluster-security.md)
* [Introduzione al monitoraggio dell'integrità di Service Fabric](service-fabric-health-introduction.md)
* [Relazione tra i tipi di nodo di Service Fabric e i set di scalabilità di macchine virtuali](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

