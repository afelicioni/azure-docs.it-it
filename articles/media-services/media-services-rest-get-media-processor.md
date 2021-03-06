---
title: Come creare un processore multimediale | Microsoft Docs
description: Informazioni su come creare un componente del processore di contenuti multimediali per codificare, decodificare, convertire il formato, crittografare o decrittografare contenuti multimediali per Servizi multimediali di Azure.
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: d1d05b46591fe4b72c92c59d357681c8e1cdb336
ms.openlocfilehash: c208660fc1439ca831ada6c9bb348dbc3eadc18c


---
# <a name="how-to-get-a-media-processor-instance"></a>Procedura: Ottenere un'istanza del processore di contenuti multimediali
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Overview
In Servizi multimediali un processore di contenuti multimediali è un componente che gestisce un'attività di elaborazione specifica, ad esempio la codifica, la conversione del formato, la crittografia o la decrittografia di contenuti multimediali. Un processore multimediale viene generalmente creato durante la creazione di un'attività per la codifica, la crittografia o la conversione di formato di contenuto multimediale.

Nella tabella seguente sono riportati il nome e la descrizione di tutti i processori multimediali disponibili.

| Nome del processore multimediale | Descrizione | Altre informazioni |
| --- | --- | --- |
| Codificatore multimediale standard |Fornisce funzionalità standard per la codifica su richiesta. |[Panoramica e confronto dei codificatori multimediali su richiesta di Azure](media-services-encode-asset.md) |
| Flusso di lavoro Premium del codificatore multimediale |Consente di eseguire attività di codifica usando il flusso di lavoro Premium del codificatore multimediale. |[Panoramica e confronto dei codificatori multimediali su richiesta di Azure](media-services-encode-asset.md) |
| Azure Media Indexer |Consente di rendere disponibili per la ricerca file e contenuti multimediali, oltre a generare tracce e parole chiave per i sottotitoli codificati. |[Azure Media Indexer](media-services-index-content.md) |
| Azure Media Hyperlapse (anteprima) |Consente di ridurre le imperfezioni del video con la stabilizzazione video. Consente inoltre di velocizzare il contenuto in una clip utilizzabile. |[Azure Media Hyperlapse](media-services-hyperlapse-content.md) |
| Azure Media Encoder |Funzionalità deprecate | |
| Storage Decryption |Funzionalità deprecate | |
| Azure Media Packager |Funzionalità deprecate | |
| Azure Media Encryptor |Funzionalità deprecate | |

## <a name="get-mediaprocessor"></a>Ottenere un'istanza di MediaProcessor
> [!NOTE]
> Quando si usa l'API REST di Servizi multimediali, tenere presenti le seguenti considerazioni:
> 
> Quando si accede alle entità in Servizi multimediali, è necessario impostare valori e campi di intestazione specifici nelle richieste HTTP. Per altre informazioni, vedere [Panoramica dell'API REST di Servizi multimediali](media-services-rest-how-to-use.md).
> 
> Dopo avere stabilito la connessione a https://media.windows.net, si riceverà un reindirizzamento 301 che indica un altro URI di Servizi multimediali. Le chiamate successive dovranno essere eseguite al nuovo URI, come descritto in [Connessione a un account di Servizi multimediali mediante l'API REST](media-services-rest-connect-programmatically.md). 
> 
> 

La chiamata REST seguente mostra come ottenere un'istanza del processore di contenuti multimediali per nome, in questo caso **Media Encoder Standard**. 

Richiesta:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

Risposta:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a>Percorsi di apprendimento di Servizi multimediali
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Fornire commenti e suggerimenti
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Passaggi successivi
Dopo avere ottenuto un'istanza del processore di contenuti multimediali, passare all'argomento [Come codificare un asset](media-services-rest-get-started.md) che illustra come usare Media Encoder Standard per codificare un asset.




<!--HONumber=Feb17_HO3-->


