---
title: Probleem gebruikers inrichten tot een toepassing-galerie van Azure AD configureren | Microsoft Docs
description: Het oplossen van algemene problemen bij het configureren van gebruikers inrichten tot een toepassing die al in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 44e344095352f2bc6b27e389fc8be2cdf3e368d8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-user-provisioning-to-an-azure-ad-gallery-application"></a>Probleem gebruikers inrichten tot een toepassing-galerie van Azure AD configureren

Configureren van [automatisch gebruikers inrichten](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) voor een app (indien ondersteund), vereist dat de specifieke instructies voor het voorbereiden van de aanvraag om automatische inrichting worden gevolgd. Vervolgens kunt u de Azure-portal voor het configureren van de inrichting service om te synchroniseren van gebruikersaccounts aan de toepassing.

U moet altijd eerst de setup-zelfstudie specifiek zijn voor het instellen van de inrichting voor uw toepassing te zoeken. Volg deze stappen voor het configureren van de app en de Azure AD om het inrichtingsproces verbinding te maken. Een lijst met app-zelfstudies kan worden gevonden op [lijst zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

## <a name="how-to-see-if-provisioning-is-working"></a>Hoe ziet u of inrichting werkt 

Zodra de service is geconfigureerd, kunnen de meeste inzicht in de werking van de service worden getekend vanaf twee locaties:

-   **Controlelogboeken** – de inrichting auditlogboeken record alle bewerkingen die worden uitgevoerd door de inrichting service, inclusief het opvragen van Azure AD voor gebruikers die binnen het bereik van de inrichting zijn toegewezen. Query uitvoeren op de doel-app voor de aanwezigheid van gebruikers, vergelijken van de gebruikersobjecten tussen het systeem. Vervolgens toevoegen, bijwerken of het gebruikersaccount in het doelsysteem op basis van de vergelijking uitschakelen. De inrichting controlelogboeken kunnen worden geopend in de Azure-portal in de **Azure Active Directory &gt; zakelijke Apps &gt; \[toepassingsnaam\] &gt; Audit logboeken** tabblad. Filter de logboeken op de **Account inrichten** categorie alleen de inrichting om gebeurtenissen te bekijken voor die app.

-   **Inrichtingsstatus –** een samenvatting van de laatste inrichting uitvoeren voor een bepaalde app worden weergegeven in de **Azure Active Directory &gt; zakelijke Apps &gt; \[toepassingsnaam\] &gt; Inrichting** sectie aan de onderkant van het scherm met de service-instellingen. Deze sectie bevat een overzicht van hoeveel gebruikers (en/of groepen) momenteel worden gesynchroniseerd tussen de twee systemen, en als er geen fouten bevat. Details van fouten worden in de controlelogboeken. Houd er rekening mee dat de status van de inrichting niet mogen worden gevuld totdat een volledige initiële synchronisatie tussen Azure AD is voltooid en de app.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Algemene probleemgebieden bij inrichting rekening te houden

Hieronder volgt een lijst van de algemene probleemgebieden die u inzoomen kunt in als u een idee van waar u wilt beginnen.

* [Inrichting van de service niet starten](#provisioning-service-does-not-appear-to-start)
* [Kan de configuratie vanwege app-referenties niet werkt niet opslaan.](#can’t-save-configuration-due-to-app-credentials-not-working)
* [Controlelogboeken spreken gebruikers zijn 'overgeslagen' en niet is ingericht, zelfs als ze zijn toegewezen](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Inrichting van de service niet starten

Als u de **inrichting Status** worden **op** in de **Azure Active Directory &gt; zakelijke Apps &gt; \[toepassingsnaam\] &gt;Inrichten** sectie van de Azure-portal. Er zijn geen andere statusgegevens zijn echter op die pagina weergegeven nadat daaropvolgende wordt opnieuw geladen. Is het waarschijnlijk dat de service wordt uitgevoerd, maar niet nog een initiële synchronisatie is voltooid. Controleer de **controlelogboeken** die hierboven worden beschreven om te bepalen welke bewerkingen die de service wordt uitgevoerd, en als er geen fouten bevat.

>[!NOTE]
>Een initiële synchronisatie kan overal van 20 minuten tot enkele uren duren, afhankelijk van de grootte van de Azure AD-directory en het aantal gebruikers in het bereik voor het inrichten. Volgende synchronisaties na de eerste synchronisatie worden sneller, omdat de inrichting service watermerken die de status van beide systemen na de eerste synchronisatie slaat, verbeterde prestaties van de volgende synchronisaties vertegenwoordigen.
>
>

## <a name="cant-save-configuration-due-to-app-credentials-not-working"></a>Kan de configuratie vanwege app-referenties niet werkt niet opslaan.

Azure AD vereist in de volgorde voor het inrichten als u wilt werken, geldige referenties waarmee deze verbinding maken met de API geleverd door de app voor het Gebruikersbeheer van een. Als deze referenties niet werkt, of u niet weet wat die ze zijn, raadpleegt u de zelfstudie voor het instellen van deze app, die eerder zijn beschreven.

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Controlelogboeken dat gebruikers worden overgeslagen en niet is ingericht, hoewel ze zijn toegewezen

Wanneer een gebruiker wordt weergegeven als 'overgeslagen' in de controlelogboeken, is het belangrijk dat u de uitgebreide details in het logboekbericht om de reden vast te lezen. Hieronder vindt u veelvoorkomende oorzaken en oplossingen:

-   **Een filter scope is geconfigureerd** **die wordt uitgefilterd de gebruiker op basis van een kenmerkwaarde**. Zie voor meer informatie over het bereikfilters <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **De gebruiker is 'niet effectief heeft het recht'.** Als u dit specifieke foutbericht ziet, is dit omdat er een probleem met de gebruikersrecord toewijzing opgeslagen in Azure AD. Los dit probleem, toewijzing de gebruiker (of groep) uit de app en opnieuw opnieuw toewijzen. Zie voor meer informatie over toewijzing <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Een vereist kenmerk is ontbreekt of niet ingevuld voor een gebruiker.** Een belangrijkste waarmee u rekening moet houden bij het instellen van de inrichting worden om te bekijken en configureren van de kenmerktoewijzingen en werkstromen die welke gebruiker (of groep) eigenschappen stroom van Azure AD voor de toepassing definiëren. Dit omvat de 'overeenkomende eigenschap' instelt met worden gebruikt voor unieke identificatie dient en overeen met gebruikers/groepen tussen de twee systemen. Zie voor meer informatie over dit proces belangrijk <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.

   * **Kenmerktoewijzingen voor groepen:** inrichten van de groepsnaam en de groepsgegevens, naast de leden, als dit wordt ondersteund op bepaalde toepassingen. U kunt inschakelen of uitschakelen van deze functionaliteit met in- of uitschakelen de **toewijzing** voor een groepsobjecten worden weergegeven in de **inrichten** tabblad. Als inrichting groepen is ingeschakeld, worden de kenmerktoewijzingen om te controleren of dat het juiste veld wordt gebruikt voor de 'overeenkomende ID' Lees. Dit is de weergegeven naam of e-mailalias), zoals de groep en de bijbehorende leden niet worden ingericht als de overeenkomende eigenschap leeg of niet ingevuld voor een groep in Azure AD is.

#<a name="next-steps"></a>Volgende stappen
[Gebruiker inrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](active-directory-saas-app-provisioning.md)
