---
title: Progettare il primo Database di Azure per il database MySQL - Interfaccia della riga di comando di Azure | Documentazione Microsoft
description: In questa esercitazione viene illustrato come creare e gestire il database di Azure per il server e il database MySQL tramite l&quot;interfaccia della riga di comando di Azure 2.0.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 99238b9c0bcc7fa80e8c72cda25f7ed809cc5195
ms.contentlocale: it-it
ms.lasthandoff: 05/10/2017

---

# <a name="design-your-first-azure-database-for-mysql-database"></a>Progettare il primo database di Azure per il database MySQL

Il database di Azure per MySQL è un servizio di database relazionale in Microsoft Cloud basato sul motore di database MySQL Community Edition. In questa esercitazione, si usano l'interfaccia della riga di comando di Azure e altre utilità per informazioni su come:

> [!div class="checklist"]
> * Creare un database di Azure per MySQL
> * Configurare il firewall del server Usare lo [strumento della riga di comando mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) per creare un database
> * Caricare dati di esempio
> * Eseguire query sui dati
> * Aggiornare i dati
> * Ripristinare i dati

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

## <a name="log-in-to-azure"></a>Accedere ad Azure

Accedere alla sottoscrizione di Azure con il comando [az login](/cli/azure/#login) e seguire le istruzioni visualizzate.

```azurecli
az login
```
Seguire le istruzioni del prompt dei comandi per aprire l'URL https://aka.ms/devicelog nel browser e quindi immettere il codice generato nel **prompt dei comandi**.

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse
Creare un [gruppo di risorse di Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) con il comando [az group create](https://docs.microsoft.com/cli/azure/group#create). Un gruppo di risorse è un contenitore logico in cui le risorse di Azure vengono distribuite e gestite come gruppo.

Nell'esempio seguente viene creato un gruppo di risorse denominato `mycliresource` nella posizione `westus`.

```azurecli
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Creare un database di Azure per il server MySQL
Creare un database di Azure per il server MySQL con il comando mysql server create. Un server può gestire più database. In genere, viene usato un database separato per ogni progetto o per ogni utente.

Nell'esempio seguente viene creato un database di Azure per il server MySQL in `westus` nel gruppo di risorse `mycliresource` con nome `mycliserver`. Il server dispone di un accesso di amministratore denominato `myadmin` e di una password `Password01!`. Il server viene creato con un livello di prestazioni **Basic** e **50** unità di calcolo condivise tra tutti i database nel server. È possibile aumentare o ridurre le capacità di calcolo e archiviazione a seconda delle esigenze dell'applicazione.

```azurecli
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Configurare una regola del firewall
Creare una regola del firewall a livello di server del Database SQL di Azure con il comando az mysql server firewall-rule create. Una regola del firewall a livello di server consente a un'applicazione esterna, ad esempio lo strumento della riga di comando **mysql** o MySQL Workbench, di connettersi al server tramite il firewall del servizio MySQL di Azure. 

Nell'esempio seguente viene creata una regola del firewall per un intervallo di indirizzi predefinito. In questo esempio viene illustrato l'intero intervallo possibile di indirizzi IP.

```azurecli
az mysql server firewall-rule create --resource-group mycliresource
--server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0
--end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a>Ottenere le informazioni di connessione

Per connettersi al server, è necessario fornire le informazioni sull'host e le credenziali di accesso.
```azurecli
az mysql server show --resource-group mycliresource --name mycliserver
```

Il risultato è in formato JSON. Annotare il **fullyQualifiedDomainName** e l'**administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-mysql"></a>Connettersi al server usando mysql
Usare lo [strumento della riga di comando mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) per stabilire una connessione al Database di Azure per il server MySQL. In questo esempio, il comando è:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Creazione di un database vuoto
Una volta connessi al server, creare un database vuoto.
```sql
mysql> CREATE DATABASE mysampledb;
```

Nel prompt, eseguire il comando seguente per cambiare la connessione nel database appena creato:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Creare tabelle nel database
Dopo aver appreso come connettersi al database di Azure per il database MySQL, si può passare al completamento di alcune attività di base.

In primo luogo, è possibile creare una tabella e caricarla con alcuni dati. Creare una tabella che contenga le informazioni riguardanti l'inventario.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Caricare i dati nelle tabelle
Ora che abbiamo una tabella, possiamo inserire alcuni dati al suo interno. Nella finestra del prompt dei comandi aperta, eseguire la query seguente per inserire alcune righe di dati.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

A questo punto, ci sono due righe di dati di esempio nella tabella creata in precedenza.

## <a name="query-and-update-the-data-in-the-tables"></a>Eseguire una query e aggiornare i dati nelle tabelle
Eseguire la query seguente per recuperare informazioni dalla tabella del database.
```sql
SELECT * FROM inventory;
```

Si possono anche aggiornare query e aggiornare i dati nelle tabelle.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

La riga viene aggiornata di conseguenza quando si recuperano dati.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Ripristinare un database a un momento precedente
Si supponga di aver eliminato accidentalmente questa tabella. Si tratta di un elemento che non è facile da ripristinare. Il database di Azure per MySQL consente di tornare in qualsiasi punto degli ultimi 35 giorni e di ripristinare questo punto nel tempo in un nuovo server. È possibile usare questo nuovo server per ripristinare i dati eliminati. La procedura seguente consente di ripristinare il server di esempio in un punto precedente all'aggiunta della tabella.

Per il ripristino sono necessarie le informazioni seguenti:

- Punto di ripristino: selezionare un punto nel tempo precedente alla modifica del server. Il punto deve essere maggiore o equivalente al valore del backup meno recente del database di origine.
- Server di destinazione: fornire un nuovo nome del server che si desidera ripristinare.
- Server di origine: fornire il nome del server che si desidera ripristinare
- Posizione: non è possibile selezionare l'area, per impostazione predefinita è la stessa del server di origine
- Piano tariffario: non è possibile modificare questo valore quando si ripristina un server. È uguale al server di origine. 

```azurecli
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

Per ripristinare il server e [ ripristinare in un punto nel tempo ](./howto-restore-server-portal.md) precedente all'eliminazione delle tabelle. Il ripristino di un server in un altro punto nel tempo crea un duplicato del nuovo server come il server originale nel punto nel tempo specificato, purché sia entro il periodo di conservazione per il [livello di servizio](./concepts-service-tiers.md) applicato.

## <a name="next-steps"></a>Passaggi successivi
Questa esercitazione illustra come:
> [!div class="checklist"]
> * Creare un database di Azure per MySQL
> * Configurare il firewall del server Usare lo [strumento della riga di comando mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) per creare un database
> * Caricare dati di esempio
> * Eseguire query sui dati
> * Aggiornare i dati
> * Ripristinare i dati

[Database di Azure per MySQL - Esempi di interfaccia della riga di comando di Azure](./sample-scripts-azure-cli.md)

