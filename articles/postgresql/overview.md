---
title: Panoramica del servizio di database relazionale Database di Azure per PostgreSQL | Microsoft Docs
description: Offre una panoramica del servizio di database relazionale Database di Azure per PostgreSQL.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.tgt_pltfrm: portal
ms.topic: overview
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 8aae9994f8fc6bb8e4e5368a1495dd257be0417f
ms.contentlocale: it-it
ms.lasthandoff: 05/10/2017

---
# <a name="what-is-azure-database-for-postgresql"></a>Che cos'è Database di Azure per PostgreSQL

Database di Azure per PostgreSQL è un servizio di database relazionale in Microsoft Cloud creato per gli sviluppatori e basato sulla versione della community del motore di database [PostgreSQL](https://www.postgresql.org/) open source. Questo servizio è disponibile in anteprima pubblica. Database di Azure per PostgreSQL offre:
- Prestazioni prevedibili a più livelli di servizio
- Scalabilità dinamica senza tempi di inattività dell'applicazione
- Disponibilità elevata predefinita
- Protezione dati

Tutte le funzionalità richiedono pochissima amministrazione e sono disponibili senza costi aggiuntivi. Queste funzionalità consentono di concentrarsi sullo sviluppo rapido di applicazioni e accelerare i tempi di mercato, piuttosto che allocare tempo e risorse preziose alla gestione delle macchine virtuali e dell'infrastruttura. È anche possibile continuare a sviluppare l'applicazione con la piattaforma e gli strumenti open source scelti e a renderla disponibile con la rapidità e l'efficienza richiesti dall'azienda senza bisogno di acquisire nuove competenze. 

Questo articolo è un'introduzione dei concetti fondamentali su Database di Azure per PostgreSQL e delle funzionalità correlate a prestazioni, scalabilità e gestibilità. Per iniziare, vedere queste guide introduttive:

- [Creare un database di Azure per PostgreSQL usando il portale di Azure](quickstart-create-server-database-portal.md)
- [Creare un Database di Azure per PostgreSQL usando l'interfaccia della riga di comando di Azure](quickstart-create-server-database-azure-cli.md)

Per un set di esempi dell'interfaccia della riga di comando di Azure e di PowerShell, vedere:

- [Azure CLI samples for Azure Database for PostgreSQL](./sample-scripts-azure-cli.md) (Esempi di interfaccia della riga di comando di Azure per Database di Azure per PostgreSQL)

## <a name="adjust-performance-and-scale-without-downtime"></a>Regolare le prestazioni e scalabilità senza tempi di inattività

Il servizio Database di Azure per PostgreSQL offre attualmente due livelli di servizio: Basic e Standard. Ogni livello di servizio offre [diversi livelli di prestazioni, garanzie di Operazioni di I/O al secondo e capacità](concepts-service-tiers.md) per supportare i carichi di lavoro del database, da quelli leggeri a quelli pesanti. È possibile creare la prima app in un server di piccole dimensioni per un costo mensile conveniente e quindi [modificare il livello delle prestazioni](scripts/sample-scale-server-up-or-down.md) nel livello di servizio manualmente o a livello di codice in qualsiasi momento per soddisfare le esigenze della soluzione. È possibile eseguire questa operazione senza tempi di inattività per l'applicazione o per i clienti. La scalabilità dinamica consente al database di rispettare i requisiti in continua evoluzione relativi alle risorse e di pagare solo le risorse necessarie quando necessario.

## <a name="monitoring-and-alerting"></a>Monitoraggio e avviso

Per decidere quando aumentare e ridurre le prestazioni, si usano le funzionalità predefinite di monitoraggio delle prestazioni e avviso, combinate con le classificazioni delle prestazioni basate sulle unità di calcolo. Usando questi strumenti, è possibile valutare rapidamente l'impatto dell'aumento o della riduzione delle unità di calcolo in base alle esigenze correnti o previste relative alle prestazioni. Per informazioni dettagliate, vedere [Azure Database for PostgreSQL options and performance: Understand what's available in each service tier](./concepts-service-tiers.md) (Opzioni e prestazioni disponibili in ogni livello di servizio di Database di Azure per PostgreSQL).

## <a name="keep-your-app-and-business-running"></a>Mantenere l'applicazione e l’esecuzione dell’azienda

Il Contratto di servizio per la disponibilità del 99,99% (non disponibile in anteprima) leader del settore di Azure, fornito da una rete globale di datacenter gestiti da Microsoft, consente di mantenere l'applicazione in esecuzione 24 ore su 24, 7 giorni su 7. Con ogni database di Azure per il server PostgreSQL, è possibile sfruttare i vantaggi delle funzionalità di sicurezza, tolleranza di errore e protezione dei dati predefinite che sarebbe altrimenti necessario acquistare o progettare, creare e gestire. Con Database di Azure per PostgreSQL, ogni livello di servizio offre un set completo di funzionalità di continuità aziendale e opzioni che è possibile usare per diventare e rimanere operativi. È possibile usare un [ripristino temporizzato](howto-restore-server-portal.md) per riportare un database a uno stato precedente, risalente anche a 35 giorni fa. Se nel data center che ospita i database si verifica un'interruzione, è anche possibile ripristinare i database dalle copie con ridondanza geografica di backup recenti.

## <a name="secure-your-data"></a>Protezione dei dati

I servizi di database di Azure vantano una tradizione di sicurezza dei dati rispettata anche da Database di Azure per PostgreSQL con funzionalità che limitano l'accesso, proteggono i dati inattivi e in transito e consentono di monitorare l'attività. Visitare il [Centro protezione di Azure](https://www.microsoft.com/TrustCenter/Security/default.aspx) per informazioni sulla sicurezza della piattaforma Azure.

## <a name="next-steps"></a>Passaggi successivi
- Vedere la [pagina relativa ai prezzi](https://azure.microsoft.com/pricing/details/postgresql/) per confrontare i costi e usare i calcolatori.
- Per iniziare, [creare il primo database di Azure per PostgreSQL tramite](./quickstart-create-server-database-portal.md).
- Compilare la prima app in Python, PHP, Ruby, C\#, Java, Node.js: [raccolte di connessioni](./concepts-connection-libraries.md)

