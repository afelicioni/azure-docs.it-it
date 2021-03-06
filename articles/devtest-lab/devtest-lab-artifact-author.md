---
title: Creare elementi personalizzati per le macchine virtuali di lab di sviluppo/test | Microsoft Docs
description: Informazioni su come creare i propri elementi per l&quot;uso nei Lab di sviluppo/test
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 32dcdc61-ec23-4a01-b731-78c029ea5316
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: e86af029422340bdfa38ba233f0ed61f6f1d8ca2
ms.lasthandoff: 03/17/2017


---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a>Creare elementi personalizzati per le macchine virtuali di lab di sviluppo e test
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Panoramica
**Elementi** vengono utilizzati per distribuire e configurare l'applicazione dopo il provisioning di una macchina virtuale. Un elemento è costituito da un file di definizione dell’elemento e altri file di script archiviati in una cartella in un archivio git. I file di definizione dell’elemento sono costituiti da JSON ed espressioni che è possibile utilizzare per specificare gli elementi da installare in una macchina virtuale. Ad esempio, è possibile definire il nome dell’elemento, il comando da eseguire e i parametri che vengono resi disponibili quando si esegue il comando. È possibile fare riferimento ad altri file di script all'interno del file di definizione dell'elemento in base al nome.

## <a name="artifact-definition-file-format"></a>Formato del file di definizione dell’elemento
L'esempio seguente illustra le sezioni che compongono la struttura di base di un file di definizione.

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2015-01-01/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| Nome dell'elemento | Obbligatorio? | Descrizione |
| --- | --- | --- |
| $schema |No |Percorso del file di schema JSON che aiuta a testare la validità del file di definizione. |
| title |Sì |Nome dell'elemento visualizzato nel lab. |
| Descrizione |Sì |Descrizione dell'elemento visualizzato nel lab. |
| iconUri |No |URI dell'icona visualizzato nel lab. |
| targetOsType |Sì |Sistema operativo della macchina virtuale in cui verrà installato l'elemento. Opzioni supportate: Windows e Linux. |
| parameters |No |Valori forniti quando viene eseguito il comando di installazione dell’elemento in un computer. Ciò consente di personalizzare l'elemento. |
| runCommand |Sì |Il comando di installazione dell’elemento che viene eseguito in una macchina virtuale. |

### <a name="artifact-parameters"></a>Parametri dell'elemento
Nella sezione dei parametri del file di definizione è possibile specificare i valori che un utente può immettere durante l’installazione di un elemento. È possibile fare riferimento a questi valori nel comando di installazione dell'elemento.

I parametri vengono definiti con la struttura seguente:

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Nome dell'elemento | Obbligatorio? | Descrizione |
| --- | --- | --- |
| type |Sì |Tipo di valore del parametro. Vedere di seguito l'elenco dei tipi consentiti: |
| displayName |Sì |Nome del parametro che viene visualizzato a un utente nel lab. | |
| Descrizione |Sì |Descrizione del parametro che viene visualizzato nel lab. |

I tipi consentiti sono:

* string: tutte le stringhe JSON valide
* int: tutti i valori integer JSON validi
* bool: tutti i valori booleani JSON validi
* array: tutte le matrici JSON valide

## <a name="artifact-expressions-and-functions"></a>Espressioni e funzioni dell’elemento
È possibile utilizzare espressioni e funzioni per la creazione del comando di installazione dell'elemento.
Le espressioni sono racchiuse tra parentesi quadre ([ e ]) e vengono valutate al momento dell’installazione dell’elemento. Le espressioni possono trovarsi in qualsiasi punto in un valore stringa JSON e restituiscono sempre un altro valore JSON. Se è necessario usare una stringa letterale che inizia con una parentesi quadra [, usare due parentesi quadre [[.
In genere, si usano espressioni con funzioni per costruire un valore. Proprio come in JavaScript, le chiamate di funzione sono formattate come functionName(arg1,arg2,arg3).

Nell'elenco seguente vengono riportate le funzioni comuni.

* parameters(parameterName) - restituisce un valore di parametro fornito quando viene eseguito il comando dell'elemento.
* concat(arg1,arg2,arg3, …..) -     Combina più valori di stringa. Questa funzione può accettare qualsiasi numero di argomenti.

Nell'esempio seguente viene illustrato come utilizzare espressioni e funzioni per costruire un valore.

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -File startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Creare un elemento personalizzato
Creare l'elemento personalizzato eseguendo i passaggi seguenti:

1. Installare un editor JSON: è necessario un editor JSON per lavorare con i file di definizione dell'elemento. Si consiglia di utilizzare [Visual Studio Code](https://code.visualstudio.com/), che è disponibile per Windows, Linux e OS X.
2. Ottenere un esempio di artifactfile.json: Controllare gli elementi creati dal team di Lab di sviluppo e test di Azure nell’ [archivio GitHub](https://github.com/Azure/azure-devtestlab) in cui è stata creata una libreria completa degli elementi che aiuta nella creazione dei propri elementi. Scaricare un file di definizione dell’elemento e apportare modifiche al codice per creare i propri elementi.
3. Utilizzo di IntelliSense - utilizzare IntelliSense per visualizzare elementi validi che possono essere utilizzati per costruire un file di definizione dell'elemento. È inoltre possibile visualizzare le diverse opzioni per i valori di un elemento. Ad esempio, IntelliSense mostra le due opzioni di Windows o Linux quando si modifica l’elemento **targetOsType** .
4. Archiviare l'elemento in un archivio git
   
   1. Creare una directory distinta per ogni elemento in cui il nome della directory è identico al nome dell’elemento.
   2. Archiviare il file di definizione dell’elemento (artifactfile.json) nella directory che è stato creata.
   3. Archiviare gli script a cui viene fatto riferimento tramite il comando di installazione dell'elemento.
      
      Di seguito è riportato un esempio di come potrebbe apparire una cartella di elementi:
      
      ![Esempio di archivio git dell’elemento](./media/devtest-lab-artifact-author/git-repo.png)
5. Aggiungere l'archivio elementi al lab. Vedere l'articolo sull'[Aggiungere un archivio elementi Git a un lab](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Post di blog correlati
* [How to troubleshoot failing Artifacts in AzureDevTestLabs (Come risolvere gli errori degli elementi in Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs)
* [Join a VM to existing AD Domain using ARM template in Azure Dev Test Lab (Aggiungere una macchina virtuale a un dominio di AD esistente usando un modello ARM in Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Passaggi successivi
* Informazioni su come [Aggiungere un archivio elementi Git a un lab](devtest-lab-add-artifact-repo.md).


