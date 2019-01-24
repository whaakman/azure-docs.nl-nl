---
title: 'Zelfstudie: Configureren van LinkedIn Sales Navigator voor automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting van gebruikersaccounts voor LinkedIn Sales Navigator ongedaan maken.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: daveba
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.openlocfilehash: 3e8d77dd0796d289c4f1c93f88e93756518ec244
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823569"
---
# <a name="tutorial-configure-linkedin-sales-navigator-for-automatic-user-provisioning"></a>Zelfstudie: LinkedIn Sales Navigator voor het automatisch inrichten van gebruikers configureren


Het doel van deze zelfstudie moet u de stappen die u uitvoeren in de LinkedIn Sales Navigator en Azure AD wilt voor het automatisch inrichten en verwijdering van gebruikersaccounts uit Azure AD voor LinkedIn Sales Navigator weergeven. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Directory-tenant
*   Een tenant LinkedIn Sales Navigator 
*   Een administrator-account in LinkedIn Sales Navigator met toegang tot het Accountcentrum LinkedIn

> [!NOTE]
> Azure Active Directory kan worden geïntegreerd met het gebruik van LinkedIn Sales Navigator de [SCIM](http://www.simplecloud.info/) protocol.

## <a name="assigning-users-to-linkedin-sales-navigator"></a>Gebruikers toewijzen voor LinkedIn Sales Navigator

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot LinkedIn Sales Navigator vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen voor LinkedIn Sales Navigator door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-sales-navigator"></a>Belangrijke tips voor het toewijzen van gebruikers voor LinkedIn Sales Navigator

*   Het wordt aanbevolen dat één Azure AD-gebruiker worden toegewezen voor LinkedIn Sales Navigator voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker voor LinkedIn Sales Navigator, moet u de **gebruiker** rol in het dialoogvenster toewijzing. De rol 'standaardtoegang' werkt niet voor het inrichten.


## <a name="configuring-user-provisioning-to-linkedin-sales-navigator"></a>Configuratie van gebruikers inrichten voor LinkedIn Sales Navigator

In deze sectie helpt u bij het verbinden van uw Azure AD voor het gebruikersaccount voor LinkedIn Sales Navigator SCIM Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen toegewezen gebruikersaccounts in LinkedIn Sales Navigator op basis van gebruiker en de groepstoewijzing van de in Azure AD.

> [!TIP]
> U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor LinkedIn Sales Navigator, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen.


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-sales-navigator-in-azure-ad"></a>Het configureren van automatische inrichten van gebruikersaccounts voor LinkedIn Sales Navigator in Azure AD:


De eerste stap is om op te halen van uw LinkedIn-toegangstoken. Als u een Enterprise-beheerder bent, kunt u zelf een toegangstoken inrichten. In het accountcentrum, gaat u naar **instellingen &gt; globale instellingen** en open de **SCIM Setup** deelvenster.

> [!NOTE]
> Als u het accountcentrum rechtstreeks in plaats van via een koppeling opent, kunt u bereiken met behulp van de volgende stappen uit.

1)  Aanmelden bij Account Center.

2)  Selecteer **Admin &gt; beheerdersinstellingen** .

3)  Klik op **geavanceerde integraties** op de linkerzijbalk. U bent omgeleid naar het accountcentrum.

4)  Klik op **+ toevoegen nieuwe SCIM configuratie** en volg de procedure door in elk veld te vullen.

> Wanneer autoassign licenties niet is ingeschakeld, betekent dit dat alleen de gegevens van de gebruiker is gesynchroniseerd.

![LinkedIn Sales Navigator inrichten](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

> Wanneer autolicense toewijzing is ingeschakeld, moet u de toepassingsexemplaar en het licentietype. Licenties zijn toegewezen op een eerst komt, eerst dienen uit te voeren totdat alle licenties zijn genomen.

![LinkedIn Sales Navigator inrichten](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5)  Klik op **token genereren**. U ziet nu de toegang tot token wordt weergegeven onder de **toegangstoken** veld.

6)  Sla uw toegangstoken op het Klembord of de computer voordat u de pagina verlaat.

7) Vervolgens maakt u zich aanmeldt bij de [Azure-portal](https://portal.azure.com), en blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

8) Als u LinkedIn Sales Navigator voor eenmalige aanmelding al hebt geconfigureerd, kunt u zoeken naar uw exemplaar van LinkedIn Sales Navigator met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **LinkedIn Sales Navigator** in de toepassingengalerie. LinkedIn Sales Navigator selecteren in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

9)  Selecteer uw exemplaar van LinkedIn Sales Navigator en selecteer vervolgens de **Provisioning** tabblad.

10) Stel de **Inrichtingsmodus** naar **automatische**.

![LinkedIn Sales Navigator inrichten](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11)  Vul de volgende velden onder **beheerdersreferenties** :

* In de **Tenant-URL** veld https://api.linkedin.com.

* In de **geheim Token** veld, voer het toegangstoken die u in stap 1 in en klikt u op **testverbinding** .

* U ziet een melding succes upperright aan van de portal.

12) Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het onderstaande selectievakje in.

13) Klik op **Opslaan**. 

14) In de **kenmerktoewijzingen** sectie, controleert u de gebruikers- en kenmerken die voor LinkedIn Sales Navigator van Azure AD worden gesynchroniseerd. Houd er rekening mee dat de kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen wordt gebruikt zodat deze overeenkomen met de gebruikersaccounts en groepen in LinkedIn Sales Navigator voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

![LinkedIn Sales Navigator inrichten](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15) Wijzigen zodat de Azure AD-inrichtingsservice voor LinkedIn Sales Navigator de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

16) Klik op **Opslaan**. 

Hiermee start u de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen voor LinkedIn Sales Navigator in de sectie gebruikers en groepen. Houd er rekening mee dat de eerste synchronisatie langer dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden duurt als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, die alle acties die worden uitgevoerd door de provisioning-service op uw app LinkedIn Sales Navigator beschrijven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
