---
title: Configuratie van één klik, eenmalige aanmelding (SSO) van uw Azure Marketplace-toepassing | Microsoft Docs
description: Stappen voor één klik configuratie van eenmalige aanmelding voor uw toepassing vanuit Azure Marketplace.
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
ms.openlocfilehash: a83d27af4fd783b95c53ef3a9169cb72bfc29d34
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67872431"
---
# <a name="one-click-app-configuration-of-single-sign-on"></a>Met één klik appconfiguratie van eenmalige aanmelding

 In deze zelfstudie leert u hoe u voor het uitvoeren van één klik, eenmalige aanmelding (SSO) configuratie voor SAML-ondersteuning, Azure Active Directory (Azure AD)-toepassingen vanuit Azure Marketplace.

## <a name="introduction-to-one-click-sso"></a>Inleiding tot één muisklik eenmalige aanmelding

De functie voor één muisklik SSO is ontworpen voor het configureren van eenmalige aanmelding voor Azure Marketplace-apps die ondersteuning bieden voor SAML-protocol. Op de configuratiepagina van Azure AD-eenmalige aanmelding kunt deze optie u de Azure AD-metagegevens automatisch configureren aan de toepassing. In op deze manier kunt u snel instellen van eenmalige aanmelding met een minimale inspanning voor handmatige.

## <a name="advantages-of-one-click-sso"></a>Voordelen van één muisklik eenmalige aanmelding

- Snelle SSO-configuratie van Azure Marketplace-toepassingen waarvoor handmatige installatie op de toepassing aan clientzijde.
- SSO-configuratie, efficiënter en nauwkeurige.
- Geen communicatie van de partner of ondersteuning nodig is voor installatie. De gebruikersinterface van biedt de toepassing voor SAML-configuratie.

## <a name="prerequisites"></a>Vereisten

- Een actief abonnement van de toepassing te configureren met eenmalige aanmelding. U moet ook beheerdersreferenties.
- De **mijn Apps beveiligde aanmelding extensie** van Microsoft is geïnstalleerd in de browser. Zie voor meer informatie, [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access).

## <a name="one-click-sso-configuration-steps"></a>Stappen voor het configureren van eenmalige aanmelding één klik

1. De toepassing toevoegen vanuit de Azure Marketplace.

2. Selecteer **eenmalige aanmelding**.

3. Selecteer **eenmalige aanmelding inschakelen**.

4. Vullen van de van de verplichte configuratiewaarden in de **SAML-basisconfiguratie** sectie.

    > [!NOTE]
    > Als de toepassing aangepaste claims die u wilt configureren heeft, kunt u ze voordat u één muisklik SSO verwerken.

5. Als de functie voor één muisklik eenmalige aanmelding beschikbaar voor uw Azure Marketplace-toepassing is, ziet u de volgende scherm. Mogelijk hebt u voor het installeren van de **mijn Apps beveiligde aanmelding browserextensie** hiervoor **de extensie installeren**.

   ![Mijn Apps beveiligen Sign-in de browser-extensie installeren](./media/one-click-sso-tutorial/install-myappssecure-extension.png)

6. Nadat u de extensie aan de browser toevoegen, selecteert u **Setup \<toepassingsnaam\>** . Nadat u bent omgeleid naar de beheerportal van de toepassing, moet u zich aanmelden als beheerder.

   ![De naam van de Setup-toepassing](./media/one-click-sso-tutorial/setup-sso.png)

7. De Browseruitbreiding configureert automatisch de eenmalige aanmelding op de toepassing. Om te bevestigen **Ja**.

   ![Opslaan van de gegevens automatisch ingevuld](./media/one-click-sso-tutorial/save-autopopulate.png)

   > [!NOTE]
   > Als de configuratie van eenmalige aanmelding voor uw toepassing extra stappen uitvoeren vereist, de prompts om uit te voeren van de stappen te volgen.

8. Nadat de configuratie is voltooid, selecteert u **OK** de wijzigingen op te slaan.

   ![Sla de gegevens automatisch ingevuld](./media/one-click-sso-tutorial/save-data.png)

9. Er wordt een bevestigingsvenster weergegeven zodat u weet dat de SSO-instellingen zijn geconfigureerd.

   ![Eenmalige aanmelding geconfigureerd](./media/one-click-sso-tutorial/sso-configured.png)

10. Nadat de configuratie voltooid is, bent u afgemeld bij de toepassing en geretourneerd naar de Azure-portal.

11. U kunt selecteren **testen** voor het testen van eenmalige aanmelding.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
* [Wat is de extensie van de browser mijn Apps beveiligde aanmelding?](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access)
 
