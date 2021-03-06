---
title: Connettere i computer Linux a Operations Management Suite (OMS) | Microsoft Docs
description: Questo articolo descrive come connettere i computer Windows ospitati nell&quot;infrastruttura locale a OMS usando Microsoft Monitoring Agent (MMA).
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 3c556f3d9e81caae574ec093b6f2ce15651c4485
ms.contentlocale: it-it
ms.lasthandoff: 05/11/2017

---

# <a name="connect-your-linux-computers-to-operations-management-suite-oms"></a>Connettere i computer Linux a Operations Management Suite (OMS)

Con OMS, è possibile raccogliere i dati generati dai computer Linux e da soluzioni contenitore come Docker, che si trovano nel data center locale, come macchine virtuali o server fisici in locale, macchine virtuali in un servizio ospitato nel cloud come Amazon Web Services (AWS) o Microsoft Azure, nonché usare tali dati. È anche possibile usare soluzioni di gestione disponibili in OMS, ad esempio Rilevamento modifiche per identificare le modifiche alla configurazione e Gestione aggiornamenti per gestire gli aggiornamenti software, per una gestione proattiva del ciclo di vita delle VM Linux.

L'agente OMS per Linux comunica in uscita con il servizio OMS sulla porta TCP 443 e se il computer si connette a un firewall o a un server proxy per comunicare in Internet, vedere [Configurare le impostazioni di proxy e firewall](log-analytics-proxy-firewall.md) per comprendere quali modifiche alla configurazione è necessario applicare.  Se si esegue il monitoraggio del computer con System Center 2016 - Operations Manager o Operations Manager 2012 R2, è possibile usare una configurazione multihomed con il servizio OMS per raccogliere i dati e inoltrarli al servizio, mantenendo il monitoraggio di Operations Manager.  I computer Linux monitorati da un gruppo di gestione di Operations Manager integrato in OMS non ricevono la configurazione per le origini dati e inoltrano i dati raccolti tramite il gruppo di gestione.  

Se i criteri di sicurezza IT non consentono ai computer nella rete di connettersi a Internet, è possibile configurare l'agente per la connessione al gateway OMS, per ricevere informazioni di configurazione e inviare i dati raccolti a seconda della soluzione abilitata. Per altre informazioni e procedure per la configurazione dell'agente Linux OMS per la comunicazione tramite un gateway OMS con il servizio OMS, vedere [Connettere computer a OMS usando il gateway OMS](log-analytics-oms-gateway.md).  

Il diagramma seguente illustra la connessione tra i computer Linux gestiti dall'agente e OMS, incluse la direzione e le porte.

![Diagramma della comunicazione degli agenti diretti con OMS](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Requisiti di sistema
Prima di iniziare, esaminare i dettagli seguenti per verificare che i prerequisiti siano soddisfatti.

### <a name="supported-linux-operating-systems"></a>Sistemi operativi Linux supportati
Le distribuzioni Linux seguenti sono supportate ufficialmente.  È tuttavia possibile che l'agente OMS per Linux sia eseguito in altre distribuzioni non elencate.

* Amazon Linux 2012.09 --> 2015.09 (x86/x64)
* CentOS Linux 5,6 e 7 (x86/x64)
* Oracle Linux 5,6 e 7 (x86/x64)
* Red Hat Enterprise Linux Server 5,6 e 7 (x86/x64)
* Debian GNU/Linux 6, 7 e 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 e 12 (x86/x64)

### <a name="package-requirements"></a>Requisiti dei pacchetti

 **Pacchetto obbligatorio**     | **Descrizione**     | **Versione minima**
--------------------- | --------------------- | -------------------
Glibc |    Libreria GNU C    | 2.5-12
Openssl    | Librerie OpenSSL | 0.9.8e o 1.0
Curl | Client Web cURL | 7.15.5
Python-ctypes | |
PAM | Moduli di autenticazione modulare     |

> [!NOTE]
>  Per raccogliere i messaggi SysLog, è necessario rsyslog o syslog-ng. Il daemon SysLog predefinito nella versione 5 di Red Hat Enterprise Linux, CentOS e nella versione Oracle Linux (sysklog) non è supportato per la raccolta di eventi SysLog. Per raccogliere i dati di SysLog da questa versione delle distribuzioni, è necessario che il daemon rsyslog sia installato e configurato per sostituire sysklog.

L'agente è costituito da più pacchetti. Il file della versione rilasciata contiene i pacchetti seguenti, disponibili eseguendo il bundle della shell con `--extract`:

**Pacchetto** | **Versione** | **Descrizione**
----------- | ----------- | --------------
omsagent | 1.3.4 | Agente Operations Management Suite per Linux
omsconfig | 1.1.1 | Agente di configurazione per l'agente OMS
omi | 1.2.0 | Open Management Infrastructure (OMI) - server CIM leggero
scx | 1.6.3 | Provider OMI CIM per metriche delle prestazioni del sistema operativo
apache-cimprov | 1.0.1 | Monitoraggio delle prestazioni del server HTTP Apache per OMI. Installato se viene rilevato il server HTTP Apache.
mysql-cimprov | 1.0.1 | Monitoraggio delle prestazioni del server MySQL per OMI. Installato se viene rilevato il server MySQL/MariaDB.
docker-cimprov | 1.0.0 | Provider Docker per OMI. Installato se viene rilevato Docker.

### <a name="compatibility-with-system-center-operations-manager"></a>Compatibilità con System Center Operations Manager
L'agente OMS per Linux condivide file binari dell'agente con l'agente System Center Operations Manager. Se si installa l'agente OMS per Linux in un sistema attualmente gestito da Operations Manager, i pacchetti OMI e SCX nel computer vengono aggiornati a una versione più recente. In questa versione gli agenti OMS e System Center 2016 - Operations Manager/Operations Manager 2012 R2 per Linux sono compatibili.

> [!NOTE]
> System Center 2012 SP1 e le versioni precedenti non sono attualmente compatibili o supportati con l'agente OMS per Linux.<br>
> Se l'agente OMS per Linux viene installato in un computer attualmente non monitorato da Operations Manager e successivamente si vuole monitorare il computer con Operations Manager, è necessario modificare la [configurazione di OMI](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) prima di individuare il computer. **Questo passaggio *non* è necessario se l'agente Operations Manager viene installato prima dell'agente OMS per Linux.**

### <a name="system-configuration-changes"></a>Modifiche alla configurazione di sistema
Dopo l'installazione dei pacchetti dell'agente OMS per Linux, vengono applicate le modifiche di configurazione aggiuntive seguenti a livello di sistema. Questi elementi vengono rimossi quando viene disinstallato il pacchetto omsagent.

* Viene creato un utente senza privilegi denominato `omsagent` . Si tratta dell'account usato per l'esecuzione del daemon omsagent.
* Viene creato un file "include" suoders in /etc/sudoers.d/omsagent. Questo autorizza omsagent a riavviare i daemon SysLog e omsagent. Se le direttive "include" sudo non sono supportate nella versione di sudo installata, queste voci verranno scritte in /etc/sudoers.
* La configurazione di SysLog viene modificata in modo da inoltrare un sottoinsieme di eventi all'agente. Per altre informazioni, vedere la sezione **Configurazione della raccolta di dati** più avanti.

### <a name="upgrade-from-a-previous-release"></a>Eseguire l'aggiornamento da una versione precedente
L'aggiornamento da versioni precedenti alla 1.0.0-47 è supportato in questa versione. L'esecuzione dell'installazione con il comando `--upgrade` comporta l'aggiornamento di tutti i componenti dell'agente alla versione più recente.

## <a name="install-the-oms-agent-for-linux"></a>Installare l'agente OMS per Linux
L'agente OMS per Linux viene fornito in un bundle di script di shell autoestraente e installabile. Questo bundle contiene pacchetti Debian e RPM per ognuno dei componenti dell'agente e può essere installato direttamente o estratto per recuperare i singoli pacchetti. Vengono forniti un bundle per le architetture x64 e uno per le architetture x86.

### <a name="installing-the-agent"></a>Installazione dell'agente

1. Trasferire il bundle appropriato (x86 o x64) nel computer Linux mediante scp/sftp.
2. Installare il bundle usando l'argomento `--install` o `--upgrade`.

    > [!NOTE]
    > Usare l'argomento `--upgrade` se sono installati pacchetti esistenti, ad esempio quando l'agente System Center Operations Manager per Linux è già installato. Per connettersi a Operations Management Suite durante l'installazione, fornire i parametri `-w <WorkspaceID>` e `-s <Shared Key>`.

### <a name="bundle-command-line-arguments"></a>Argomenti della riga di comando del bundle
```
Options:
  --extract              Extract contents and exit.
  --force                Force upgrade (override version checks).
  --install              Install the package from the system.
  --purge                Uninstall the package and remove all related data.
  --remove               Uninstall the package from the system.
  --restart-deps         Reconfigure and restart dependent service
  --source-references    Show source code reference hashes.
  --upgrade              Upgrade the package in the system.
  --version              Version of this shell bundle.
  --version-check        Check versions already installed to see if upgradable.
  --debug                use shell debug mode.

  -w id, --id id         Use workspace ID <id> for automatic onboarding.
  -s key, --shared key   Use <key> as the shared key for automatic onboarding.
  -d dmn, --domain dmn   Use <dmn> as the OMS domain for onboarding. Optional.
                         default: opinsights.azure.com
                         ex: opinsights.azure.us (for FairFax)
  -p conf, --proxy conf  Use <conf> as the proxy configuration.
                         ex: -p [protocol://][user:password@]proxyhost[:port]
  -a id, --azure-resource id Use Azure Resource ID <id>.
  -m marker, --multi-homing-marker marker
                         Onboard as a multi-homing(Non-Primary) workspace.

  -? | --help            shows this usage text.
```

#### <a name="to-install-and-onboard-directly"></a>Per eseguire installazione e onboarding direttamente
```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="to-install-and-onboard-to-a-workspace-in-us-government-cloud"></a>Per eseguire installazione e onboarding in un'area di lavoro nel cloud US Government
```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

#### <a name="to-install-the-agent-packages-and-onboard-at-a-later-time"></a>Per installare i pacchetti dell'agente ed eseguire l'onboarding in un secondo momento
```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --upgrade
```

#### <a name="to-extract-the-agent-packages-from-the-bundle-without-installing"></a>Per estrarre i pacchetti dell'agente dal bundle senza eseguire l'installazione
```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --extract
```

## <a name="configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway"></a>Configurazione dell'agente per l'uso con un server proxy HTTP o un gateway OMS
L'agente OMS per Linux supporta la comunicazione tramite un server proxy HTTP o HTTPS o un gateway OMS con il servizio OMS.  Sono supportate sia l'autenticazione anonima che quella di base (nome utente/password).  

### <a name="proxy-configuration"></a>Configurazione proxy
Il valore di configurazione proxy ha la sintassi seguente:

`[protocol://][user:password@]proxyhost[:port]`

Proprietà|Descrizione
-|-
Protocol|http o https
user|Nome utente facoltativo per l'autenticazione proxy
password|Password facoltativa per l'autenticazione proxy
proxyhost|Indirizzo o FQDN del server proxy/gateway OMS
port|Numero di porta facoltativo del server proxy/gateway OMS

Ad esempio: `http://user01:password@proxy01.contoso.com:8080`

È possibile specificare il server proxy durante l'installazione o modificando il file di configurazione proxy.conf dopo l'installazione.   

### <a name="specify-proxy-configuration-during-installation"></a>Specificare la configurazione proxy durante l'installazione
L'argomento `-p` o `--proxy` per il bundle di installazione di omsagent specifica la configurazione proxy da usare.

```
sudo sh ./omsagent-1.3.0-1.universal.x64.sh --upgrade -p http://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-the-proxy-configuration-in-a-file"></a>Definire la configurazione proxy in un file
La configurazione proxy può essere impostata nel file: `/etc/opt/microsoft/omsagent/proxy.conf`. Questo file può essere creato o modificato direttamente, ma deve essere leggibile dall'utente omsagent. Ad esempio:
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:8080"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-the-proxy-configuration"></a>Rimozione della configurazione proxy
Per rimuovere una configurazione proxy definita in precedenza e ripristinare la connettività diretta, rimuovere il file proxy.conf:
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```
## <a name="enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager"></a>Consentire all'agente OMS per Linux di inviare segnalazioni a System Center Operations Manager
Seguire questa procedura per configurare l'agente OMS per Linux per inviare segnalazioni a un gruppo di gestione di System Center Operations Manager.  

1. Modificare il file `/etc/opt/omi/conf/omiserver.conf`
2. Assicurarsi che la riga che inizia con **httpsport=** definisca la porta 1270. Ad esempio: `httpsport=1270`
3. Riavviare il server OMI: `sudo /opt/omi/bin/service_control restart`

## <a name="onboarding-with-operations-management-suite"></a>Onboarding con Operations Management Suite
Se non sono stati forniti chiave e ID dell'area di lavoro durante l'installazione del bundle, l'agente deve essere successivamente registrato con Operations Management Suite.

### <a name="onboarding-using-the-command-line"></a>Onboarding usando la riga di comando
Eseguire il comando omsadmin.sh fornendo chiave e ID dell'area di lavoro. Questo comando deve essere eseguito come comando radice (con elevazione sudo):
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a>Onboarding usando un file
1.    Creare il file `/etc/omsagent-onboard.conf`. Il file deve essere leggibile e scrivibile per la radice.
`sudo vi /etc/omsagent-onboard.conf`
2.    Inserire le righe seguenti nel file con la chiave condivisa e l'ID dell'area di lavoro:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  

3.    Eseguire il comando seguente per l'onboarding in OMS: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
4.    Il file verrà eliminato al completamento dell'onboarding

## <a name="manage-omsagent-daemon"></a>Gestire il daemon omsagent
A partire dalla versione 1.3.0-1, viene registrato il daemon omsagent per ogni area di lavoro di cui viene eseguito l'onboarding. Il nome del daemon è *omsagent-\<id-area di lavoro>*.  È possibile usare il comando `/opt/microsoft/omsagent/bin/service_control` per gestire il daemon.

```
sudo sh /opt/microsoft/omsagent/bin/service_control start|stop|restart|enable|disable [<workspace id>]
```

L'ID dell'area di lavoro è un parametro facoltativo. Se viene specificato, il comando verrà applicato solo al daemon specifico dell'area di lavoro.  In caso contrario, verrà applicato a tutti i daemon.


## <a name="agent-logs"></a>Log dell'agente
I log per l'agente OMS per Linux sono disponibili in: `/var/opt/microsoft/omsagent/<workspace id>/log/`. I log per il programma omsconfig (configurazione dell'agente) sono disponibili in: `/var/opt/microsoft/omsconfig/log/`. I log per i componenti OMI e SCX (che forniscono i dati delle metriche delle prestazioni) sono disponibili in:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`

### <a name="log-rotation-configuration"></a>Configurazione di rotazione del log
La configurazione di rotazione del log per omsagent è disponibile in: `/etc/logrotate.d/omsagent-<workspace id>`

Le impostazioni predefinite sono le seguenti:
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-the-oms-agent-for-linux"></a>Disinstallazione dell'agente OMS per Linux
I pacchetti dell'agente possono essere disinstallati usando dpkg o rpm oppure eseguendo il file con estensione sh del bundle con l'argomento `--remove`.  Per rimuovere completamente tutti gli elementi dell'agente OMS per Linux, è inoltre possibile eseguire il file con estensione sh del bundle con l'argomento `--purge`.

### <a name="debian--ubuntu"></a>Debian e Ubuntu
```
> sudo dpkg -P omsconfig
> sudo dpkg -P omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

### <a name="centos-oracle-linux-rhel-and-sles"></a>CentOS, Oracle Linux, RHEL e SLES
```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```
## <a name="troubleshooting"></a>Risoluzione dei problemi

### <a name="issue-unable-to-connect-through-proxy-to-oms"></a>Problema: Impossibile stabilire la connessione tramite proxy a OMS

#### <a name="probable-causes"></a>Possibili cause
* Il proxy specificato durante l'onboarding è errato
* Gli endpoint di servizio di OMS non sono presenti nell'elenco elementi consentiti del data center

#### <a name="resolutions"></a>Soluzioni
1. Eseguire di nuovo l'onboarding nel servizio OMS con l'agente OMS per Linux usando il comando seguente con l'opzione `-v` abilitata. In questo modo l'output dettagliato dell'agente si connette tramite proxy al servizio OMS.
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Vedere la sezione Configurazione dell'agente per l'uso con un server proxy HTTP(#configuring the-agent-for-use-with-a-http-proxy-server) per verificare di aver configurato correttamente l'agente per la comunicazione tramite un server proxy.    
* Controllare che gli endpoint di servizio di OMS seguenti siano presenti nell'elenco elementi consentiti:

    |Risorsa agente| Porte |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Porta 443|   
    |*.oms.opinsights.azure.com | Porta 443|   
    |ods.systemcenteradvisor.com | Porta 443|   
    |*.blob.core.windows.net/ | Porta 443|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problema: Viene visualizzato un errore 403 durante il tentativo di onboarding

#### <a name="probable-causes"></a>Possibili cause
* Data e ora nel server Linux non sono corrette
* L'ID e la chiave dell'area di lavoro usati non sono corretti

#### <a name="resolution"></a>Risoluzione

1. Controllare l'ora nel server Linux con il comando date. Se l'ora è sfasata di +/- 15 minuti rispetto all'ora corrente, l'onboarding ha esito negativo. Per risolvere il problema, aggiornare la data e/o il fuso orario del server Linux.
Novità: la versione più recente dell'agente OMS per Linux invia ora una notifica se lo sfasamento di orario causa l'errore di onboarding. Eseguire di nuovo l'onboarding usando le istruzioni corrette per chiave e ID dell'area di lavoro

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problema: Viene visualizzato un errore 404 o 500 nel file di log subito dopo l'onboarding
Si tratta di un problema noto che si verifica durante il primo caricamento dei dati di Linux in un'area di lavoro di OMS. Questo non influisce sui dati inviati o sull'esperienza d'uso del servizio.

### <a name="issue--you-are-not-seeing-any-data-in-the-oms-portal"></a>Problema: Non vengono visualizzati dati nel portale di OMS

#### <a name="probable-causes"></a>Possibili cause

- Il caricamento nel servizio OMS ha avuto esito negativo
- La connessione al servizio OMS è bloccata
- Viene eseguito il backup dei dati dell'agente OMS per Linux

#### <a name="resolutions"></a>Soluzioni
1. Controllare che l'onboarding del servizio OMS sia avvenuto correttamente verificando la presenza del file seguente: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Eseguire di nuovo l'onboarding usando le istruzioni della riga di comando `omsadmin.sh`
3. Se si usa un proxy, vedere i passaggi di risoluzione del proxy indicati in precedenza.
4. In alcuni casi, quando l'agente OMS per Linux non può comunicare con il servizio OMS, i dati dell'agente vengono inseriti in una coda fino a raggiungere le dimensioni intere del buffer, ovvero 50 MB. L'agente OMS per Linux deve essere riavviato usando il comando seguente: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`.
> [!NOTE]
> Il problema è stato risolto nell'agente versione 1.1.0-28 e versioni successive.

