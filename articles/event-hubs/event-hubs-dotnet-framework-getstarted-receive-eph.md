---
title: Ricevere eventi da Hub eventi di Azure usando .NET Framework | Documentazione Microsoft
description: Seguire questa esercitazione per ricevere eventi da Hub eventi di Azure usando .NET Framework.
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 6c309a14e00324a9335bde61fe175ec3906c066d
ms.contentlocale: it-it
ms.lasthandoff: 04/18/2017


---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a>Ricevere eventi da Hub eventi di Azure usando .NET Framework

## <a name="introduction"></a>Introduzione
Hub eventi è un servizio che consente di elaborare grandi quantità di dati di telemetria sugli eventi da applicazioni e dispositivi connessi. Dopo aver raccolto i dati in Hub eventi, è possibile archiviarli usando un cluster di archiviazione o trasformarli usando un provider di analisi in tempo reale. Questa funzionalità di elaborazione e raccolta di eventi su vasta scala rappresenta un componente chiave delle moderne architetture di applicazioni, tra cui Internet delle cose (IoT).

Questa esercitazione illustra come scrivere un'applicazione console .NET Framework che riceve messaggi da un hub eventi usando l'**[host processore di eventi][EventProcessorHost]**. Per inviare eventi usando .NET Framework, vedere [Inviare eventi a Hub eventi di Azure usando .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) oppure fare clic sul linguaggio di invio appropriato nel sommario a sinistra.

L'[host processore di eventi][EventProcessorHost] è una classe .NET che semplifica la ricezione di eventi dagli hub eventi gestendo checkpoint persistenti e ricezioni parallele da tali hub. Usando l'[host processore di eventi][Event Processor Host] è possibile suddividere gli eventi su più ricevitori, anche se ospitati in nodi diversi. Questo esempio illustra come usare l'[host processore di eventi][EventProcessorHost] per un ricevitore singolo. L'esempio di [elaborazione di eventi con aumento del numero di istanze][Scale out Event Processing with Event Hubs] illustra come usare l'[host processore di eventi][EventProcessorHost] con più ricevitori.

## <a name="prerequisites"></a>Prerequisiti

Per completare l'esercitazione sono necessari gli elementi seguenti:

* [Microsoft Visual Studio 2015 o versione successiva](http://visualstudio.com). Gli screenshot in questa esercitazione illustrano Visual Studio 2017.
* Un account Azure attivo. Se non si ha un account, è possibile crearne uno gratuito in pochi minuti. Per informazioni dettagliate, vedere la pagina relativa alla [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Creare uno spazio dei nomi di Hub eventi e un hub eventi

Il primo passaggio consiste nell'usare il [portale di Azure](https://portal.azure.com) per creare uno spazio dei nomi di tipo Hub eventi e ottenere le credenziali di gestione necessarie all'applicazione per comunicare con l'hub eventi. Per creare uno spazio dei nomi e un hub eventi, seguire la procedura descritta in [questo articolo](event-hubs-create.md) e quindi procedere con i passaggi seguenti.

## <a name="create-an-azure-storage-account"></a>Creare un account di Archiviazione di Azure

Per usare l'[host processore di eventi][EventProcessorHost] è necessario un [account di archiviazione di Azure][Azure Storage account]:

1. Accedere al [portale di Azure][Azure portal] e fare clic su **Nuovo** nella parte superiore sinistra della schermata.
2. Fare clic su **Archiviazione** e quindi su **Account di archiviazione**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. Nel pannello **Crea account di archiviazione** digitare un nome per l'account di archiviazione. Scegliere una sottoscrizione, un gruppo di risorse e una località di Azure in cui creare la risorsa. Fare quindi clic su **Crea**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. Nell'elenco degli account di archiviazione fare clic su quello appena creato.
5. Nel pannello Account di archiviazione fare clic su **Chiavi di accesso**. Copiare il valore di **key1** da usare più avanti in questa esercitazione.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)
6. In Visual Studio creare un nuovo progetto di app desktop di Visual C# usando il modello di progetto **Applicazione console**. Assegnare al progetto il nome **Receiver**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
7. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **Receiver**, quindi scegliere **Gestisci pacchetti NuGet per la soluzione**.
8. Fare clic sulla scheda **Sfoglia** e quindi cercare `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Fare clic su **Installa**e accettare le condizioni per l'utilizzo.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Visual Studio scarica e installa il [pacchetto NuGet Azure Service Bus Event Hub - EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)e aggiunge un riferimento al pacchetto con tutte le relative dipendenze.
9. Fare clic con il pulsante destro del mouse sul progetto **Receiver**, scegliere **Aggiungi** e quindi **Classe**. Assegnare alla nuova classe il nome **SimpleEventProcessor** e quindi fare clic su **Aggiungi** per crearla.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
10. Aggiungere le istruzioni seguenti all'inizio del file SimpleEventProcessor.cs:
    
     ```csharp
     using Microsoft.ServiceBus.Messaging;
     using System.Diagnostics;
     ```
    
     Sostituire quindi il corpo della classe con il codice seguente:
    
     ```csharp
     class SimpleEventProcessor : IEventProcessor
     {
         Stopwatch checkpointStopWatch;
    
         async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
         {
             Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
             if (reason == CloseReason.Shutdown)
             {
                 await context.CheckpointAsync();
             }
         }
    
         Task IEventProcessor.OpenAsync(PartitionContext context)
         {
             Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
             this.checkpointStopWatch = new Stopwatch();
             this.checkpointStopWatch.Start();
             return Task.FromResult<object>(null);
         }
    
         async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
         {
             foreach (EventData eventData in messages)
             {
                 string data = Encoding.UTF8.GetString(eventData.GetBytes());
    
                 Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                     context.Lease.PartitionId, data));
             }
    
             //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
             if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
             {
                 await context.CheckpointAsync();
                 this.checkpointStopWatch.Restart();
             }
         }
     }
     ```
    
     Questa classe verrà chiamata da **EventProcessorHost** per elaborare gli eventi ricevuti dall'hub eventi. Si noti che la classe `SimpleEventProcessor` usa un cronometro per chiamare periodicamente il metodo checkpoint sul contesto di **EventProcessorHost** . Questo assicura che, se il ricevitore viene riavviato, non perderà più di cinque minuti di lavoro di elaborazione.
11. Nella classe **Program** aggiungere l'istruzione `using` seguente all'inizio del file:
    
     ```csharp
     using Microsoft.ServiceBus.Messaging;
     ```
    
     Sostituire quindi il metodo `Main` nella classe `Program` con il codice seguente, sostituendo il nome dell'hub eventi e la stringa di connessione a livello di spazio dei nomi salvata in precedenza, nonché l'account di archiviazione e la chiave copiata nelle sezioni precedenti. 
    
     ```csharp
     static void Main(string[] args)
     {
       string eventHubConnectionString = "{Event Hubs namespace connection string}";
       string eventHubName = "{Event Hub name}";
       string storageAccountName = "{storage account name}";
       string storageAccountKey = "{storage account key}";
       string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
    
       string eventProcessorHostName = Guid.NewGuid().ToString();
       EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
       Console.WriteLine("Registering EventProcessor...");
       var options = new EventProcessorOptions();
       options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
       eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
    
       Console.WriteLine("Receiving. Press enter key to stop worker.");
       Console.ReadLine();
       eventProcessorHost.UnregisterEventProcessorAsync().Wait();
     }
     ```

12. Eseguire il programma e assicurarsi che non siano presenti errori.
  
Congratulazioni. Sono stati ricevuti messaggi da un hub eventi usando l'host processore di eventi.


> [!NOTE]
> Questa esercitazione usa una singola istanza di [EventProcessorHost][EventProcessorHost]. Per aumentare la velocità effettiva, è consigliabile eseguire più istanze di [EventProcessorHost][EventProcessorHost], come illustrato nell'esempio di [elaborazione di eventi con aumento del numero di istanze][elaborazione di eventi con aumento del numero di istanze]. In questi casi, le varie istanze si coordinano automaticamente tra loro per ottenere il bilanciamento del carico relativo agli eventi ricevuti. Se si vuole che ognuno dei vari ricevitori elabori *tutti* gli eventi, è necessario usare il concetto **ConsumerGroup** . Quando si ricevono eventi da più macchine, potrebbe risultare utile specificare nomi per le istanze di [EventProcessorHost][EventProcessorHost] in base alle macchine (o ai ruoli) in cui sono distribuite. Per altre informazioni su questi argomenti, vedere [Panoramica di Hub eventi][Event Hubs Overview] e [Guida alla programmazione di Hub eventi][Event Hubs Programming Guide].
> 
> 

<!-- Links -->
[Event Hubs Overview]: event-hubs-overview.md
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]: ../storage/storage-create-storage-account.md
[Event Processor Host]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Passaggi successivi
Ora che è stata creata un'applicazione funzionante che crea un hub eventi e invia e riceve dati, è possibile ottenere altre informazioni visitando i collegamenti seguenti:

* [Host processore di eventi](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Panoramica di Hub eventi][Event Hubs overview]
* [Domande frequenti su Hub eventi](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
