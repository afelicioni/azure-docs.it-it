---
title: Visualizzazione dei log di diagnostica per Azure Data Lake Analytics | Documentazione Microsoft
description: 'Informazioni su come configurare e accedere ai log di diagnostica per Azure Data Lake Analytics  '
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/03/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: 71a839d9c076d33c404a56bf0544666dc86eaa4d
ms.contentlocale: it-it
ms.lasthandoff: 05/26/2017


---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Accesso ai log di diagnostica per Azure Data Lake Analytics

Informazioni su come abilitare la registrazione diagnostica per l'account di Data Lake Analytics e su come visualizzare i log raccolti per l'account.

Le organizzazioni possono abilitare la registrazione diagnostica per il proprio account di Azure Data Lake Analytics per raccogliere gli audit trail dell'accesso ai dati. Questi registri includono informazioni quali:

* Un elenco di utenti che hanno avuto accesso ai dati.
* La frequenza con cui si accede ai dati.
* La quantità di dati archiviati nell'account.

## <a name="enable-logging"></a>Abilitazione della registrazione

1. Accedere al nuovo [portale di Azure](https://portal.azure.com).

2. Aprire l'account Data Lake Analytics e selezionare **Log di diagnostica** dalla sezione __Monitoraggio__. Selezionare quindi __Turn on diagnostics__ (Attiva diagnostica).

    ![Attivare la diagnostica per acquisire i log di controllo e i log delle richieste.](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. Dalle __Impostazioni di diagnostica__, impostare lo stato su __Attivo__ e selezionare le opzioni di registrazione.

    ![Attivare la diagnostica per acquisire i log di controllo e i log delle richieste](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Abilitare i log di diagnostica")

   * Impostare lo **Stato** su **Attivo** per abilitare la registrazione diagnostica.

   * È possibile scegliere di archiviare/elaborare i dati in due modi diversi.

     * Selezionare __Archive to a storage account__ (Archivia in un account di archiviazione) per archiviare i log in un account di archiviazione di Azure. Usare questa opzione se si vogliono archiviare i dati. Se si seleziona questa opzione è necessario fornire un account di archiviazione di Azure in cui salvare i log.

     * Selezionare **Stream to an Event Hub** (Esegui streaming in un Hub eventi) per trasmettere i dati di log a un Hub eventi di Azure. Usare questa opzione se si ha una pipeline di elaborazione a valle per analizzare in tempo reale i log in ingresso. Se si seleziona questa opzione, è necessario fornire i dettagli dell'Hub eventi di Azure che si desidera utilizzare.

     * Selezionare __Send to Log Analytics__ (Invia a Log Analytics) per inviare i dati al servizio Log Analytics. Usare questa opzione per raccogliere e analizzare i log con Log Analytics.
   * Specificare se si desidera ottenere i log di controllo, i log delle richieste o entrambi.

   * Specificare il numero di giorni per cui devono essere conservati i dati.

   * Fare clic su **Save**.

        > [!NOTE]
        > È necessario selezionare una tra le opzioni __Archive to a storage account__ (Archivia in un account di archiviazione), __Stream to an Event Hub__ (Esegui streaming in un Hub eventi) o __Send to Log Analytics__ (Invia a Log Analytics) prima di usare il pulsante __Salva__.

Dopo aver attivato le impostazioni di diagnostica, è possibile tornare al pannello __Log di diagnostica__ per visualizzare i log.

## <a name="view-logs"></a>Visualizzare i log

Esistono due modi per visualizzare i dati di log dell'account Data Lake Analytics.

* Dalle impostazioni dell'account Data Lake Analytics
* Dall'account di Archiviazione di Azure dove sono archiviati i dati

### <a name="using-the-data-lake-analytics-settings-view"></a>Usando la visualizzazione Impostazioni dell'account Data Lake Analytics

1. Dal pannello dell'account Data Lake Analytics selezionare **Log di diagnostica** e quindi la voce per la quale visualizzare i log.

    ![Visualizzare la registrazione diagnostica](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Visualizzare i log di diagnostica")

2. I log sono suddivisi in **Log di controllo** e **Log delle richieste**.

    ![Voci del log](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * I log delle richieste acquisiscono tutte le richieste API fatte nell'account di Data Lake Analytics.
   * I log di controllo sono simili a quelli delle richieste ma forniscono una suddivisione più dettagliata delle operazioni eseguite nell'account di Data Lake Analytics. Ad esempio, una singola chiamata API di caricamento nei log delle richieste potrebbe risultare in molteplici operazioni di "Aggiungi" nei log di controllo.

3. Fare clic sul link **Scarica** di una voce di log per scaricare i log.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Dall'account di Archiviazione di Azure che contiene i dati di log

1. Aprire il pannello Account di Archiviazione di Azure associato a Data Lake Analytics per la registrazione e quindi fare clic su BLOB. Il pannello **Servizio BLOB** elenca due contenitori.

    ![Visualizzare la registrazione diagnostica](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Visualizzare i log di diagnostica")

   * Il contenitore **insights-logs-audit** contiene i log di controllo.
   * Il contenitore **insights-logs-requests** contiene i log delle richieste.
2. All'interno di questi contenitori i log vengono archiviati con la struttura seguente.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > Il pannello `##` nel percorso contengono l'anno, il mese, il giorno e l'ora in cui è stato creato il log. Data Lake Analytics crea un file ogni ora, in modo `m=` contenga sempre un valore di `00`.

    Ad esempio, il percorso completo a un log di controllo potrebbe essere:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Analogamente, il percorso completo a un log della richiesta potrebbe essere:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Struttura di log

I log di controllo e delle richieste sono in formato JSON. In questa sezione, viene esaminata la struttura di JSON per i log delle richieste e di controllo.

### <a name="request-logs"></a>Request Logs

Di seguito viene riportata una voce di esempio nel log delle richieste in formato JSON. Ogni BLOB ha un oggetto radice denominato **record** che contiene una matrice di oggetti di log.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Schema del log delle richieste

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| time |String |Il timestamp del log (fusorario UTC) |
| resourceId |String |L’ID della risorsa interessata dall’operazione |
| category |String |La categoria di log. Ad esempio, **Richieste**. |
| operationName |String |Il nome dell'operazione registrata. Ad esempio, GetAggregatedJobHistory. |
| resultType |String |Lo stato dell'operazione, ad esempio 200. |
| callerIpAddress |String |L’indirizzo IP del client che esegue la richiesta |
| correlationId |String |L'id del log. Questo valore può essere usato per raggruppare un set di voci di log correlate |
| identity |Oggetto |L'identità che ha generato il log |
| properties |JSON |Per informazioni dettagliate, vedere la sezione successiva (Schema delle proprietà del log di richiesta) |

#### <a name="request-log-properties-schema"></a>Schema delle proprietà del log di richiesta

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| HttpMethod |String |Il metodo HTTP utilizzato per l'operazione. Esempio: GET. |
| Path |String |Il percorso coinvolto nell'operazione |
| RequestContentLength |int |La lunghezza del contenuto della richiesta HTTP |
| ClientRequestId |String |L’ID che identifica in modo univoco la richiesta |
| StartTime |String |L'ora in cui il server ha ricevuto la richiesta |
| EndTime |String |L'ora in cui il server ha inviato una risposta |

### <a name="audit-logs"></a>Log di controllo

Di seguito viene riportata una voce di esempio nel log di controllo in formato JSON. Ogni BLOB ha un oggetto radice denominato **record** che contiene una matrice di oggetti di log.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Schema del log di controllo

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| time |String |Il timestamp del log (fusorario UTC) |
| resourceId |String |L’ID della risorsa interessata dall’operazione |
| category |String |La categoria di log. Ad esempio, **Audit**. |
| operationName |String |Il nome dell'operazione registrata. Ad esempio, JobSubmitted. |
| resultType |String |Stato secondario per lo stato del processo (operationName). |
| resultSignature |String |Informazioni aggiuntive sullo stato di processo (operationName). |
| identity |String |L'utente che ha richiesto l'operazione. Ad esempio, susan@contoso.com. |
| properties |JSON |Per informazioni dettagliate, vedere la sezione successiva (Schema delle proprietà del log di controllo) |

> [!NOTE]
> **resultType** e **resultSignature** offrono informazioni sul risultato di un'operazione e contengono un valore solo se un'operazione è stata completata. Ad esempio, contengono un valore quando **operationName** contiene un valore **JobStarted** o **JobEnded**.
>
>

#### <a name="audit-log-properties-schema"></a>Schema delle proprietà del log di controllo

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| JobId |String |L'ID assegnato al processo |
| JobName |String |Il nome fornito per il processo |
| JobRunTime |String |Il runtime usato per l'elaborazione del processo |
| SubmitTime |String |Ora (UTC) in cui è stato inviato il processo |
| StartTime |String |Ora di avvio dell'esecuzione del processo dopo l'invio (in UTC). |
| EndTime |String |Ora di termine del processo. |
| Parallelismo |String |Il numero di unità di Data Lake Analytics richiesto per questo processo durante l'invio. |

> [!NOTE]
> **SubmitTime**, **StartTime**, **EndTime** e **Parallelism** offrono informazioni su un'operazione e contengono un valore solo se un'operazione è stata avviata o completata. Ad esempio, **SubmitTime** contiene un valore dopo che **operationName** indica **JobSubmitted**.

## <a name="process-the-log-data"></a>Elaborare i dati di log

Azure Data Lake Analytics fornisce un esempio su come elaborare e analizzare i dati di log. È possibile trovare l'esempio all'indirizzo [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Passaggi successivi
* [Panoramica di Azure Data Lake Analytics](data-lake-analytics-overview.md)

