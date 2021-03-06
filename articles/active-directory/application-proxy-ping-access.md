---
title: Autenticazione basata su intestazione con PingAccess per il proxy dell&quot;applicazione di Azure AD | Microsoft Docs
description: Pubblicare le applicazioni con PingAccess e il proxy dell&quot;applicazione per supportare l&quot;autenticazione basata su intestazione.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: 8db76d1f83cdf1cf53ddd1e9c69c56400d04af2d
ms.contentlocale: it-it
ms.lasthandoff: 04/29/2017

---

# <a name="publish-applications-that-support-header-based-authentication-with-azure-ad-application-proxy-and-pingaccess"></a>Pubblicare le applicazioni che supportano l'autenticazione basata su intestazione con il proxy dell'applicazione di AD Azure e PingAccess

Il proxy dell'applicazione Azure Active Directory e PingAccess hanno collaborato per fornire ai clienti di Azure Active Directory l'accesso a un numero ancora maggiore di applicazioni. PingAccess espande le [offerte esistenti del proxy dell'applicazione](active-directory-application-proxy-get-started.md) per includere l'accesso remoto alle applicazioni che usano intestazioni per l'autenticazione. 

## <a name="what-is-pingaccess-for-azure-ad"></a>Che cos'è PingAccess per Azure AD?

Per consentire agli utenti l'accesso alle applicazioni che usano intestazioni per l'autenticazione, pubblicare l'app per l'accesso remoto sia nel proxy dell'applicazione sia in PingAccess. Il proxy dell'applicazione tratta queste app come qualsiasi altra, usando Azure AD per autenticare l'accesso e quindi passando il traffico attraverso il servizio del connettore. PingAccess sta davanti alle app e converte il token di accesso da Azure AD in un'intestazione, in modo che l'applicazione riceva l'autenticazione nel formato che è in grado di leggere. 

Gli utenti non noteranno nulla di diverso quando eseguono l'accesso per usare le app aziendali. Possono comunque lavorare da qualsiasi luogo e dispositivo. Quando gli utenti sono in ufficio, il traffico non viene intercettato né dal proxy dell'applicazione né da PingAccess, perciò gli utenti hanno la stessa esperienza di sempre.

Poiché i connettori del proxy dell'applicazione indirizzano il traffico remoto a tutte le app indipendentemente dal loro tipo di autenticazione, continueranno a bilanciare il carico automaticamente. 

## <a name="how-do-i-get-access"></a>Come si ottiene l'accesso?

Poiché questo scenario è il risultato di una partnership fra Azure Active Directory e PingAccess, sono necessarie le licenze per entrambi i servizi. Le sottoscrizioni di Azure Active Directory Premium includono una licenza starter di PingAccess che consente di configurare fino a 20 applicazioni con questo flusso. 

Per altre informazioni, vedere [Edizioni di Azure Active Directory](active-directory-editions.md).

## <a name="publish-your-first-application"></a>Pubblicare la prima applicazione

Questo articolo è destinato a chi pubblica un'app con questo scenario per la prima volta. Illustra come iniziare a usare sia l'applicazione sia PingAccess, oltre ai passaggi di pubblicazione. Se sono già stati configurati entrambi i servizi ma si desidera rivedere i passaggi di pubblicazione, è possibile saltare le due sezioni di registrazione.

>[!NOTE]
>Poiché questo scenario è il risultato di una partnership fra Azure AD e PingAccess, alcune delle istruzioni sono presenti sul sito di Ping Identity. 

### <a name="install-an-application-proxy-connector"></a>Installare un connettore proxy di applicazione

Se il proxy dell'applicazione è già attivato e un connettore è installato, è possibile saltare questi passaggi.

Il connettore del proxy di applicazione è un servizio di Windows Server che indirizza il traffico dai dipendenti remoti alle app pubblicate. Per istruzioni di installazione più dettagliate, vedere [Abilitare il proxy di applicazione nel portale di Azure](active-directory-application-proxy-enable.md).

1. Accedere al [portale di Azure](https://portal.azure.com) come amministratore globale. 
2. Selezionare **Azure Active Directory** > **Proxy dell'applicazione**.
3. Selezionare **Download Connector** (Scarica il connettore) per avviare il download del connettore del proxy di applicazione. Seguire le istruzioni di installazione. 
4. Scaricando il connettore si attiverà automaticamente il proxy di applicazione per la directory, ma se così non fosse, è possibile selezionare **Abilita proxy di applicazione**. 

![Abilitare il proxy dell'applicazione e scaricare il connettore](./media/application-proxy-ping-access/install-connector.png)

### <a name="add-your-app-to-azure-ad-with-application-proxy"></a>Aggiungere l'app ad Azure AD con il proxy dell'applicazione

Questa sezione è composta da due parti. In primo luogo è necessario pubblicare l'app in Azure AD. Quindi occorre raccogliere alcune informazioni sull'app che è possibile usare durante la procedura PingAccess. 

#### <a name="publish-the-app"></a>Pubblicare l'app

1. Se non lo si è fatto nell'ultima sezione, accedere al [portale di Azure](https://portal.azure.com) come amministratore globale. 
2. Selezionare **Azure Active Directory** > **Applicazioni aziendali**. 
3. Selezionare **Aggiungi** nella parte superiore del pannello. 
4. Selezionare **Applicazione locale**.
5. Compilare i campi obbligatori con le informazioni della nuova app. Usare le seguenti linee guida per le impostazioni:
  - **URL interno**: normalmente si indica l'URL che porta alla pagina di accesso dell'app quando ci si trova nella rete aziendale. Per questa relazione il connettore deve trattare il proxy PingAccess come prima pagina dell'app. Usare il formato seguente: `https://<host name of your PA server>:<port>/<App path name>`. La porta 3000 per impostazione predefinita, ma è possibile configurarla in PingAccess.
  - **Metodo di autenticazione preliminare**: Azure Active Directory
  - **Tradurre URL nelle intestazioni**: No
6. Selezionare **Aggiungi** nella parte inferiore del pannello. L'applicazione viene aggiunta e si apre il menu di avvio rapido. 
7. Nel menu di avvio rapido selezionare **Assegna utente per il test** e aggiungere almeno un utente all'applicazione. Assicurarsi che questo account di test abbia accesso all'applicazione locale. 
8. Selezionare **Assegna** per salvare l'assegnazione dell'utente di test. 
9. Nel pannello di gestione dell'app selezionare **Single Sign-On**. 
10. Scegliere **Header-based sign-on** (Accesso basato su intestazione) dal menu a discesa. Selezionare **Salva**.

  ![Selezionare l'accesso basato su intestazione](./media/application-proxy-ping-access/sso-header.PNG)

11. Chiudere il pannello Applicazioni aziendali o scorrere completamente a sinistra per tornare al menu di Azure Active Directory. 
12. Selezionare **Registrazioni per l'app**.
13. Selezionare l'app appena aggiunta e quindi **URL di risposta**. 
14. Verificare se l'URL esterno assegnato all'app al passaggio 5 è incluso nell'elenco URL di risposta. In caso contrario aggiungerlo ora. 
15. Nel pannello delle impostazioni dell'app selezionare **Autorizzazioni necessarie**. 
16. Selezionare **Aggiungi**. Per l'API scegliere **Windows Azure Active Directory** e quindi **Seleziona**. Per le autorizzazioni scegliere **Read and write all applications** (Leggi e scrivi in tutte le applicazioni) e **Accedi e leggi il profilo di un altro utente** e quindi **Seleziona** e **Fine**.  

  ![Autorizzazioni SELECT](./media/application-proxy-ping-access/select-permissions.png) 

#### <a name="collect-information-for-the-pingaccess-steps"></a>Raccogliere informazioni per la procedura PingAccess

1. Sempre nel pannello delle impostazioni dell'app selezionare **Proprietà**. Annotare il valore **ID applicazione**. Sarà usato come ID client per la configurazione di PingAccess.
2. Nel pannello delle impostazioni dell'app selezionare **Chiavi**. 
3. Creare una chiave immettendo una descrizione della chiave e scegliendo una data di scadenza dal menu a discesa. 
4. Selezionare **Salva**. Viene visualizzato un GUID nel campo **Valore**. 

  Annotare il valore adesso, in quanto non sarà più visibile dopo aver chiuso questa finestra. 

5. Chiudere il pannello Registrazioni per l'app o scorrere completamente a sinistra per tornare al menu di Azure Active Directory.
6. Selezionare **Proprietà**.
7. Salvare il GUID **ID directory**. 

### <a name="download-pingaccess-and-configure-your-app"></a>Scaricare PingAccess e configurare l'app

I passaggi dettagliati per la parte PingAccess di questo scenario continuano nella documentazione di Ping Identity, [Configurare PingAccess per Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Questi passaggi consentono di ottenere un account PingAccess se non se ne possiede già uno, installare PingAccess Server e creare una connessione al Provider OIDC di Azure AD con l'ID directory copiato dal portale di Azure. È quindi possibile usare i valori dell'ID applicazione e della chiave per creare una sessione Web in PingAccess. Successivamente è possibile impostare i mapping delle identità e creare l'host virtuale, il sito e l'applicazione.

### <a name="test-your-app"></a>Test dell'app

Dopo aver completato tutti questi passaggi, l'app dovrebbe funzionare. Per verificarlo aprire un browser e passare all'URL esterno creato quando è stata pubblicata l'applicazione in Azure. Accedere con l'account di test assegnato all'app. 

## <a name="next-steps"></a>Passaggi successivi

- [Risolvere i problemi del Proxy applicazione](active-directory-application-proxy-troubleshoot.md)

