---
title: Configurare l&quot;ambiente di origine (VMware in Azure) | Documentazione Microsoft
description: Questo articolo descrive come configurare l&quot;ambiente locale per avviare la replica di macchine virtuali VMware in Azure.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/05/2017
ms.author: anoopkv
ms.translationtype: Human Translation
ms.sourcegitcommit: eccc927550aef4c9cd71ffad64d2eddedb74326e
ms.openlocfilehash: 18627223bdb4f0986f07f17233ce7daf29cb7dd9
ms.contentlocale: it-it
ms.lasthandoff: 02/13/2017

---

# <a name="set-up-the-source-environment-vmware-to-azure"></a>Configurare l'ambiente di origine (VMware in Azure)
> [!div class="op_single_selector"]
> * [Macchine virtuali VMware](./site-recovery-set-up-vmware-to-azure.md)
> * [Server fisici](./site-recovery-set-up-physical-to-azure.md)

Questo articolo descrive come configurare l'ambiente locale per avviare la replica di macchine virtuali in esecuzione su VMware in Azure.

## <a name="prerequisites"></a>Prerequisiti

Nell'articolo si presuppone che siano già stati creati gli elementi seguenti:
- Insieme di credenziali di Servizi di ripristino nel [portale di Azure](http://portal.azure.com "Portale di Azure").
- Account dedicato in VMware vCenter che può essere usato per l'[individuazione automatica](./site-recovery-vmware-to-azure.md#vmware-account-permissions).
- Macchina virtuale nella quale installare il server di configurazione.

## <a name="configuration-server-minimum-requirements"></a>Requisiti minimi per il server di configurazione
Il software per il server di configurazione deve essere distribuito in una macchina virtuale VMware a disponibilità elevata. La tabella seguente elenca i requisiti minimi hardware, software e di rete per un server di configurazione.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> I server proxy basati su HTTPS non sono supportati dal server di configurazione.

## <a name="choose-your-protection-goals"></a>Scegliere gli obiettivi della protezione

1. Nel portale di Azure accedere al pannello degli insiemi di credenziali di **Servizi di ripristino** e selezionare l'insieme di credenziali.
2. Nel menu delle risorse dell'insieme di credenziali passare ad **Attività iniziali** > **Site Recovery** > **Passaggio 1: Preparare l'infrastruttura** > **Obiettivo di protezione**.

    ![Scegliere gli obiettivi](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. In **Obiettivo di protezione** selezionare **In Azure** e scegliere **Sì, con VMware vSphere Hypervisor**. Fare quindi clic su **OK**.

    ![Scegliere gli obiettivi](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Configurare l'ambiente di origine
L'impostazione dell'ambiente di origine prevede due attività principali:

- Installare e registrare un server di configurazione con Site Recovery.
- Individuare le macchine virtuali locali connettendo Site Recovery agli host VMware vCenter o vSphere EXSi.

### <a name="step-1-install-and-register-a-configuration-server"></a>Passaggio 1: Installare e registrare un server di configurazione

1. Fare clic su **Passaggio 1: Preparare l'infrastruttura** > **Origine**. Se non è disponibile un server di configurazione, in **Prepara origine** fare clic su **+Server di configurazione** per aggiungerne uno.

    ![Impostare l'origine](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. Nel pannello **Aggiungi server** verificare che **Server di configurazione** sia visualizzato in **Tipo di server**.
4. Scaricare il file di installazione per l'Installazione unificata di Azure Site Recovery.
5. Scaricare la chiave di registrazione dell'insieme di credenziali, Per eseguire l'installazione unificata, è necessaria la chiave di registrazione. La chiave è valida per cinque giorni dal momento in cui viene generata.

    ![Impostare l'origine](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. Nel computer usato come server di configurazione, eseguire l'**installazione unificata di Azure Site Recovery** per installare il server di configurazione, il server di elaborazione e il server master di destinazione.

#### <a name="run-azure-site-recovery-unified-setup"></a>Eseguire l'Installazione unificata di Azure Site Recovery

> [!TIP]
> La registrazione del server di configurazione non riesce se l'ora nel computer differisce di oltre cinque minuti dall'ora locale. Sincronizzare l'ora del sistema con un [server di riferimento ora](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) prima di iniziare l'installazione.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Il server di configurazione può essere installato tramite la riga di comando. Per altre informazioni, vedere l'argomento relativo all'[installazione del server di configurazione con gli strumenti da riga di comando](http://aka.ms/installconfigsrv).

#### <a name="add-the-vmware-account-for-automatic-discovery"></a>Aggiungere l'account VMware per l'individuazione automatica

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a>Passaggio 2 - Aggiungere un vCenter
Per consentire ad Azure Site Recovery di individuare macchine virtuali in esecuzione nell'ambiente locale, è necessario connettere il server VMware vCenter o gli host vSphere ESXi a Site Recovery.

Selezionare **+vCenter** per avviare la connessione di un server VMware vCenter o di un host VMware vSphere ESXi.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Problemi comuni
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Passaggi successivi
[Configurare l'ambiente di destinazione](./site-recovery-prepare-target-vmware-to-azure.md) in Azure.

