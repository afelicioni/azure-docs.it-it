---
title: 'Esercitazione: Integrazione di Azure Active Directory con ShiftPlanning | Microsoft Docs'
description: Informazioni su come usare ShiftPlanning con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 1477ba08b35a853ef7c26f74a4d9e86b3bf6d850
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-shiftplanning"></a>Esercitazione: Integrazione di Azure Active Directory con ShiftPlanning
In questa esercitazione viene illustrata l'integrazione di Azure e ShiftPlanning.

Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* Sottoscrizione di Azure valida
* Sottoscrizione di ShiftPlanning abilitata per l'accesso Single Sign-On (SSO)

Al termine dell'esercitazione, gli utenti di Azure AD assegnati a ShiftPlanning saranno in grado di eseguire l’accesso Single Sign-On all'applicazione tramite il sito aziendale di ShiftPlanning (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1. Abilitazione dell'integrazione dell'applicazione per ShiftPlanning
2. Configurazione dell'accesso Single Sign-On (SSO)
3. Configurazione del provisioning utente
4. Assegnazione degli utenti

![Scenario](./media/active-directory-saas-shiftplanning-tutorial/IC786612.png "Scenario")

## <a name="enable-the-application-integration-for-shiftplanning"></a>Abilitare l'integrazione dell'applicazione per ShiftPlanning
In questa sezione viene descritto come abilitare l'integrazione dell'applicazione per ShiftPlanning.

**Per abilitare l'integrazione dell'applicazione per ShiftPlanning, seguire questa procedura:**

1. Nel portale di Azure classico fare clic su **Active Directory**nel riquadro di spostamento sinistro.
   
    ![Active Directory](./media/active-directory-saas-shiftplanning-tutorial/IC700993.png "Active Directory")

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applicazioni](./media/active-directory-saas-shiftplanning-tutorial/IC700994.png "Applicazioni")

4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Aggiungere un'applicazione](./media/active-directory-saas-shiftplanning-tutorial/IC749321.png "Aggiungere un'applicazione")

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-shiftplanning-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")

6. Nella **casella di ricerca** digitare **ShiftPlanning**.
   
    ![Raccolta di applicazioni](./media/active-directory-saas-shiftplanning-tutorial/IC786613.png "Raccolta di applicazioni")

7. Nel riquadro dei risultati selezionare **ShiftPlanning**, quindi fare clic su **Completa** per aggiungere l'applicazione.
   
    ![ShiftPlanning](./media/active-directory-saas-shiftplanning-tutorial/IC786614.png "ShiftPlanning")
   
## <a name="configure-single-sign-on"></a>Configura accesso Single Sign-On

In questa sezione viene descritto come consentire agli utenti di eseguire l'autenticazione a ScreenSteps tramite il proprio account di Azure AD usando la federazione basata sul protocollo SAML.

Come parte di questa procedura, verrà richiesto di creare un file di certificato con codifica Base 64.  

Se non si ha familiarità con questa procedura, vedere il video che descrive [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o)

**Per configurare l'accesso Single Sign-On, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **ShiftPlanning** nel portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786615.png "Configurare l'accesso Single Sign-On")

2. Nella pagina **Stabilire come si desidera che gli utenti accedano a ShiftPlanning** selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786616.png "Configurare l'accesso Single Sign-On")

3. Nella casella di testo **URL accesso ShiftPlanning** della pagina **Configura URL app** digitare l'URL usando il modello "*https://company.shiftplanning.com/includes/saml/*" e fare clic su **Avanti**.
   
    ![Configurare l'URL dell'app](./media/active-directory-saas-shiftplanning-tutorial/IC786617.png "Configurare l'URL dell'app")

4. Nella pagina **Configura accesso Single Sign-On in ShiftPlanning** fare clic su **Scarica certificato** per scaricare il certificato e salvare il file di certificato sul computer.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786618.png "Configurare l'accesso Single Sign-On")

5. In un'altra finestra del Web browser accedere al sito aziendale di **ShiftPlanning** come amministratore.
6. Nel menu in alto fare clic su **Admin**.
   
    ![Amministratore](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Amministratore")

7. In **Integrazione** fare clic su **Single Sign-On**.
   
    ![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786620.png "Single Sign-On")

8. Nella sezione **Single Sign-On** , eseguire la procedura seguente:
   
    ![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786905.png "Single Sign-On")
   
   1. Selezionare **Abilitato SAML**.
   2. Selezionare **Allow Password Login** (Consenti accesso tramite password).
   3. Nella finestra di dialogo **Configura accesso Single Sign-On in ShiftPlanning** del portale di Azure classico copiare il valore di **URL accesso remoto** e incollarlo nella casella di testo **SAML Issuer URL** (URL autorità di certificazione SAML).
   4. Nella finestra di dialogo **Configure single sign-on at ShiftPlanning** (Configura accesso Single Sign-On in ShiftPlanning) del portale di Azure classico copiare il valore di **URL disconnessione remota** e incollarlo nella casella di testo **Remote Logout URL** (URL disconnessione remota).
   5. Creare un file con **codifica Base 64** dal certificato scaricato.  
       
     >[!TIP]
     >Per informazioni dettagliate, vedere [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o)
     > 
     > 

   6. Aprire il certificato con codifica base 64 nel blocco note, copiarne il contenuto negli appunti e incollarlo nella casella di testo **Certificato X.509** .
   7. Fare clic su **Salva impostazioni**.

9. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Complete** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786621.png "Configurare l'accesso Single Sign-On")
   
## <a name="configure-user-provisioning"></a>Configura provisioning utenti

Per consentire agli utenti di Azure AD di accedere a ShiftPlanning, è necessario eseguirne il provisioning in ShiftPlanning.  

Nel caso di ShiftPlanning, il provisioning è un'attività manuale.

**Per effettuare il provisioning di un account utente, seguire questa procedura:**

1. Accedere al sito aziendale di **ShiftPlanning** come amministratore.
2. Fare clic su **Admin**.
   
    ![Amministratore](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Amministratore")
3. Fare clic su **Personale**.
   
    ![Personale](./media/active-directory-saas-shiftplanning-tutorial/IC786623.png "Personale")
4. In **Actions** (Azioni) fare clic su **Add Employee** (Aggiungi dipendente).
   
    ![Aggiungi dipendenti](./media/active-directory-saas-shiftplanning-tutorial/IC786624.png "Aggiungi dipendenti")
5. Nella sezione **Aggiungi dipendenti** eseguire la procedura seguente:
   
    ![Salvare i dipendenti](./media/active-directory-saas-shiftplanning-tutorial/IC786625.png "Salvare i dipendenti")
   
   1. Digitare il **nome**, il **cognome** e l'**indirizzo di posta elettronica** di un account di Azure Active Directory valido di cui si desidera eseguire il provisioning nelle relative caselle di testo.
   2. Fare clic su **Salva dipendenti**.

>[!NOTE]
>È possibile usare qualsiasi altro strumento o API di creazione di account utente offerti da ShiftPlanning per eseguire il provisioning degli account utente di Azure AD.
> 
> 

## <a name="assign-users"></a>Assegna utenti
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

**Per assegnare gli utenti a ShiftPlanning, seguire questa procedura:**

1. Nel portale di Azure classico creare un account di test.

2. Nella pagina di integrazione dell'applicazione **ShiftPlanning**  fare clic su **Assegna utenti**.
   
    ![Assegnare utenti](./media/active-directory-saas-shiftplanning-tutorial/IC786626.png "Assegnare utenti")

3. Selezionare l'utente di test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.
   
    ![Sì](./media/active-directory-saas-shiftplanning-tutorial/IC767830.png "Sì")
 
Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).


