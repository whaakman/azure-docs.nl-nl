---
title: Gebruikers inrichten beheer voor zakelijke apps in Azure Active Directory | Microsoft Docs
description: Informatie over het beheren van gebruikers account inrichten voor zakelijke apps met de Azure Active Directory
services: active-directory
documentationcenter: 
author: asmalser
manager: mtillman
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 122dbdb838377a36020f9ec692b38544004e676c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Het beheer van gebruikersaccount inrichten voor zakelijke apps in Azure portal
In dit artikel wordt beschreven hoe u de [Azure-portal](https://portal.azure.com) voor het beheren van automatische account gebruikersaanvragen en verwijderen van gegevens voor toepassingen die ondersteuning bieden voor deze, met name lijsten die zijn toegevoegd in de categorie 'aanbevolen' van de [Azure Active Directory-toepassingsgalerie](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Zie voor meer informatie over automatische account gebruikersaanvragen en hoe het werkt, [gebruikersaanvragen automatiseren en Deprovisioning voor SaaS-toepassingen met Azure Active Directory](active-directory-saas-app-provisioning.md).

## <a name="finding-your-apps-in-the-portal"></a>Uw apps te zoeken in de portal
Alle toepassingen die zijn geconfigureerd voor eenmalige aanmelding in een map met een directory-beheerder met behulp van de [Azure Active Directory-toepassingsgalerie](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), kan worden bekeken en beheerd in de [Azure-portal](https://portal.azure.com). De toepassingen vindt u in de **meer Services** &gt; **bedrijfstoepassingen** sectie van de portal. Zakelijke apps zijn apps die zijn geïmplementeerd en worden gebruikt binnen uw organisatie.

![Blade bedrijfstoepassingen][0]

Als u de **alle toepassingen** koppeling aan de linkerkant geeft een lijst met alle apps die zijn geconfigureerd, met inbegrip van apps die waren toegevoegd uit de galerie. Als u een app laadt de resourceblade voor die app, waarbij de rapporten kunnen worden weergegeven voor die app en een aantal instellingen kan worden beheerd.

Instellingen voor het inrichten gebruikersaccount kan worden beheerd door te selecteren **inrichten** aan de linkerkant.

![De resource-blade toepassing][1]

## <a name="provisioning-modes"></a>Inrichting modi
De **inrichten** blade begint met een **modus** menu ziet u welke inrichting modi worden ondersteund voor een zakelijke toepassing en kan ze worden geconfigureerd. De beschikbare opties zijn onder andere:

* **Automatische** -deze optie wordt weergegeven als Azure AD ondersteunt automatische inrichting op basis van een API en/of de inrichting van gebruikersaccounts aan deze toepassing. Als u deze modus wordt weergegeven een interface waarmee beheerders via Azure AD verbinding maken met de toepassing Gebruikersbeheer API configureren, Accounttoewijzingen en werkstromen die definiëren hoe de gegevens van gebruikersaccounts moeten stromen tussen Azure AD maken en de app en het beheren van de Azure AD-service inricht.
* **Handmatige** -deze optie wordt weergegeven als Azure AD biedt geen ondersteuning voor automatische inrichting van gebruikersaccounts aan deze toepassing. Deze optie betekent dat account gebruikersrecords opgeslagen in de toepassing moeten worden beheerd met behulp van een extern proces, op basis van de gebruiker beheer en inrichting mogelijkheden van die toepassing (met inbegrip van SAML-compileerprogramma inrichting).

## <a name="configuring-automatic-user-account-provisioning"></a>Configuratie van automatische gebruikers account inrichten
Als u de **automatische** optie wordt weergegeven, een scherm weergegeven dat is onderverdeeld in vier secties:

### <a name="admin-credentials"></a>Referenties voor beheerder
Dit is waar de referenties nodig voor Azure AD verbinding maken met de toepassing Gebruikersbeheer API worden ingevoerd. De invoer vereist clientdownload is afhankelijk van de toepassing. Zie voor meer informatie over de vereisten voor specifieke toepassingen en het type referentie, de [zelfstudie voor de configuratie voor de specifieke toepassing](active-directory-saas-app-provisioning.md).

Als u de **verbinding testen** knop kunt u de referenties testen door Azure AD poging verbinding maken met de app de app met behulp van de opgegeven referenties inrichten.

### <a name="mappings"></a>Toewijzingen
Dit is waar beheerders kunnen weergeven en bewerken welke kenmerken gebruikersstroom tussen Azure AD en de doeltoepassing wanneer gebruikersaccounts worden ingericht of bijgewerkt.

Er is een vooraf geconfigureerde verzameling toewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten van elke SaaS-app. Sommige apps beheren andere soorten objecten, zoals groepen of contactpersonen. Selecteer een van de toewijzingen in de tabel ziet u de editor voor kolomtoewijzing aan de rechterkant, waar ze kunnen worden bekeken en aangepast.

![De resource-blade toepassing][2]

Ondersteunde aanpassingen zijn onder andere:

* Inschakelen en uitschakelen van de toewijzingen voor specifieke objecten, zoals het Azure AD-gebruikersobject in de SaaS-app gebruikersobject.
* Bewerken welke kenmerken stromen van het gebruikersobject Azure AD naar de app gebruikersobject. Zie voor meer informatie over toewijzing van kenmerken [kenmerk toewijzing typen](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).
* Filter de inrichting acties die Azure AD worden uitgevoerd op de doeltoepassing. In plaats van Azure AD volledig-objecten synchroniseren, kunt u de acties uitgevoerd beperken. Bijvoorbeeld door het selecteren van **Update**, Azure AD-alleen updates bestaande gebruikers-accounts in een toepassing en geen nieuwe maakt. Door het selecteren van **maken**, Azure alleen nieuwe gebruikersaccounts maakt, maar wordt de bestaande bestanden niet bijgewerkt. Deze functie kan beheerders verschillende toewijzingen voor het maken van een account maken en bijwerken van werkstromen.

### <a name="settings"></a>Instellingen
Deze sectie kan beheerders starten en stoppen van de Azure AD-service voor de geselecteerde toepassing inricht, evenals eventueel de inrichting cache wissen en de service opnieuw starten.

Als inrichting wordt ingeschakeld voor het eerst uitvoert voor een toepassing, schakelt u de service door het wijzigen van de **inrichting Status** naar **op**. Dit zorgt ervoor dat de Azure AD-service inricht om uit te voeren voor een eerste synchronisatie waar deze de gebruikers die zijn toegewezen leest in de **gebruikers en groepen** sectie, zoekt de doeltoepassing voor hen en voert u de inrichting acties die zijn gedefinieerd in de Azure AD **toewijzingen** sectie. Tijdens dit proces slaat de inrichting service in de cache opgeslagen gegevens over welke accounts voor gebruikers die worden beheerd, zodat niet-beheerde accounts in de doeltoepassingen die nooit binnen het bereik van de toewijzing zijn niet worden beïnvloed door de inrichting ongedaan bewerkingen. Na de eerste synchronisatie met de inrichting service automatisch worden gebruikers en groepsobjecten op een interval van tien minuten.

Wijzigen van de **inrichting Status** naar **uit** gewoon de inrichting-service wordt onderbroken. In deze status Azure niet maken, bijwerken of verwijderen van een gebruiker of groepsobjecten worden weergegeven in de app. Wijzigen van de status tot on zorgt ervoor dat de service om op te halen waarop dit werd afgebroken.

Selecteren van de **huidige status wissen en opnieuw starten van synchronisatie** checkbox en opslaan de inrichting-service gestopt, dumpbestanden gegevens over welke Azure AD-accounts in de cache wordt beheerd, de services opnieuw gestart en de initiële synchronisatie opnieuw wordt uitgevoerd. Deze optie kan beheerders het inrichtingsproces voor de implementatie opnieuw starten.

### <a name="synchronization-details"></a>Synchronisatiedetails
Deze sectie bevat aanvullende informatie over de werking van de inrichting service, inclusief de eerste en laatste keer dat de inrichting-service wordt uitgevoerd op de toepassing en hoeveel gebruikers en groepsobjecten worden beheerd.

Vindt u koppelingen naar de **inrichting activiteitenrapport**, waarmee u een logboek van alle gebruikers en groepen hebt gemaakt, bijgewerkt en verwijderd tussen Azure AD en de doeltoepassing en de **inrichting foutenrapport** waarmee u meer gedetailleerde foutberichten worden weergegeven voor gebruiker en groepsobjecten die niet worden gelezen konden, gemaakt, bijgewerkt of verwijderd. 

##<a name="feedback"></a>Feedback

We hopen u zoals uw Azure AD-ervaring. Zorg ervoor dat uw feedback! Plaats uw feedback en ideeën voor verbetering van de **beheerportal** sectie van onze [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  We Doe enthousiast bent over het bouwen van cool nieuw per dag uit, en gebruik van de richtlijnen voor vorm en definiëren wat we hierna bouwen.


[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
