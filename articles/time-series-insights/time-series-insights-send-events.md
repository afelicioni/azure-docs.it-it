---
title: Inviare eventi a un ambiente di Azure Time Series Insights | Microsoft Docs
description: Questa esercitazione illustra come effettuare il push degli eventi all&quot;ambiente Time Series Insights
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: venkatja
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 92e3e64f235e165a6a1772b6e1724789f3ec3049
ms.lasthandoff: 04/26/2017

---
# <a name="send-events-to-a-time-series-insights-environment-via-event-hub"></a>Inviare eventi a un ambiente Time Series Insights tramite un hub eventi

Questa esercitazione illustra come creare e configurare un hub eventi ed eseguire un'applicazione di esempio per effettuare il push degli eventi. Se esiste un hub eventi che include già eventi in formato JSON, è possibile saltare questa esercitazione e visualizzare l'ambiente nell'[utilità di esplorazione delle serie temporali](https://insights.timeseries.azure.com).

## <a name="configure-an-event-hub"></a>Configurare un hub eventi
1. Per creare un hub eventi, seguire le istruzioni contenute nella [documentazione](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) sugli hub eventi.

2. Assicurarsi di creare un gruppo di consumer usato esclusivamente dall'origine evento di Time Series Insights.

  > [!IMPORTANT]
  > Verificare che questo gruppo di consumer non venga usato da altri servizi, ad esempio un processo di Analisi di flusso o un altro ambiente Time Series Insights. Se il gruppo di consumer viene usato da altri servizi, l'operazione di lettura viene compromessa per questo ambiente e per gli altri servizi. Se si usa "$Default" come gruppo di consumer, è probabile che venga usato anche da altri lettori.

  ![Selezionare il gruppo di consumer dell'hub eventi](media/send-events/consumer-group.png)

3. Nell'hub eventi creare "MySendPolicy" che viene usato per inviare eventi nell'esempio seguente.

  ![Selezionare Criteri di accesso condiviso e fare clic sul pulsante Aggiungi](media/send-events/shared-access-policy.png)  

  ![Aggiungere nuovi criteri di accesso condiviso](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a>Creare un'origine evento di Time Series Insights
1. Se non è ancora stata creata un'origine evento, seguire le istruzioni indicate [qui](time-series-insights-add-event-source.md) per crearne una.

2. Specificare "deviceTimestamp" come nome della proprietà Timestamp. Questa proprietà viene usata come timestamp effettivo nell'esempio seguente. Il nome della proprietà Timestamp applica la distinzione tra maiuscole e minuscole e i valori devono avere il formato __aaaa-MM-ggTHH:mm:ss.FFFFFFFK__ quando viene inviato come file JSON all'hub eventi. Se la proprietà non esiste nell'evento, viene usata l'ora in cui l'evento è stato accodato all'hub eventi.

  ![Creare un'origine evento](media/send-events/event-source-1.png)

## <a name="run-sample-code-to-push-events"></a>Eseguire il codice di esempio per effettuare il push degli eventi
1. Passare al criterio dell'hub eventi "MySendPolicy" e copiare la stringa di connessione con la chiave del criterio.

  ![Copiare la stringa di connessione MySendPolicy](media/send-events/sample-code-connection-string.png)

2. Eseguire questo codice che invierà 600 eventi per ognuno dei tre dispositivi. Aggiornare `eventHubConnectionString` con la stringa di connessione.

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON to event hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a>Forme JSON supportate
### <a name="sample-1"></a>Esempio 1

#### <a name="input"></a>Input

Un oggetto JSON semplice.

```json
{
    "deviceId":"device1",
    "deviceTimestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a>Output: 1 evento

|deviceId|deviceTimestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>Esempio 2

#### <a name="input"></a>Input
Una matrice JSON con due oggetti JSON. Ogni oggetto JSON verrà convertito in un evento.
```json
[
    {
        "deviceId":"device1",
        "deviceTimestamp":"2016-01-08T01:08:00Z"
    },
    {
        "deviceId":"device2",
        "deviceTimestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---2-events"></a>Output: 2 eventi

|deviceId|deviceTimestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|

### <a name="sample-3"></a>Esempio 3

#### <a name="input"></a>Input

Un oggetto JSON con una matrice JSON annidata che contiene due oggetti JSON.
```json
{
    "location":"WestUs",
    "events":[
        {
            "deviceId":"device1",
            "deviceTimestamp":"2016-01-08T01:08:00Z"
        },
        {
            "deviceId":"device2",
            "deviceTimestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---2-events"></a>Output: 2 eventi
Si noti che la proprietà "location" viene copiata in ogni evento.

|location|events.deviceId|events.deviceTimestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>Esempio 4

#### <a name="input"></a>Input

```json
{
    "location":"WestUs",
    "manufacturerInfo":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "deviceId":"device1",
            "deviceTimestamp":"2016-01-08T01:08:00Z",
            "deviceData":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "deviceId":"device2",
            "deviceTimestamp":"2016-01-17T01:17:00Z",
            "deviceData":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---2-events"></a>Output: 2 eventi

|location|manufacturerInfo.name|manufacturerInfo.location|events.deviceId|events.deviceTimestamp|events.deviceData.type|events.deviceData.units|events.deviceData.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Passaggi successivi

* Visualizzare l'ambiente nel [portale di Time Series Insights](https://insights.timeseries.azure.com)

