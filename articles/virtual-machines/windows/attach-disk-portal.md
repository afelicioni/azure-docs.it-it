---
title: Collegare un disco dati a una macchina virtuale Windows | Microsoft Docs
description: Come collegare un disco dati nuovo o esistente a una macchina virtuale Windows nel portale di Azure tramite il modello di distribuzione di Resource Manager.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3790fc59-7264-41df-b7a3-8d1226799885
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 3a3dce590013187e4136a65a47a10c9532321b1e
ms.lasthandoff: 04/03/2017


---
# <a name="how-to-attach-a-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Collegare un disco dati a una macchina virtuale Windows nel portale di Azure
Questo articolo illustra come collegare dischi nuovi o esistenti a una macchina virtuale Windows tramite il portale di Azure. È possibile anche [collegare un disco dati a una macchina virtuale Linux nel portale di Azure](../linux/attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Prima di procedere, rivedere i suggerimenti seguenti:

* La dimensione della macchina virtuale controlla il numero di dischi dati che è possibile collegare. Per informazioni dettagliate, vedere [Dimensioni delle macchine virtuali](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Per utilizzare l'archiviazione Premium, è necessario utilizzare una macchina virtuale della serie DS o serie GS. È possibile utilizzare dischi dagli account di archiviazione sia Premium che Standard con queste macchine virtuali. L’archiviazione Premium è disponibile solo in determinate aree geografiche. Per ulteriori informazioni, vedere [Archiviazione Premium: archiviazione ad alte prestazioni per carichi di lavoro delle macchine virtuali di Azure](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Per un nuovo disco, non è necessario crearlo prima perché Azure lo crea quando lo si collega.
* Per un disco esistente, il file .vhd deve essere disponibile in un account di archiviazione di Azure. È possibile usare un .vhd già esistente se non è collegato a un'altra macchina virtuale o caricare il proprio file .vhd nell'account di archiviazione.

È anche possibile [collegare un disco dati usando Powershell](attach-disk-ps.md).



## <a name="find-the-virtual-machine"></a>Trovare la macchina virtuale
1. Accedere al [portale di Azure](https://portal.azure.com/).
2. Scegliere **Macchine virtuali**dal menu Hub.
3. Selezionare la macchina virtuale dall'elenco.
4. Nel pannello Macchine virtuali di **Essentials** fare clic su **Dischi**.
   
    ![Aprire le impostazioni del disco](./media/attach-disk-portal/find-disk-settings.png)

Continuare seguendo le istruzioni per il collegamento di un [nuovo disco](#option-1-attach-a-new-disk) o un [disco esistente](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Opzione 1: Connettere e inizializzare un nuovo disco
1. Nel pannello **Dischi** fare clic su **Collega nuovo**.
2. Esaminare le impostazioni predefinite, aggiornare se necessario e quindi fare clic su **OK**.
   
   ![Esaminare le impostazioni del disco](./media/attach-disk-portal/attach-new.png)
3. Dopo che Azure crea il disco e lo collega alla macchina virtuale, il nuovo disco viene elencato nella sezione Impostazioni disco della macchina virtuale in **Dischi dati**.

### <a name="initialize-a-new-data-disk"></a>Inizializzare un nuovo disco dati

1. Connettersi alla macchina virtuale. Per istruzioni, vedere [Come connettersi e accedere a una macchina virtuale di Azure che esegue Windows Server](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Dopo aver eseguito l'accesso alla macchina virtuale, aprire **Server Manager**. Nel riquadro sinistro fare clic su **Servizi file e archiviazione**.
   
    ![Avviare Server Manager](./media/attach-disk-portal/fileandstorageservices.png)
3. Espandere il menu e selezionare **Dischi**.
4. La sezione **dischi** contiene un elenco dei dischi. Nella maggior parte dei casi, avrà disco 0, disco 1 e disco 2. Il disco 0 è il disco del sistema operativo, il disco 1 è il disco temporaneo e il disco 2 è il disco dati che è stato appena connesso alla VM. Il nuovo disco dati elencherà la partizione come **sconosciuta**. Fare clic con il pulsante destro del mouse sul disco e scegliere **Inizializza**.
5. Si riceverà una notifica che tutti i dati verranno cancellati quando viene inizializzato il disco. Fare clic su **Sì** per accettare il messaggio di avviso e inizializzare il disco. Una volta completata l'operazione, la partizione verrà elencata come **GPT**. Fare di nuovo clic con il pulsante destro del mouse sul disco e scegliere **Nuovo volume**.
6. Completare la procedura guidata usando i valori predefiniti. Al termine della procedura guidata, nella sezione **Volumi** verrà visualizzato il nuovo volume. Il disco sarà ora online e pronto per l'archiviazione di dati.

    ![Inizializzazione del volume completata](./media/attach-disk-portal/newvolumecreated.png)


## <a name="option-2-attach-an-existing-disk"></a>Opzione 2: Collegare un disco esistente
1. Nel pannello **Dischi** fare clic su **Collega esistente**.
2. In **Collega un disco esistente** fare clic su **File VHD**.
   
   ![Collegare un disco esistente](./media/attach-disk-portal/attach-existing.png)
3. In **Account di archiviazione**, selezionare l'account e un contenitore che contiene il file con estensione vhd.
   
   ![Individuare il percorso di un VHD](./media/attach-disk-portal/find-storage-container.png)
4. Selezionare il file con estensione vhd.
5. In **Collega un disco esistente** il file appena selezionato è elencato in **File VHD**. Fare clic su **OK**.
6. Dopo che Azure collega il disco alla macchina virtuale, esso viene elencato nella sezione Impostazioni disco della macchina virtuale in **Dischi dei dati**.



## <a name="use-trim-with-standard-storage"></a>Usare TRIM con l'archiviazione Standard

Se si usa l'archiviazione Standard (HDD), è consigliabile abilitare TRIM. TRIM ignora i blocchi inutilizzati nel disco facendo in modo che venga addebitato solo lo spazio di archiviazione usato effettivamente. In questo modo è possibile risparmiare sui costi quando si creano file di grandi dimensioni per poi eliminarli. 

Per verificare l'impostazione TRIM, è possibile eseguire questo comando. Aprire un prompt dei comandi nella macchina virtuale Windows e digitare:

```
fsutil behavior query DisableDeleteNotify
```

Se il comando restituisce 0, l'impostazione TRIM è abilitata correttamente. Se restituisce 1, eseguire questo comando per abilitare TRIM:
```
fsutil behavior set DisableDeleteNotify 0
```
                
Dopo l'eliminazione di dati dal disco è possibile verificare che le operazioni TRIM avvengano correttamente eseguendo la deframmentazione con TRIM:

```
defrag.exe <volume:> -l
```

## <a name="next-steps"></a>Passaggi successivi
Se l'applicazione deve usare l'unità D: per archiviare i dati, è possibile [modificare la lettera di unità del disco temporaneo di Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).


