---
title: Usare estensioni di PostgreSQL nel database di Azure per PostgreSQL | Documentazione Microsoft
description: "Descrive la capacità di estendere le funzionalità del database usando le estensioni nel database di Azure per PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql - database
ms.tgt_pltfrm: portal
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: dd16442078cd1d440edf0a16cba32b1494c6db55
ms.contentlocale: it-it
ms.lasthandoff: 05/10/2017

---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>Estensioni di PostgreSQL nel database di Azure per PostgreSQL
PostgreSQL offre la capacità di estendere le funzionalità del database usando le estensioni. Le estensioni consentono a più oggetti SQL correlati di essere collegati tra loro in un singolo pacchetto che può essere caricato o rimosso dal database con un singolo comando. Le estensioni, una volta caricate nel database, possono essere usate come funzionalità integrate. Per altre informazioni sulle estensioni di PostgreSQL, vedere [Packaging Related Objects into an Extension](https://www.postgresql.org/docs/9.6/static/extend-extensions.html) (Creare un pacchetto di oggetti correlati formando un'estensione).

## <a name="how-to-use-postgresql-extensions"></a>Come usare le estensioni di PostgreSQL?
Per poter usare le estensioni di PostgreSQL è necessario prima installarle per il database. Per installare una determinata estensione, eseguire un semplice comando [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) dallo strumento psql per caricare gli oggetti del pacchetto nel database.

Il database di Azure per PostgreSQL supporta un sottoinsieme di estensioni chiave come indicato di seguito ed è possibile installarle solo per il proprio database. È possibile creare estensioni personalizzate con il database di Azure per il servizio PostgreSQL.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Estensioni supportate dal database di Azure per PostgreSQL
Di seguito è riportato un elenco delle estensioni standard di PostgreSQL attualmente supportate dal database di Azure per PostgreSQL. È possibile ottenere queste informazioni anche eseguendo una query con pg\_available\_extensions. 

### <a name="data-types-extensions"></a>Estensioni di tipi di dati
| **Estensione** | **Descrizione** |
|------------------------------------------------------------------|--------------------------------------------------------|
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Fornisce un tipo stringa di caratteri che non distingue fra maiuscole e minuscole |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Fornisce il tipo di dati per l'archiviazione dei set di coppie chiave/valore |

### <a name="functions-extensions"></a>Estensioni di funzioni
| **Estensione** | **Descrizione** |
|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Offre diverse funzioni per determinare analogie e distanza tra le stringhe. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Fornisce funzioni e operatori per la manipolazione delle matrici di interi senza null. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Fornisce funzioni di crittografia |
| [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Gestisce le tabelle partizionate per ora o ID |
| [pg\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Fornisce funzioni e operatori per determinare la somiglianza del testo alfanumerico in base alla corrispondenza trigramma |
| [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Generare identificatori universalmente univoci (UUID) |

### <a name="full-text-search-extensions"></a>Estensioni di ricerca full-text
| **Estensione** | **Descrizione** |
|----------------------------------------------------------------------|-------------------------------------------------------------------------------|
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Un dizionario di ricerca di testo che rimuove gli accenti (segni diacritici) dai lessemi. |

### <a name="index-types-extensions"></a>Estensioni di tipi di indice
| **Estensione** | **Descrizione** |
|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| [btree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Fornisce classi operatore GIN di esempio che implementano un comportamento simile alla struttura b-tree per determinati tipi di dati. |
| [btree\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Fornisce classi operatore indice GiST che implementano la struttura b-tree. |

### <a name="language-extensions"></a>Estensioni di linguaggio
| **Estensione** | **Descrizione** |
|--------------------------------------------------------------------|---------------------------------------|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | Linguaggio procedurale caricabile PL/pgSQL |

### <a name="miscellaneous-extensions"></a>Estensioni varie
| **Estensione** | **Descrizione** |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Fornisce un modo per esaminare ciò che avviene nella cache del buffer condiviso in tempo reale. |
| [pg\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Fornisce un modo per caricare i dati di relazione nella cache del buffer. |
| [pg\_stat\_statements](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Fornisce un modo per tenere traccia delle statistiche di esecuzione di tutte le istruzioni SQL eseguite da un server. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Wrapper di dati esterni utilizzato per accedere ai dati archiviati in server PostgreSQL esterni |

### <a name="postgis"></a>PostGIS
| **Estensione** | **Descrizione** |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| [PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal | Oggetti spaziali e geografici per PostgreSQL. |
| address\_standardizer, address\_standardizer\_data\_us | Consente di analizzare un indirizzo nei suoi elementi costitutivi. In genere è utilizzata per supportare il passaggio di normalizzazione dell'indirizzo nella geocodifica. |
| [pgrouting](http://pgrouting.org/) | Estende il database geospaziale PostGIS/PostgreSQL per fornire funzionalità di routing geospaziale. |

## <a name="next-steps"></a>Passaggi successivi
Non è visualizzata l'estensione che si vuole usare? È possibile comunicarlo. Votare per le richieste esistenti o creare nuovi commenti e richieste nel [forum dei commenti di clienti](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
