---

title: Assegnare licenze a un gruppo in Azure Active Directory | Documentazione Microsoft
description: Come assegnare licenze usando le licenze basate sui gruppi di Azure Active Directory
services: active-directory
keywords: Licenze di Azure AD
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: cd2c8b8bd006d3172cb75f12f4a16fef9b698974
ms.lasthandoff: 03/10/2017


---

# <a name="assign-licenses-to-a-group-of-users-in-azure-active-directory"></a>Assegnare licenze a un gruppo di utenti in Azure Active Directory

Questo articolo illustra in modo dettagliato uno scenario di base relativo all'assegnazione di licenze a un gruppo di utenti in Azure Active Directory (Azure AD) e quindi alla verifica che tutti i membri del gruppo ottengano correttamente la licenza.

In questo esempio il tenant contiene un gruppo di sicurezza denominato *HR Department*. Il gruppo include tutti i membri del reparto Risorse umane, in questo caso circa 1.000 utenti. L'amministratore vuole assegnare licenze di Office 365 Enterprise E3 all'intero reparto. Il servizio Enterprise Yammer incluso nel prodotto deve essere disabilitato temporaneamente fino a un secondo momento, quando il reparto sarà pronto per iniziare a usarlo. L'amministratore desidera inoltre distribuire le licenze Enterprise Mobility + Security allo stesso gruppo di utenti.

## <a name="step-1-assign-the-required-licenses"></a>Passaggio 1: assegnare le licenze necessarie

1. Accedere al [**Portale di Azure**](https://portal.azure.com) con un account amministratore. Per consentire di gestire le licenze, l'account richiede il ruolo amministratore globale o il ruolo amministratore dell'account utente.

2. Fare clic su **More Services** (Altri servizi) nel riquadro di spostamento a sinistra e scegliere **Azure Active Directory**. È possibile inserire tra i preferiti questo pannello facendo clic sull'icona a stella o aggiungendolo al dashboard del portale.

3. Nel pannello **Azure Active Directory** selezionare **Licenze**. Verrà aperto un pannello in cui si possono visualizzare e gestire tutti i prodotti del tenant a cui è possibile assegnare una licenza.

4. In **All products** (Tutti i prodotti) selezionare sia Office 365 Enterprise E3 sia Enterprise Mobility + Security scegliendo i nomi di prodotto. Selezionare **Assegna** nella parte superiore del pannello per avviare l'assegnazione.

  ![Tutti i prodotti, assegnare una licenza](media/active-directory-licensing-group-assignment-azure-portal/all-products-assign.png)

5. Nel pannello**Assegna licenza** fare clic su **Utenti e gruppi** per aprire il pannello **Utenti e gruppi**. Cercare il nome del gruppo *Reparto risorse Umane*, selezionare il gruppo e quindi assicurarsi di confermare facendo clic su **Seleziona** nella parte inferiore del pannello.

  ![Selezionare un gruppo](media/active-directory-licensing-group-assignment-azure-portal/select-a-group.png)

6. Nel pannello **Assegna licenza** fare clic su **Opzioni di assegnazione (facoltativo)** per visualizzare tutti i piani di servizio inclusi nei due prodotti selezionati in precedenza. Trovare **Yammer Enterprise** e impostarlo su **Disattivato** per disabilitare il servizio dalla licenza del prodotto. Confermare facendo clic su **OK** nella parte inferiore di **Opzioni di assegnazione**.

  ![Opzioni di assegnazione](media/active-directory-licensing-group-assignment-azure-portal/assignment-options.png)

7. Infine, nel pannello **Assegna licenza** fare clic su **Assegna** nella parte inferiore del pannello per completare l'assegnazione.

8. Verrà visualizzata una notifica nell'angolo in alto a destra che mostra lo stato e il risultato del processo. Se non è stato possibile completare l'assegnazione al gruppo, ad esempio a causa di licenze già esistenti nel gruppo, fare clic sulla notifica per visualizzare i dettagli dell'errore.

È stato specificato un modello di licenza nel gruppo del reparto risorse umane. È stato avviato un processo in background in Azure AD per elaborare tutti i membri esistenti di tale gruppo. L'operazione iniziale potrebbe richiedere molto tempo, in base alla dimensione corrente del gruppo. Nel passaggio successivo verrà illustrato come verificare che il processo sia stato completato e se sono necessari altri interventi per la risoluzione dei problemi.

> [!NOTE]
> È possibile avviare la stessa assegnazione da un percorso alternativo: **Utenti e gruppi** in Azure AD. Passare ad **Azure Active Directory** > **Utenti e gruppi** > **Tutti i gruppi**. Per trovare il gruppo, selezionarlo e passare alla scheda **Licenze**. Con il pulsante **Assegna** nella parte superiore del pannello è possibile aprire il pannello di assegnazione delle licenze.

## <a name="step-2-verify-that-the-initial-assignment-has-completed"></a>Passaggio 2: verificare che l'assegnazione iniziale sia stata completata

1. Passare ad **Azure Active Directory** > **Utenti e gruppi** > **Tutti i gruppi**. Trovare quindi il gruppo *HR Department* a cui sono state assegnate le licenze.

2. Nel pannello del gruppo *HR Department* selezionare **Licenze** per verificare rapidamente se le licenze sono state completamente assegnate agli utenti e se ci sono errori che richiedono attenzione. Sono disponibili le informazioni seguenti:

  - Elenco delle licenze di prodotto attualmente assegnate al gruppo. Selezionare una voce per visualizzare i servizi specifici che sono stati abilitati e apportare modifiche.

  - Stato delle modifiche più recenti delle licenze apportate al gruppo, se è in corso l'elaborazione delle modifiche o se l'elaborazione è stata completata per tutti i membri utente.

  - Informazioni sugli utenti con stato di errore perché non è stato possibile assegnare le licenze a tali utenti.

  ![Opzioni di assegnazione](media/active-directory-licensing-group-assignment-azure-portal/assignment-errors.png)

3. Per informazioni più dettagliate sull'elaborazione delle licenze, vedere **Azure Active Directory** > **Utenti e gruppi** > *nome gruppo* > **Log di controllo**. Si notino le attività seguenti:

  - Attività: **iniziare ad applicare agli utenti le licenze basate sui gruppi**. Viene registrato quando il sistema preleva la modifica dell'assegnazione della licenza per il gruppo e avvia l'applicazione a tutti i membri utente. Contiene informazioni sulla modifica apportata.

  - Attività: **finire di applicare agli utenti le licenze basate sui gruppi**. Viene registrato quando il nostro sistema completa l'elaborazione di tutti gli utenti del gruppo. Contiene un riepilogo di quanti utenti sono stati elaborati correttamente e il numero di utenti a cui non è stato possibile assegnare le licenze di gruppo.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Passaggio 3: Controllare i problemi relativi alle licenze e risolverli

1. Passare ad **Azure Active Directory** > **Utenti e gruppi** > **Tutti i gruppi**, quindi trovare il gruppo *HR Department* a cui sono state assegnate le licenze.
2. Nel pannello del gruppo **Reparto Risorse umane** selezionare **Licenze**. La notifica nella parte superiore del pannello indica che ci sono 10 utenti per cui non è stato possibile assegnare le licenze. Facendo clic su di essa viene aperto un elenco di tutti gli utenti che presentano un errore di licenza per questo gruppo.
3. La colonna **N. assegnazioni con errori** indica che non è stato possibile assegnare entrambe le licenze prodotto agli utenti. **Principali cause dell'errore** contiene la causa dell'errore. In questo caso si tratta di **Piani di servizio in conflitto**.

  ![N. assegnazioni con errori](media/active-directory-licensing-group-assignment-azure-portal/failed-assignments.png)

4. Selezionare un utente per aprire il pannello **Licenze**. Questo pannello mostra tutte le licenze attualmente assegnate all'utente. In questo esempio, è possibile vedere che l'utente ha la licenza di Office 365 Enterprise E1 ereditata dal gruppo **Kiosk users**. Questo genera un conflitto con la licenza E3 che il sistema ha provato ad applicare dal gruppo **HR Department**. Nessuna licenza del gruppo è stata quindi assegnata all'utente.

  ![Visualizzare le licenze per un utente](media/active-directory-licensing-group-assignment-azure-portal/user-license-view.png)

5. Per risolvere questo problema, è possibile rimuovere l'utente dal gruppo **Utenti del chiosco**. Dopo che Azure AD elabora la modifica, le licenze del **Reparto risorse umane** sono assegnate correttamente.

  ![Licenza assegnata correttamente](media/active-directory-licensing-group-assignment-azure-portal/license-correctly-assigned.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle funzionalità disponibili per la gestione delle licenze tramite i gruppi, vedere:

* [Che cosa sono le licenze basate sui gruppi in Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Identificazione e risoluzione dei problemi relativi alle licenze per un gruppo in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Come eseguire la migrazione di singoli utenti con licenza alle licenze basate sui gruppi in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Scenari aggiuntivi relativi alle licenze basate sui gruppi in Azure Active Directory](active-directory-licensing-group-advanced.md)

