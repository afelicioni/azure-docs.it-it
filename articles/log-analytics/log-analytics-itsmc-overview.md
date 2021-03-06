---
title: IT Service Management Connector in OMS | Microsoft Docs
description: Usare IT Service Management Connector per monitorare e gestire centralmente gli elementi di lavoro di ITSM in OMS e risolvere rapidamente eventuali problemi.
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: v-jysur
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 0aa41bbc0e0135737d352553607f48a39757bcc3
ms.contentlocale: it-it
ms.lasthandoff: 05/09/2017


---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Gestire centralmente gli elementi di lavoro ITSM con IT Service Management Connector (anteprima)

È possibile usare IT Service Management Connector (ITSMC) in Log Analytics di OMS per monitorare e gestire centralmente gli elementi di lavoro nei prodotti o servizi di ITSM.

IT Service Management Connector integra i prodotti e i servizi di gestione IT (ITSM) esistenti con Log Analytics di OMS.  La soluzione presenta un'integrazione bidirezionale con i prodotti e i servizi ITSM, in cui offre agli utenti OMS un'opzione per creare eventi imprevisti, avvisi o eventi nella soluzione ITSM. Il connettore importa anche dati, ad esempio eventi imprevisti e richieste di modifica dalla soluzione ITSM in Log Analytics di OMS.

Con IT Service Management Connector è possibile:

  - Monitorare e gestire centralmente gli elementi di lavoro per i prodotti e i servizi ITSM usati nell'organizzazione.
  - Creare elementi di lavoro ITSM, ad esempio avvisi, eventi, eventi imprevisto, in ITSM dagli avvisi OMS e tramite la ricerca log.
  - Leggere gli eventi imprevisti e richieste di modifica dalla soluzione ITSM e correlare i dati di log rilevanti nell'area di lavoro di Log Analytics.
  - Trovare tutti gli eventi imprevisti e non comuni e risolverli, anche prima che gli utenti finali si rivolgano e li segnalino al supporto tecnico.
  - Importare i dati degli elementi di lavoro in Log Analytics e creare report indicatori di prestazioni chiave.  Grazie all'uso di questi report, è possibile individuare, valutare e agire su diversi elementi importanti, ad esempio la valutazione della presenza di malware.
  - Visualizzare i dashboard curati per ottenere informazioni approfondite su eventi imprevisti, richieste di modifica e sistemi interessati.
  - Risolvere i problemi più rapidamente tramite correlazione con altre soluzioni di gestione nell'area di lavoro Log Analytics.


## <a name="prerequisites"></a>Prerequisiti

Per importare gli elementi di lavoro ITSM in Log Analytics di OMS, la soluzione richiede una connessione tra IT Service Management Connector in OMS e i prodotti o i servizi ITSM da cui si importano gli elementi di lavoro.


## <a name="configuration"></a>Configurazione

Aggiungere la soluzione IT Service Management Connector all'area di lavoro di OMS usando la procedura descritta in [Aggiungere soluzioni di Log Analytics dalla raccolta soluzioni](log-analytics-add-solutions.md).

Riquadro di IT Service Management Connector come appare nella raccolta soluzioni:

![riquadro connettore](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Dopo averlo aggiunto correttamente, IT Service Management Connector viene visualizzato in **OMS** > **Impostazioni** > **Origini connesse.**

![ITSMC connessi](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

## <a name="management-packs"></a>Management Pack
Questa soluzione non richiede alcun pacchetto di gestione.

## <a name="connected-sources"></a>Origini connesse

I seguenti prodotti o servizi ITSM sono supportati da IT Service Management Connector:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>Uso della soluzione

Una volta collegato IT Service Management connector di OMS con il servizio ITSM, i servizi di Log Analytics avviano la raccolta dei dati dai prodotti o servizi ITSM connessi.

> [!NOTE]
> - I dati importati dalla soluzione IT Service Management Connector vengono visualizzati in Log Analytics come eventi denominati **ServiceDesk_CL**.
- L'evento contiene un campo denominato **ServiceDeskWorkItemType_s**. che può prendere il proprio valore come evento imprevisto o richiesta di modifica, a seconda dei dati dell'elemento di lavoro contenuti nell'evento **ServiceDesk_CL**.

Le informazioni seguenti mostrano esempi di dati raccolti da IT Service Management Connector:

> [!NOTE]
> A seconda del tipo di elemento di lavoro importato in Log Analytics, **ServiceDesk_CL** contiene i campi seguenti:

**Elemento di lavoro:** **Eventi imprevisti**  
ServiceDeskWorkItemType_s="Incident"

**Fields**

- ServiceDeskConnectionName
- ID Service Desk
- Stato
- Urgenza
- Impatto
- Priorità
- Riassegnazione
- Created By (Creato da)
- Resolved By (Risolto da)
- Closed By (Chiuso da)
- Sorgente
- Assegnato a 
- Categoria
- Titolo
- Descrizione
- Data di creazione
- Data di chiusura
- Data di risoluzione
- Data ultima modifica
- Computer


**Elemento di lavoro:** **Richieste di modifica**

ServiceDeskWorkItemType_s="ChangeRequest"

**Fields**
- ServiceDeskConnectionName
- ID Service Desk
- Created By (Creato da)
- Closed By (Chiuso da)
- Sorgente
- Assegnato a 
- Titolo
- Tipo
- Categoria
- Stato
- Riassegnazione
- Conflict Status (Stato di conflitto)
- Urgenza
- Priorità
- Rischio
- Impatto
- Assegnato a 
- Data di creazione
- Data di chiusura
- Data ultima modifica
- Data richiesta
- Planned Start Date (Data di inizio pianificata)
- Planned End Date (Data di fine pianificata)
- Work Start Date (Data di inizio lavoro)
- Work End Date (Data di fine pianificata)
- Descrizione
- Computer

Schermata di esempio di Log Analytics per i dati ITSM:

![Schermata di Log Analytics](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a>IT Service Management connector: integrazione con altre soluzioni OMS

IT Service Management connector supporta attualmente l'integrazione con la soluzione Elenco dei servizi.

Elenco dei servizi individua automaticamente i componenti delle applicazioni nei sistemi Windows e Linux ed esegue la mappatura della comunicazione fra i servizi. Consente di visualizzare i server nel modo in cui si pensa a essi, ovvero come sistemi interconnessi che forniscono servizi critici. L'elenco dei servizi mostra le connessioni fra i server, i processi e le porte di tutte le architetture connesse via TCP senza il bisogno di alcuna configurazione a parte l'installazione di un agente. Altre informazioni: [Elenco dei servizi](../operations-management-suite/operations-management-suite-service-map.md).

Con questa integrazione, è possibile visualizzare gli elementi del service desk creati nelle soluzioni ITSM come illustrato nell'esempio seguente:

![Soluzione integrata ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Creare elementi di lavoro ITSM per avvisi di OMS

Per gli avvisi OMS, è possibile creare elementi di lavoro associati nelle origini ITSM connesse.  A tale scopo, usare la procedura seguente:

1. Dalla finestra **Ricerca Log** eseguire una query di ricerca log per visualizzare i dati. I risultati della query sono l'origine degli elementi di lavoro.
2. In **Ricerca Log** fare clic su **Avviso** per aprire la pagina **Aggiungi regola di avviso**.

    ![Schermata di Log Analytics](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Nella finestra **Aggiungi regola di avviso**, inserire i dettagli necessari per **Nome**, **Gravità**,  **	Query di ricerca** e **Criteri avvisi** (misurazione dell'intervallo di tempo/metrica).
4. Selezionare **Sì** per **Azioni ITSM**.
5. Selezionare la connessione ITSM dall'elenco **Selezionare una connessione**.
6. Specificare i dettagli richiesti.
7. Per creare un elemento di lavoro separato per ogni voce di log dell'avviso, selezionare la casella di controllo **Crea elementi di lavoro singoli per ogni voce di log**.

    oppure

    non selezionare questa casella di controllo per creare un solo elemento di lavoro per il numero di voci di log in questo avviso.

7. Fare clic su **Save**.

In **avvisi** verrà creato l'avviso OMS. Gli elementi di lavoro della connessione ITSM corrispondente vengono creati quando viene soddisfatta la condizione dell'avviso specificata.

## <a name="create-itsm-work-items-from-oms-logs"></a>Creare elementi di lavoro ITSM da log di OMS

È possibile creare elementi di lavoro nelle origini ITSM connesse tramite Ricerca log di OMS. A tale scopo, usare la procedura seguente:

1. Da **Ricerca Log** cercare i dati richiesti, selezionare i dettagli e fare clic su **Crea elemento di lavoro**.

    Viene visualizzata la finestra **Crea elemento di lavoro ITSM**:

    ![Schermata di Log Analytics](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Aggiungere i dettagli seguenti:

  - **Titolo elemento di lavoro**: il titolo dell'elemento di lavoro.
  - **Descrizione elemento di lavoro**: descrizione per il nuovo elemento di lavoro.
  - **Computer interessato**: nome del computer in cui sono stati trovati i dati del log.
  - **Selezionare una connessione**: connessione ITSM in cui si desidera creare questo elemento di lavoro.
  - **Elemento di lavoro**: tipo di elemento di lavoro.

3. Per usare un modello di elemento di lavoro esistente per un evento imprevisto, fare clic su **Sì** nell'opzione **Generate work item based on the template** (Genera elemento di lavoro in base al modello) e quindi fare clic su **Crea**.

    Oppure

    Fare clic su **No** se si desidera specificare valori personalizzati.

4. Inserire i valori appropriati nelle caselle di testo **Tipo di contatto**, **Impatto**, **Urgenza**, **Categoria** e **Sottocategoria** e quindi fare clic su **Crea**.

Verrà creato l'elemento di lavoro in ITSM, che può essere visualizzato anche in OMS.

## <a name="contact-us"></a>Contatti

Per eventuali domande o commenti e suggerimenti su IT Service Management Connector, è possibile contattare [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Passaggi successivi
[Aggiungere prodotti o servizi ITSM a IT Service Management Connector](log-analytics-itsmc-connections.md).

