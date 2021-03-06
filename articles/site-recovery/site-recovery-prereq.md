---
title: Prerequisiti per la replica in Azure con Azure Site Recovery | Microsoft Docs
description: Questo articolo riepiloga i prerequisiti per la replica di macchine virtuali e computer fisici in Azure tramite il servizio Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: rajanaki
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 5ff598af73b6be727753ecac5b99f28bae19a417
ms.lasthandoff: 04/17/2017

---

#  <a name="prerequisites-for-replication-to-azure-by-using-azure-site-recovery"></a>Prerequisiti per la replica in Azure con Azure Site Recovery


Il servizio Azure Site Recovery contribuisce alla strategia di continuità aziendale e ripristino di emergenza orchestrando la replica dei server fisici locali e delle macchine virtuali sul cloud (Azure) o in un data center secondario. In caso di interruzioni nella località primaria, è possibile eseguire il failover a una località secondaria per mantenere disponibili app e carichi di lavoro. Sarà possibile tornare alla località primaria quando sarà di nuovo operativa. Per altre informazioni su Site Recovery, vedere [Che cos'è Site Recovery?](site-recovery-overview.md).

In questo articolo vengono riepilogati i prerequisiti necessari per avviare la replica di Site Recovery in Azure.

È possibile inserire commenti nella parte inferiore di questo articolo oppure porre domande tecniche nel [forum sui servizi di ripristino di Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="azure-requirements"></a>Requisiti di Azure

**Requisito** | **Dettagli**
--- | ---
**Account di Azure** | Account [Microsoft Azure](http://azure.microsoft.com/) .<br/><br/> È possibile iniziare con una [versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/).
**Servizio Site Recovery** | Per altre informazioni sui prezzi di Site Recovery, vedere [Prezzi di Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Archiviazione di Azure** | È necessario un account di archiviazione di Azure per archiviare i dati replicati. L'account deve essere nella stessa area dell'insieme di credenziali di servizi di ripristino. I dati replicati vengono memorizzati in Archiviazione di Azure e le macchine virtuali di Azure vengono create quando si verifica il failover.<br/><br/> A seconda del modello di risorsa da usare per le macchine virtuali di Azure di cui si esegue il failover, l'account deve essere configurato con il [modello di Azure Resource Manager](../storage/storage-create-storage-account.md) o con il [modello di distribuzione classica](../storage/storage-create-storage-account-classic-portal.md).<br/><br/>È possibile usare l'[archiviazione con ridondanza geografica](../storage/storage-redundancy.md#geo-redundant-storage) o locale. È consigliabile usare l'archiviazione con ridondanza geografica per una maggiore resilienza dei dati in caso di interruzione del servizio a livello di area o se non è possibile recuperare l'area primaria.<br/><br/> È possibile usare l'archiviazione Standard o Premium. [Archiviazione premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) viene in genere usata per le macchine virtuali che richiedono un livello di prestazioni di I/O costantemente elevato e bassa latenza per ospitare carichi di lavoro con numerose operazioni di I/O. Se si usa Archiviazione Premium per i dati replicati, sarà necessario anche un account di archiviazione standard per l'archiviazione dei log di replica in cui vengono acquisite le modifiche in corso ai dati locali.<br/><br/>
**Limiti di archiviazione** | Non è possibile spostare gli account di archiviazione usati da Site Recovery tra gruppi di risorse nella stessa sottoscrizione o in diverse sottoscrizioni.<br/><br/> La replica in account di archiviazione Premium non è attualmente supportata in India centrale e India meridionale.
**Rete di Azure** | È necessaria una rete di Azure a cui si connetteranno le macchine virtuali dopo il failover. Deve trovarsi nella stessa area dell'insieme di credenziali dei servizi di ripristino.<br/><br/> Nel portale di Azure è possibile creare le reti con i [modelli di Resource Manager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) o di [distribuzione classica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).<br/><br/> In caso di replica da System Center Virtual Machine Manager ad Azure, si configurerà il mapping di rete tra le reti di macchine virtuali Virtual Machine Manager e le reti di Azure in modo da garantire che le macchine virtuali di Azure si connettano alle reti appropriate dopo il failover.
**Limitazioni di rete** | Non è possibile spostare gli account di rete usati da Site Recovery tra gruppi di risorse nella stessa sottoscrizione o in diverse sottoscrizioni.
**Mapping di rete** | Se si esegue la replica di macchine virtuali Hyper-V in cloud Virtual Machine Manager, è necessario configurare il mapping di rete in modo che le macchine virtuali di Azure siano connesse alle reti appropriate se vengono create dopo il failover.

>[!NOTE]
>Nelle sezioni di seguito vengono descritti i prerequisiti per i vari componenti nell'ambiente del cliente. Per altre informazioni sul supporto per configurazioni specifiche, leggere la [matrice di supporto](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-virtual-machines-or-physical-windows-or-linux-servers-to-azure"></a>Ripristino di emergenza in Azure di macchine virtuali VMware o server Windows/Linux fisici
Di seguito sono elencati i componenti necessari per il ripristino di emergenza di macchine virtuali VMware o server di Windows o Linux fisici oltre a quelli indicati nei [requisiti di Azure](#Azure requirements).


### <a name="configuration-server-or-additional-process-server-you-will-need-to-set-up-an-on-premises-machine-as-the-configuration-server-to-coordinate-communications-between-the-on-premises-site-and-azure-and-to-manage-data-replication-brbr"></a>**Server di configurazione o di elaborazione aggiuntiva**: è necessario configurare un computer locale come server di configurazione per coordinare le comunicazioni tra il sito locale e Azure e per gestire la replica dei dati. <br></br>

1. **Host di VMware vCenter o vSphere**

| **Componente** | **Requisiti** |
| --- | --- |
| **vSphere** | Uno o più hypervisor VMware vSphere.<br/><br/>Gli hypervisor devono eseguire vSphere versione 6.0, 5.5 o 5.1 con gli aggiornamenti più recenti.<br/><br/>È consigliabile che gli host di vSphere e i server vCenter si trovino nella stessa rete del server di elaborazione. Si tratta della rete in cui si trova il server di configurazione, a meno che non sia stato configurato un server di elaborazione dedicato. |
| **vCenter** | È consigliabile distribuire un server VMware vCenter per gestire gli host vSphere. Deve eseguire vCenter versione 6.0 o 5.5 con gli aggiornamenti più recenti.<br/><br/>**Limitazione**: Site Recovery non supporta il l'incrocio tra vCenter e vMotion. Neanche l'archiviazione DRS e l'archiviazione vMotion sono supportate nella macchina virtuale di destinazione master in seguito a un'operazione di nuova protezione.||

1. **Prerequisiti dei computer replicati**


| **Componente** | **Requisiti** |
| --- | --- |
| **Macchine virtuali VMware locali** | Nelle VM replicate devono essere installati e in esecuzione gli strumenti VMware.<br/><br/> Le VM devono essere conformi ai [prerequisiti di Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) per la creazione di VM di Azure.<br/><br/>La capacità dei singoli dischi nei computer protetti non deve superare 1,023 GB. <br/><br/>Minimo 2 GB di spazio disponibile sull'unità di installazione richiesto per l'installazione del componente.<br/><br/>La porta 20004 deve essere aperta nel firewall locale della VM se si desidera abilitare la coerenza fra più VM.<br/><br/>I nomi delle macchine devono contenere da 1 a 63 caratteri, ovvero lettere, numeri e trattini. Il nome deve iniziare e terminare con una lettera o un numero. È possibile modificare il nome di Azure dopo aver abilitato la replica per una macchina.<br/><br/> |
| **Computer Windows (fisico o VMware)** | Nel computer deve essere in esecuzione un sistema operativo a 64 bit supportato: Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 con SP1 o versioni successive.<br/><br/> Il sistema operativo deve essere installato nell'unità C e il disco del sistema operativo deve essere un disco di base di Windows, non un disco dinamico. Il disco dati può essere dinamico.<br/><br/>|
| **Computer Linux** (fisico o VMware) | È necessario un sistema operativo a 64 bit supportato: Red Hat Enterprise Linux 6.7, 6.8, 7.1 o 7.2; Centos 6.5, 6.6, 6.7, 6.8, 7.0, 7.1, o 7.2; Oracle Enterprise Linux 6.4 o 6.5 che esegue il kernel compatibile Red Hat o Unbreakable Enterprise Kernel Release 3 (UEK3), SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4.<br/><br/>I file /etc/hosts nei computer protetti devono contenere le voci che eseguono il mapping del nome host locale agli indirizzi IP associati a tutte le schede di rete.<br/><br/>Per connettersi a una macchina virtuale di Azure che esegue Linux dopo il failover usando un client Secure Shell (SSH), accertarsi che il servizio Secure Shell nel computer protetto sia impostato per l'avvio automatico all'avvio del sistema e che le regole del firewall permettano una connessione SSH a tale computer.<br/><br/>Nome host, punti di montaggio, nomi dispositivo e percorsi di sistema di Linux e nomi file (ad esempio /etc/, /usr) dovranno essere specificati solo con caratteri dell'alfabeto latino.<br/><br/>Le seguenti directory (se impostate come partizioni, file-system separati) devono essere tutte nello stesso disco (il disco del sistema operativo) nel server di origine:   / (root), /boot, /usr, /usr/local, /var, /ecc.<br/><br/>Le funzionalità XFS v5, ad esempio i checksum di metadati, non sono attualmente supportate da ASR nei file System XFS. Assicurarsi che i file System XFS non usino alcuna funzionalità v5. È possibile usare l'utilità xfs_info per controllare il superblocco XFS per la partizione. Se ftype è impostato su 1, le funzionalità XFSv5 sono in uso.<br/><br/>Nei server Red Hat Enterprise Linux 7 e CentOS 7 l'utilità lsof deve essere installata e disponibile.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager"></a>Ripristino di emergenza in Azure di macchine virtuali Hyper-V (non Virtual Machine Manager)

Di seguito sono elencati i componenti necessari per il ripristino di emergenza di macchine virtuali Hyper-V in cloud Virtual Machine Manager oltre a quelli indicati nei [requisiti di Azure](#Azure requirements).

| **Prerequisito** | **Dettagli** |
| --- | --- |
| **Host Hyper-V** |Uno o più server locali in cui è in esecuzione Windows Server 2012 R2 con tutti gli aggiornamenti più recenti e il ruolo Hyper-V abilitato o Microsoft Hyper-V Server 2012 R2.<br/><br/>Il server Hyper-V deve contenere una o più macchine virtuali.<br/><br/>I server Hyper-V devono essere connessi a Internet, in modo diretto o tramite proxy.<br/><br/>Nei server Hyper-V devono essere state installate le correzioni indicate nell'articolo [KB2961977](https://support.microsoft.com/kb/2961977) della Microsoft Knowledge Base.
|**Provider e agente**| Durante la distribuzione di Azure Site Recovery verrà installato il provider di Azure Site Recovery. Durante l'installazione del provider verrà installato anche l'agente di Servizi di ripristino di Azure in ogni server Hyper-V che esegue le macchine virtuali da proteggere. <br/><br/>In tutti i server Hyper-V in un insieme di credenziali di Site Recovery devono essere installate le stesse versioni del provider e dell'agente.<br/><br/>Il provider dovrà connettersi ad Azure Site Recovery tramite Internet. Il traffico può essere inviato direttamente oppure tramite un proxy. Si noti che il proxy basato su HTTPS non è supportato. Il server proxy deve consentire l'accesso a: <br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Se nel server sono disponibili regole firewall basate sull'indirizzo IP, assicurarsi che le regole consentano la comunicazione con Azure.<br/><br/> Consentire gli [intervalli IP del data center di Azure ](https://www.microsoft.com/download/confirmation.aspx?id=41653) e la porta HTTPS (443).<br/><br/> Consentire gli intervalli di indirizzi IP per l'area di Azure della sottoscrizione e per gli Stati Uniti occidentali (usati per il controllo di accesso e la gestione delle identità).


## <a name="disaster-recovery-of-hyper-v-virtual-machines-in-virtual-machine-manager-clouds-to-azure"></a>Ripristino di emergenza in Azure di macchine virtuali Hyper-V in cloud Virtual Machine Manager

Di seguito sono elencati i componenti necessari per il ripristino di emergenza di macchine virtuali Hyper-V in cloud Virtual Machine Manager oltre a quelli indicati nei [requisiti di Azure](#Azure requirements).

| **Prerequisito** | **Dettagli** |
| --- | --- |
| **Virtual Machine Manager** |Uno o più server Virtual Machine Manager in esecuzione su **System Center 2012 R2 o versioni successive**. Ogni server Virtual Machine Manager deve essere configurato con uno o più cloud. <br/><br/>Ogni cloud deve contenere:<br>- Uno o più gruppi di host Virtual Machine Manager.<br/>- Uno o più cluster o server host Hyper-V in ogni gruppo host.<br/><br/>Per altre informazioni sull'impostazione di cloud Virtual Machine Manager, vedere [Come creare un cloud in VMM 2012](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx). |
| **Hyper-V** |I server host Hyper-V devono eseguire almeno Windows Server 2012 R2 con ruolo Hyper-V Microsoft Hyper-V Server 2012 R2 e disporre degli ultimi aggiornamenti installati.<br/><br/> Il server Hyper-V deve contenere una o più macchine virtuali.<br/><br/> Il server host Hyper-V o il cluster che include le macchine virtuali da replicare deve essere gestito in un cloud Virtual Machine Manager.<br/><br/>I server Hyper-V devono essere connessi a Internet, in modo diretto o tramite proxy.<br/><br/>Nei server Hyper-V è necessario avere installato le correzioni specificate nell'articolo [2961977](https://support.microsoft.com/kb/2961977).<br/><br/>I server host Hyper-V richiedono l'accesso a Internet per la replica dei dati in Azure. |
| **Provider e agente** |Durante la distribuzione di Azure Site Recovery vengono installati il provider di Azure Site Recovery nel server Virtual Machine Manager e l'agente di Servizi di ripristino negli host Hyper-V. Il provider e l'agente devono connettersi ad Azure tramite Internet, in modo diretto o attraverso un proxy. Un proxy basato su HTTPS non è supportato. Il server proxy nel server Virtual Machine Manager e gli host Hyper-devono poter accedere a: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Se nel server Virtual Machine Manager sono disponibili regole firewall basate sull'indirizzo IP, assicurarsi che le regole consentano la comunicazione con Azure.<br/><br/> Consentire gli [intervalli IP del data center di Azure ](https://www.microsoft.com/download/confirmation.aspx?id=41653) e la porta HTTPS (443).<br/><br/>Consentire gli intervalli di indirizzi IP per l'area di Azure della sottoscrizione e per gli Stati Uniti occidentali (usati per il controllo di accesso e la gestione delle identità).<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Prerequisiti dei computer replicati
| **Componente** | **Dettagli** |
| --- | --- |
| **Macchine virtuali protette** | Site Recovery supporta tutti i sistemi operativi supportati da [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).<br/><br/>Le VM devono essere conformi ai [prerequisiti di Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) per la creazione di VM di Azure. I nomi delle macchine devono contenere da 1 a 63 caratteri, ovvero lettere, numeri e trattini. Il nome deve iniziare e terminare con una lettera o un numero. <br/><br/>È possibile modificare il nome dopo aver abilitato la replica per la VM. <br/><br/> La capacità dei singoli dischi nei computer protetti non deve superare 1,023 GB. Una macchina virtuale può avere fino a 16 dischi (quindi fino a 16 TB).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-virtual-machines-in-virtual-machine-manager-clouds-to-a-customer-owned-site"></a>Ripristino di emergenza in un sito del cliente di macchine virtuali Hyper-V in cloud Virtual Machine Manager

Di seguito sono elencati i componenti necessari per il ripristino di emergenza in siti dei clienti di macchine virtuali Hyper-V in cloud Virtual Machine Manager oltre a quelli indicati nei [requisiti di Azure](#Azure requirements).

| **Componenti** | **Dettagli** |
| --- | --- |
| **Virtual Machine Manager** |  È consigliabile distribuire un server Virtual Machine Manager nel sito primario e un server Virtual Machine Manager nel sito secondario.<br/><br/> È possibile [eseguire la replica tra cloud in un unico server VMM](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). A tale scopo, sono necessari almeno due cloud configurati sul server Virtual Machine Manager.<br/><br/> I server Virtual Machine Manager devono eseguire almeno System Center 2012 SP1 con gli aggiornamenti più recenti.<br/><br/> Ogni server Virtual Machine Manager deve essere configurato con uno o più cloud. In tutti i cloud deve essere impostato il profilo della capacità Hyper-V. <br/><br/>I cloud devono contenere uno o più gruppi di host Virtual Machine Manager. Per altre informazioni sull'impostazione di cloud Virtual Machine Manager, vedere [Preparare la distribuzione di Azure Site Recovery](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Hyper-V** | I server Hyper-V devono eseguire almeno Windows Server 2012 con ruolo Hyper-V e con gli ultimi aggiornamenti installati.<br/><br/> Il server Hyper-V deve contenere una o più macchine virtuali.<br/><br/>  I server host Hyper-V devono trovarsi nei gruppi host disponibili nei cloud VMM primario e secondario.<br/><br/> Se si esegue Hyper-V in un cluster in Windows Server 2012 R2 è consigliabile installare l'[aggiornamento 2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Se si esegue Hyper-V in un cluster basato su indirizzi IP statici in Windows Server 2012, il broker del cluster non viene creato automaticamente. È necessario configurare manualmente il broker del cluster. Per altre informazioni sul broker del cluster, vedere [Configurare la replica da cluster a cluster con il ruolo del broker di replica](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Provider** | Durante la distribuzione di Site Recovery, installare il provider di Azure Site Recovery in server Virtual Machine Manager. Il provider comunica con Site Recovery su HTTPS 443 per coordinare la replica. La replica dei dati viene eseguita tra il server Hyper-V primario e quello secondario attraverso la rete LAN o una connessione VPN.<br/><br/> Il provider in esecuzione nel server Virtual Machine Manager deve poter accedere agli URL seguenti:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Il provider deve consentire la comunicazione del firewall dai server Virtual Machine Manager agli [intervalli IP dei data center di Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) e il protocollo HTTPS (443). |


## <a name="url-access"></a>Accesso a URL
Gli URL seguenti dovranno essere disponibili dai server host Hyper-V, VMware e VMM.

|**URL** | **Da VMM a VMM** | **Da VMM ad Azure** | **Da Hyper-V ad Azure** | **Da VMware ad Azure** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | Consenti | Consenti | Consenti | Consenti |
|``*.backup.windowsazure.com`` | Facoltativo | Consenti | Consenti | Consenti |
|``*.hypervrecoverymanager.windowsazure.com`` | Consenti | Consenti | Consenti | Consenti |
|``*.store.core.windows.net`` | Consenti | Consenti | Consenti | Consenti |
|``*.blob.core.windows.net`` | Facoltativo | Consenti | Consenti | Consenti |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Facoltativo | Facoltativo | Facoltativo | Consentire download SQL |
|``time.windows.com`` | Consenti | Consenti | Consenti | Consenti|
|``time.nist.gov`` | Consenti | Consenti | Consenti | CONSENTI |

