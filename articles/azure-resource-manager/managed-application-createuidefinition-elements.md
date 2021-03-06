---
title: Funzioni per la creazione di definizioni dell&quot;interfaccia utente di Applicazione gestita di Azure | Microsoft Docs
description: Illustra le funzioni da usare durante la creazione di definizioni dell&quot;interfaccia utente per le applicazioni gestite di Azure
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: 635e44a7ec6f9244f5fe75eb5ad947cdd8ae59a4
ms.contentlocale: it-it
ms.lasthandoff: 05/12/2017


---
# <a name="createuidefinition-elements"></a>Elementi di CreateUiDefinition
Questo articolo illustra lo schema e le proprietà per tutti gli elementi supportati di CreateUiDefinition. Usare questi elementi quando si [crea un'applicazione Azure gestita](managed-application-publishing.md). Lo schema per la maggior parte degli elementi è il seguente:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Keep calm and visit the [Azure Portal](portal.azure.com).",
  "constraints": {},
  "options": {},
  "visible": true
}
```
| Proprietà | Obbligatorio | Description |
| -------- | -------- | ----------- |
| name | Sì | Identificatore interno per fare riferimento a un'istanza specifica di un elemento. Il nome dell'elemento viene usato con maggiore frequenza in `outputs`, dove i valori di output degli elementi specificati vengono mappati ai parametri del modello. È anche possibile usarlo per associare il valore di output di un elemento al valore `defaultValue` di un altro elemento. |
| type | Sì | Controllo dell'interfaccia utente da sottoporre a rendering per l'elemento. Per un elenco di tipi supportati, vedere [Elementi](#elements). |
| label | Sì | Testo visualizzato dell'elemento. Alcuni tipi di elemento includono più etichette, quindi il valore può essere un oggetto contenente più stringhe. |
| defaultValue | No | Valore predefinito dell'elemento. Alcuni tipi di elementi supportano valori predefiniti complessi, quindi il valore può essere un oggetto. |
| toolTip | No | Testo da visualizzare nella descrizione comando dell'elemento. Analogamente a `label`, alcuni elementi supportano più stringhe di descrizione comando. I collegamenti inline possono essere incorporati tramite la sintassi di markdown.
| constraints | No | Una o più proprietà usate per personalizzare il comportamento di convalida dell'elemento. Le proprietà supportate per constraints dipendono dal tipo di elemento. Alcuni tipi di elementi non supportano la personalizzazione del comportamento di convalida e quindi non includono alcuna proprietà constraints. |
| options | No | Proprietà aggiuntive per la personalizzazione del comportamento dell'elemento. Analogamente a `constraints`, le proprietà supportate dipendono dal tipo di elemento. |
| visible | No | Indica se l'elemento è visualizzato. Se `true`, l'elemento e gli elementi figlio applicabili vengono visualizzati. Il valore predefinito è `true`. Usare le [funzioni logiche](managed-application-createuidefinition-functions.md#logical-functions) per controllare dinamicamente il valore di questa proprietà.

## <a name="elements"></a>Elementi

La documentazione per ogni elemento include un esempio di interfaccia utente, uno schema, commenti sul comportamento dell'elemento, in genere relativi alla convalida e alla personalizzazione supportata, e infine output di esempio.

- [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md)
- [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](managed-application-microsoft-common-section.md)
- [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Passaggi successivi
* Per un'introduzione alle applicazioni gestite, vedere [Panoramica di Applicazione gestita di Azure](managed-application-overview.md).
* Per un'introduzione alla creazione delle definizioni dell'interfaccia utente, vedere [Introduzione a CreateUiDefinition](managed-application-createuidefinition-overview.md).

