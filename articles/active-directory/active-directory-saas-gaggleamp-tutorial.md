---
title: 'Esercitazione: Integrazione di Azure Active Directory con GaggleAMP | Documentazione Microsoft'
description: Informazioni su come configurare l&quot;accesso Single Sign-On tra Azure Active Directory e GaggleAMP.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 3c83745c09514e63cfe686fffd319328b19ee460
ms.openlocfilehash: b70cb12131d97fcf2569f5efd97b87037baa1837
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a>Esercitazione: Integrazione di Azure Active Directory con GaggleAMP
Questa esercitazione descrive l'integrazione di GaggleAMP con Azure Active Directory (Azure AD).

L'integrazione di GaggleAMP con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a GaggleAMP
* È possibile abilitare gli utenti per l'accesso Single Sign-On automatico a GaggleAMP (SSO) con i propri account Azure AD
* È possibile gestire gli account da una posizione centrale: il portale di Azure classico 

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti
Per configurare l'integrazione di Azure AD con GaggleAMP, sono necessari gli elementi seguenti:

* Sottoscrizione di Azure AD.
* Sottoscrizione di GaggleAMP abilitata per l'accesso SSO

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.
> 
> 

A questo scopo, è consigliabile seguire le indicazioni seguenti:

* Non usare l'ambiente di produzione, a meno che non sia necessario.
* Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
L'obiettivo di questa esercitazione è quello di testare l'accesso Single Sign-On (SSO) di Azure AD in un ambiente di test. 

Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di GaggleAMP dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On (SSO) di Microsoft Azure AD

## <a name="add-gaggleamp-from-the-gallery"></a>Aggiungere GaggleAMP dalla raccolta
Per configurare l'integrazione di GaggleAMP in Azure AD, è necessario aggiungere GaggleAMP dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere GaggleAMP dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro. 
   
    ![Active Directory][1]
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applications][2]
4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Applicazioni][3]
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Applicazioni][4]
6. Nella casella di ricerca digitare **GaggleAMP**.
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_01.png)
7. Nel riquadro dei risultati selezionare **GaggleAMP** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_02.png)>

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD
Questa sezione descrive come configurare e testare l'accesso Single Sign-On di Azure AD con GaggleAMP in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso SSO, Azure AD deve conoscere qual è l'utente di GaggleAMP che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in GaggleAMP.

La relazione di collegamento viene stabilita assegnando il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente) in GaggleAMP.

Per configurare e testare l'accesso Single Sign-On di Azure AD con GaggleAMP, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creazione di un utente di test di GaggleAMP](#creating-a-GaggleAMP-test-user)** : per avere una controparte di Britta Simon in GaggleAMP collegata alla relativa rappresentazione in Azure AD.
4. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)**: per verificare se la configurazione funziona.

### <a name="configure-azure-ad-sso"></a>Configurare l'accesso SSO di Azure AD
Questa sezione descrive come abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure classico e come configurare l'accesso Single Sign-On nell'applicazione GaggleAMP.

**Per configurare l'accesso SSO di Azure AD con GaggleAMP, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **GaggleAMP** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
    ![Configura accesso Single Sign-On][6] 
2. Nella pagina **Stabilire come si desidera che gli utenti accedano a GaggleAMP** selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_03.png) 
3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_04.png) 
   1. Nella casella di testo URL di accesso, digitare l'URL usato dagli utenti per accedere all'applicazione GaggleAMP adottando il modello seguente: **"https://secure4.gaggleamp.com"**.

    >[!NOTE]
    >Contattare il [team delle vendite di GaggleAMP](mailto:sales@gaggleamp.com) se è necessario il valore **URL di accesso** per l'applicazione."
    >
   2. Fare clic su **Avanti**.

4. Nella pagina **Configura accesso Single Sign-On in GaggleAMP** seguire questa procedura:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_05.png)   
  1. Fare clic su **Scarica certificato**e quindi salvare il file nel computer. Questo certificato e gli URL dei metadati (ID entità, URL di accesso SSO e URL di disconnessione SSO) saranno necessari per configurare l'accesso Single Sign-On nel sito GaggleAMP.
  2. Fare clic su **Avanti**.
5. In un'altra istanza del browser passare alla pagina dell'accesso Single Sign-On SAML creata dal team di supporto di Gaggle, ad esempio: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*.
6. Nella pagina **SAML SSO** seguire questa procedura:  
   
    ![Accesso Single Sign-On di GaggleAMP](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_06.png)  
  1. Nel portale di Azure classico, copiare il valore di **URL autorità di certificazione** e quindi incollarlo nella casella di testo **Autorità di certificazione del provider di identità**.  
  2. Nel portale di Azure classico copiare il valore di **URL servizio Single Sign-On** e quindi incollarlo nella casella di testo **Identity Provider Single Sign-On URL**. 
  3. Fare clic su **Save**      
  4. Inviare il certificato scaricato al [team di vendite di GaggleAMP](mailto:sales@gaggleamp.com).
5. Nel portale di Azure classico, selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**.
   
    ![Single Sign-On di Microsoft Azure AD][10]
6. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.  
   
    ![Single Sign-On di Microsoft Azure AD][11]

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di Azure classico.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_09.png) 
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per visualizzare l'elenco di utenti, fare clic su **Utenti**nel menu in alto.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_03.png) 
4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** nella barra degli strumenti in basso.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_04.png) 
5. Nella pagina **Informazioni sull'utente** seguire questa procedura:
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_05.png) 
  1. In Tipo di utente selezionare Nuovo utente nell'organizzazione.  
  2. Nella casella di testo **Nome utente** digitare **BrittaSimon**.
  3. Fare clic su **Avanti**.
6. Nella pagina **Profilo utente** seguire questa procedura:
   
   ![Creazione di un utente test di Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_06.png) 
  1. Nella casella di testo **Nome** digitare **Britta**.  
  2. Nella casella di testo **Cognome** digitare **Simon**.
  3. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.
  4. Nell'elenco **Ruolo** selezionare **Utente**.
  5. Fare clic su **Avanti**.
7. Nella pagina **Ottieni password temporanea** fare clic su **crea**.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_07.png) 
8. Nella pagina **Ottieni password temporanea** seguire questa procedura:
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_08.png) 
  1. Prendere nota del valore visualizzato in **Nuova password**.
  2. Fare clic su **Completa**.   

### <a name="create-a-gaggleamp-test-user"></a>Creare un utente di test di GaggleAMP
Questa sezione descrive come creare un utente chiamato Britta Simon in GaggleAMP. GaggleAMP supporta il provisioning just-in-time, che è abilitato per impostazione predefinita.

Non è necessario alcun intervento dell'utente in questa sezione. Durante un tentativo di accesso a GaggleAMP verrà creato un nuovo utente, se questo non esiste già. 

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD
Questa sezione descrive come abilitare Britta Simon a usare l'accesso Single Sign-On di Azure concedendole l'accesso a GaggleAMP.

![Assegna utente][200] 

**Per assegnare Britta Simon a GaggleAMP, seguire questa procedura:**

1. Per aprire la visualizzazione applicazioni nel portale di Azure, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.
   
    ![Assegna utente][201] 
2. Nell'elenco di applicazioni selezionare **GaggleAMP**.
    ![Elenco di Azure](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_50.png)
3. Scegliere **Utenti**dal menu in alto.
   
    ![Assegna utente][203] 
4. Nell'elenco di utenti selezionare **Britta Simon**.
5. Fare clic su **Assegna**sulla barra degli strumenti in basso.
   
    ![Assegna utente][205]

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On
Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro GaggleAMP nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione GaggleAMP.

## <a name="additional-resources"></a>Risorse aggiuntive
* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_205.png

