---
title: Aggiornare PhoneFactor al server Azure MFA | Documentazione Microsoft
description: Introduzione all&quot;uso del server Azure MFA quando si esegue l&quot;aggiornamento dalla versione precedente dell&quot;agente PhoneFactor.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 42838ff7-bdf2-4d06-bacc-b3839a00cd76
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 4547a805c1827a703bf0ef118387882e45c3f241
ms.openlocfilehash: 5bbd7088f5d4986a5b1c9f5695a5defb678307a9


---
# <a name="upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Aggiornare l'agente PhoneFactor al server Azure Multi-Factor Authentication
Per aggiornare l'agente PhoneFactor&5;.x o versione precedente al server Azure Multi-Factor Authentication, occorre prima disinstallare l'agente PhoneFactor e i componenti associati. È quindi possibile installare il server Multi-Factor Authentication e i relativi componenti associati.

## <a name="uninstall-the-phonefactor-agent"></a>Disinstallare l'agente PhoneFactor

1. Innanzitutto, eseguire il backup del file di dati di PhoneFactor. Il percorso di installazione predefinito è C:\Programmi\PhoneFactor\Data\Phonefactor.pfdata.

2. Se il portale utenti è installato:
  1. Passare alla cartella di installazione ed eseguire il backup del file web.config. Il percorso di installazione predefinito è C:\inetpub\wwwroot\PhoneFactor.

  2. Se sono stati aggiunti i temi personalizzati al portale, eseguire il backup della cartella personalizzata sotto la directory C:\inetpub\wwwroot\PhoneFactor\App_Themes.

  3. Disinstallare il portale utenti tramite l'agente PhoneFactor (disponibile solo se installato nello stesso server dell'agente PhoneFactor) o tramite Programmi e funzionalità di Windows.

3. Se il servizio Web app per dispositivi mobili è installato:

  1. Passare alla cartella di installazione ed eseguire il backup del file web.config. Il percorso di installazione predefinito è C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.

  2. Disinstallare il servizio Web app per dispositivi mobili tramite Programmi e funzionalità di Windows.

4. Se l'SDK del servizio Web è installato, disinstallarlo tramite l'agente PhoneFactor o Programmi e funzionalità di Windows.

5. Disinstallare l'agente PhoneFactor tramite Programmi e funzionalità di Windows.

## <a name="install-the-multi-factor-authentication-server"></a>Installare il server Multi-Factor Authentication

Il percorso di installazione viene rilevato dal Registro di sistema dall'installazione dell'agente PhoneFactor precedente in modo che venga installato nello stesso percorso, ad esempio C:\Programmi\PhoneFactor. Le nuove installazioni avranno un percorso di installazione predefinito diverso (ad esempio, C:\Programmi\Multi-Factor Authentication Server). Il file di dati lasciato dall'agente PhoneFactor precedente deve essere aggiornato durante l'installazione, pertanto le impostazioni e gli utenti dovranno essere ancora disponibili dopo aver installato il nuovo server Multi-Factor Authentication.

1. Se richiesto, attivare il server Multi-Factor Authentication e assicurarsi che venga assegnato al gruppo di replica corretto.

2. Se l'SDK del servizio Web in precedenza è stato installato, installare il nuovo SDK servizio Web tramite l'interfaccia utente del server Multi-Factor Authentication.

  Il nome predefinito della directory virtuale è adesso "MultiFactorAuthWebServiceSdk" anziché "PhoneFactorWebServiceSdk". Se si desidera utilizzare il nome precedente, è necessario modificare il nome della directory virtuale durante l'installazione. In caso contrario, si consente all'installazione di usare il nuovo nome predefinito, è necessario modificare l'URL in tutte le applicazioni che fanno riferimento all'SDK del servizio Web, ad esempio il portale utenti e il servizio Web app per dispositivi mobili in modo che punti al percorso corretto.

3. Se il portale utenti in precedenza è stato installato nel server dell'agente PhoneFactor, installare il nuovo portale utenti Multi-Factor Authentication tramite la relativa interfaccia utente.

  Il nome predefinito della directory virtuale è adesso "MultiFactorAuth" anziché "PhoneFactor". Se si desidera utilizzare il nome precedente, è necessario modificare il nome della directory virtuale durante l'installazione. In caso contrario, se si consente all'installazione di usare il nuovo nome predefinito, è necessario fare clic sull'icona del portale utenti nel server Multi-Factor Authentication e aggiornare l'URL del portale utente nella scheda Impostazioni.

4. Se in precedenza il portale per gli utenti e/o il servizio Web app per dispositivi mobili è stato installato in un server diverso dall'agente PhoneFactor:

  1. Passare al percorso di installazione, ad esempio C:\Program Files\PhoneFactor, e copiare i programmi di installazione appropriati nell'altro server. Sono disponibili programmi di installazione a 32 bit e a 64 bit per il portale per gli utenti e il servizio Web app per dispositivi mobili. Si chiamano rispettivamente MultiFactorAuthenticationUserPortalSetupXX.msi e MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.

  2. Per installare il portale per gli utenti nel server Web, aprire un prompt dei comandi come amministratore ed eseguire il file MultiFactorAuthenticationUserPortalSetupXX.msi.

    Il nome predefinito della directory virtuale è adesso "MultiFactorAuth" anziché "PhoneFactor". Se si desidera utilizzare il nome precedente, è necessario modificare il nome della directory virtuale durante l'installazione. In caso contrario, se si consente all'installazione di usare il nuovo nome predefinito, è necessario fare clic sull'icona del portale utenti nel server Multi-Factor Authentication e aggiornare l'URL del portale utente nella scheda Impostazioni. Sarà necessario informare gli utenti esistenti del nuovo URL.

  3. Passare al percorso di installazione del portale per gli utenti, ad esempio C:\inetpub\wwwroot\MultiFactorAuth, e modificare il file web.config. Copiare i valori nelle sezioni appSettings e applicationSettings presenti nel file web.config originale di cui è stato eseguito il backup prima dell'aggiornamento nel nuovo file web.config. Se il nuovo nome della directory virtuale predefinita è stato mantenuto durante l'installazione dell'SDK servizio Web, modificare l'URL nella sezione applicationSettings in modo che punti al percorso corretto. Se nel file web.config precedente sono state modificate altre impostazioni predefinite, applicare le stesse modifiche nel nuovo file web.config.

  4. Per installare il servizio Web app per dispositivi mobili nel server Web, aprire un prompt dei comandi come amministratore ed eseguire il file MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.

    Il nome predefinito della directory virtuale è adesso "MultiFactorAuthMobileAppWebService" anziché "PhoneFactorPhoneAppWebService". Se si desidera utilizzare il nome precedente, è necessario modificare il nome della directory virtuale durante l'installazione. È possibile scegliere un nome più breve per consentire agli utenti di digitarlo meglio nei propri dispositivi mobili. In alternativa, se si consente l'uso del nuovo nome predefinito per il programma di installazione, è necessario fare clic sull'icona dell'app per dispositivi mobili nel server Multi-Factor Authentication e aggiornare l'URL del servizio Web app per dispositivi mobili.

  5. Passare al percorso di installazione del servizio Web app per dispositivi mobili, ad esempio C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService, e modificare il file web.config. Copiare i valori nelle sezioni appSettings e applicationSettings presenti nel file web.config originale di cui è stato eseguito il backup prima dell'aggiornamento nel nuovo file web.config. Se il nuovo nome della directory virtuale predefinita è stato mantenuto durante l'installazione dell'SDK servizio Web, modificare l'URL nella sezione applicationSettings in modo che punti al percorso corretto. Se nel file web.config precedente sono state modificate altre impostazioni predefinite, applicare le stesse modifiche nel nuovo file web.config.

## <a name="next-steps"></a>Passaggi successivi

- [Installare il portale utenti](multi-factor-authentication-get-started-portal.md) per il server Azure Multi-Factor Authentication.

- [Configurare l'autenticazione di Windows](multi-factor-authentication-get-started-server-windows.md) per le applicazioni. 



<!--HONumber=Feb17_HO1-->


