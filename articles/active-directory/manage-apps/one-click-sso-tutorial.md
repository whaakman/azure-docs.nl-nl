---
title: Een één klikt u op eenmalige aanmelding configureren voor uw toepassing uit de galerie met Azure AD-app | Microsoft Docs
description: Stappen voor het configureren van één klikt u op eenmalige aanmelding voor uw toepassing uit de galerie met Azure AD-app.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: e0416991-4b5d-4b18-89bb-91b6070ed3ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 358240823da469551e254356fc0613bea20d78c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057846"
---
# <a name="one-click-sso-feature-for-azure-ad-gallery-applications"></a>Een functie van klikt u op eenmalige aanmelding voor toepassingen van Azure AD-galerie

 In deze zelfstudie leert u hoe u voor het uitvoeren van één klikt u op eenmalige aanmelding voor alle SAML-toepassingen die gebruikersinterface voor configuratie van eenmalige aanmelding bieden.

## <a name="introduction-to-one-click-sso"></a>Inleiding tot eenmalige aanmelding met één klik

Één klik SSO-functie is ingevoerd voor het configureren van de eenmalige aanmelding voor Azure AD-galerie-apps die ondersteuning bieden voor SAML-protocol. We bieden deze optie om toe te staan onze klanten de metagegevens van de Azure AD aan de toepassing om automatisch te configureren op de configuratiepagina van Azure AD-eenmalige aanmelding. Het doel is om SSO snel instellen met minimale inspanningen van handmatige klanten te helpen. 

## <a name="advantages-of-the-one-click-sso"></a>Voordelen van de gebruiker klikt u op eenmalige aanmelding

- Snelle SSO-configuratie van de met galerietoepassingen waarbij klanten zich moeten handmatige installatie aan toepassing doen.
- Meer efficiënte en nauwkeurige manier van de configuratie.
- Er is geen communicatie van de partner of ondersteuning nodig is voor de installatie als de toepassing biedt de gebruikersinterface voor de SAML-configuratie.

## <a name="prerequisites"></a>Vereisten

- Actief-abonnement van de toepassing met de referenties van de beheerder die u wilt configureren met OneClick SSO.
- **Mijn browserextensie Apps beveiligde aanmelding** van Microsoft is geïnstalleerd in de browser. Als u wilt meer informatie over deze extensie, verwijzen naar dit [koppeling](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access).

## <a name="one-click-sso-feature-step-by-step-details"></a>Stapsgewijze details zijn uitgerust met één klik op eenmalige aanmelding

1. De toepassing uit de galerie met Azure AD-App toevoegen.

2. Klik op eenmalige aanmelding.

3. Klik op het inschakelen van eenmalige aanmelding.

4. Vul de verplichte configuratiewaarden in de sectie SAML-basisconfiguratie.

    > [!NOTE] 
    > Als de toepassing moet de configuratie van aangepaste claims, kunt u ze configureren voordat u OneClick SSO.

5. Als één klik SSO-functie is geïmplementeerd voor elke toepassing in de galerie, ziet u de volgende scherm. Als de **mijn Apps beveiligde aanmelding browserextensie** is nog niet is geïnstalleerd, u moet klikken op **de extensie installeren** optie.

    ![Mijn Apps beveiligen Sign-in de browser-extensie installeren](./media/one-click-sso-tutorial/install-myappssecure-extension.png)

6. Na het toevoegen van de extensie in de browser, klikt u op **Setup toepassingsnaam** die wordt u doorgestuurd naar de beheerportal van de toepassing. U moet aanmelden als beheerder toegang te krijgen tot de toepassing.

    ![De naam van de Setup-toepassing](./media/one-click-sso-tutorial/setup-sso.png)

7. De uitbreiding van de browser wordt nu automatisch de toepassing configureren voor u. De bevestiging van uw eerst gevraagd als u wilt doorgaan. Klik op **Ja**.

    ![Opslaan van de automatisch ingevuld gegevens](./media/one-click-sso-tutorial/save-autopopulate.png)

    > [!NOTE]
    > Als een toepassing extra nagivation of stappen moet, ziet u de juiste berichten waarin u wordt gevraagd deze stappen uitvoert. 

8. Nadat de configuratie is voltooid, klikt u op **Ok** de wijzigingen op te slaan.

    ![Sla de gegevens automatisch ingevuld](./media/one-click-sso-tutorial/save-data.png)

9. Een geslaagde pop bevestigingsbericht wordt weergegeven en de SSO-instellingen zijn geconfigureerd. Vervolgens kunt u de toepassing testen.

    ![Eenmalige aanmelding geconfigureerd](./media/one-click-sso-tutorial/sso-configured.png)

10. Zodra de configuratie is voltooid is, wordt de toepassing wordt afgemeld en keert u terug naar Azure portal.

11. U kunt klikken op de knop testen om te testen de eenmalige aanmelding.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
* [Wat is mijn Apps beveiligde aanmelding browserextensie](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access)
 