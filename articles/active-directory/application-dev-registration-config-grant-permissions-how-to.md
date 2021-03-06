---
title: Come concedere autorizzazioni a un&quot;applicazione personalizzata | Microsoft Docs
description: Come concedere autorizzazioni all&quot;applicazione personalizzata tramite il portale di Azure AD o un parametro URL
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: 922774c2482737537b64787ae473231ec1fbb68e
ms.lasthandoff: 04/14/2017


---

# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Come concedere autorizzazioni a un'applicazione personalizzata

Se si vuole concedere in modo preventivo il consenso per l'app in uso o si verifica un errore che indica che non è stato fornito il consenso a un'app, seguire questa procedura.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Come fornire il consenso dell'amministratore per l'applicazione

Questa operazione consente di fornire all'applicazione il consenso per tutti gli utenti dell'organizzazione.

1. Passare al pannello **Registrazioni per l'app** in qualità di **amministratore globale** e quindi selezionare l'app.

2. Selezionare **Autorizzazioni necessarie** e infine scegliere il pulsante **Concedi autorizzazioni** nella parte superiore del pannello.

È possibile, in alternativa, creare una richiesta in *login.microsoftonline.com* con le configurazioni dell'app e aggiungerla in *&prompt=admin\_consent*. Dopo avere eseguito l'accesso con le credenziali di amministratore, all'app viene concesso il consenso per tutti gli utenti.

## <a name="how-to-force-user-consent-for-your-application"></a>Come forzare il consenso dell'utente per l'applicazione

* Eseguire l'aggiunta alle richieste di autenticazione *&prompt=consent* che richiedono il consenso degli utenti finali a ogni autenticazione.

## <a name="next-steps"></a>Passaggi successivi

[Consenso e integrazione di applicazioni con Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[Consenso e concessione delle autorizzazioni per le app con convergenza di Azure Active Directory v2.0](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD in Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory)

