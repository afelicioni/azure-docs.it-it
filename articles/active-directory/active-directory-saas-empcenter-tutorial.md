---
title: 'Esercitazione: Integrazione di Azure Active Directory con EmpCenter | Microsoft Docs'
description: Informazioni su come usare EmpCenter con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: a00ecf6e-917a-4284-b998-41506931585e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 0837cb33bf438fb7fd9665d21d411f0170cdd393
ms.openlocfilehash: 6f217ee0398933cfad713398952a79d39b6020c3
ms.lasthandoff: 02/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Esercitazione: Integrazione di Azure Active Directory con EmpCenter
Questa esercitazione descrive l'integrazione di Azure e EmpCenter.  
Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* Sottoscrizione di Azure valida
* Sottoscrizione di EmpCenter abilitata per l'accesso Single Sign-On

Al termine dell'esercitazione, gli utenti di Azure AD assegnati a EmpCenter potranno accedere all'applicazione tramite il sito aziendale di EmpCenter (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1. Abilitazione dell'integrazione dell'applicazione per EmpCenter
2. Configurazione dell'accesso Single Sign-On
3. Configurazione del provisioning utente
4. Assegnazione degli utenti

![Scenario](./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenario")

## <a name="enabling-the-application-integration-for-empcenter"></a>Abilitazione dell'integrazione dell'applicazione per EmpCenter
Questa sezione descrive come abilitare l'integrazione dell'applicazione per EmpCenter.

### <a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Per abilitare l'integrazione dell'applicazione per EmpCenter, seguire questa procedura:
1. Nel portale di Azure classico fare clic su **Active Directory**nel riquadro di spostamento sinistro.
   
   ![Active Directory](./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
   ![Applicazioni](./media/active-directory-saas-empcenter-tutorial/IC700994.png "Applicazioni")
4. Fare clic su **Add** nella parte inferiore della pagina.
   
   ![Aggiungere un'applicazione](./media/active-directory-saas-empcenter-tutorial/IC749321.png "Aggiungere un'applicazione")
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
   ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-empcenter-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")
6. Nella **casella di ricerca** digitare **EmpCenter**.
   
   ![Raccolta di applicazioni](./media/active-directory-saas-empcenter-tutorial/IC802917.png "Raccolta di applicazioni")
7. Nel riquadro dei risultati selezionare **EmpCenter** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
   ![EmpCentral](./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
   

## <a name="configuring-single-sign-on"></a>Configurazione dell'accesso Single Sign-On

Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione a EmpCenter tramite il proprio account in Azure AD usando la federazione basata sul protocollo SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Per configurare l'accesso Single Sign-On, seguire questa procedura:
1. Nella pagina di integrazione dell'applicazione **EmpCenter** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
   ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802919.png "Configurare l'accesso Single Sign-On")
2. Nella pagina **Stabilire come si desidera che gli utenti accedano a EmpCenter** selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.
   
   ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802920.png "Configurare l'accesso Single Sign-On")
3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:
   
   ![Configurare le impostazioni dell'app](./media/active-directory-saas-empcenter-tutorial/IC802921.png "Configurare le impostazioni dell'app")
   
   1. Nella casella di testo **URL di accesso** digitare l'URL usato dagli utenti per accedere all'applicazione EmpCenter, ad esempio: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
   2. Fare clic su **Avanti**
4. Nella pagina **Configura accesso Single Sign-On in EmpCenter** fare clic su **Download metadati** e quindi salvare il file di metadati nel computer.
   
   ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802922.png "Configurare l'accesso Single Sign-On")
5. Inviare il file di metadati scaricato al team di supporto di EmpCenter.
   
   > [!NOTE]
   > La configurazione effettiva dell'accesso Single Sign-On deve essere eseguita dal team di supporto di EmpCenter.
   > Una volta completata l'abilitazione dell'accesso Single Sign-On per la sottoscrizione, si riceverà una notifica.
   > 
   > 
6. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Complete** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
   
   ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802923.png "Configurare l'accesso Single Sign-On")
   
## <a name="configuring-user-provisioning"></a>Configurazione del provisioning utente

Per consentire agli utenti di Azure AD di accedere a EmpCenter, è necessario eseguirne il provisioning in EmpCenter.  
Nel caso di EmpCenter, gli account utente devono essere creati dal team di supporto di EmpCenter.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da EmpCenter per eseguire il provisioning degli account utente di Azure Active Directory.
> 
> 

## <a name="assigning-users"></a>Assegnazione degli utenti
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

### <a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Per assegnare gli utenti a EmpCenter, seguire questa procedura:
1. Nel portale di Azure classico creare un account di test.
2. Nella pagina di integrazione dell'applicazione **EmpCenter** fare clic su **Assegna utenti**.
   
   ![Assegnare utenti](./media/active-directory-saas-empcenter-tutorial/IC802924.png "Assegnare utenti")
3. Selezionare l'utente di test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.
   
   ![Sì](./media/active-directory-saas-empcenter-tutorial/IC767830.png "Sì")

Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).


