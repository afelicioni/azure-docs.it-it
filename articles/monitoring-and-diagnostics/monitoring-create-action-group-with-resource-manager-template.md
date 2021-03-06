---
title: Creare gruppi di azione con i modelli di Resource Manager | Microsoft Docs
description: I gruppi di azione consentono di inviare una notifica tramite posta elettronica, un SMS o chiamare webhook quando si verificano determinati eventi.
author: anirudhcavale
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 2dd7b14f1466fa7244a2af2c030d8b794658aaad
ms.contentlocale: it-it
ms.lasthandoff: 04/12/2017


---

# <a name="create-an-action-group-with-a-resource-manager-template"></a>Creare un gruppo di azione con un modello di Resource Manager
L'articolo illustra come usare un [modello di Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) per configurare gruppi di azione. I modelli consentono di impostare automaticamente gruppi di azione relativi alle risorse al momento della creazione per assicurarsi che tutte le parti corrette ricevano una notifica quando viene attivato un avviso.

I passaggi di base sono i seguenti:

1.    Creare un modello come file JSON che descriva come creare il gruppo di azione.
2.    [Distribuire il modello con un metodo di distribuzione qualsiasi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

La sezione seguente descrive come creare un modello di Resource Manager prima solo per un gruppo di azione e poi per un gruppo di azione durante la creazione di un'altra risorsa.

## <a name="resource-manager-template-for-an-action-group"></a>Modello di Resource Manager per un gruppo di azione

Per creare un gruppo di azione usando un modello di Resource Manager, si crea una risorsa di tipo `Microsoft.Insights/actionGroups` e si compilano tutte le proprietà correlate. Due modelli di esempio che creano un gruppo di azione sono mostrati di seguito.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    }    
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```


## <a name="next-steps"></a>Passaggi successivi
Altre informazioni sui [gruppi di azione](monitoring-action-groups.md)  
Altre informazioni sugli [Avvisi](monitoring-overview-alerts.md)  
Come aggiungere [avvisi usando un modello di Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)

