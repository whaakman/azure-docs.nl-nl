---
title: Probleem bij het inrichten van gebruikers naar een toepassing in Azure AD-galerie configureren | Microsoft Docs
description: Het oplossen van veelvoorkomende problemen met het configureren gebruikers in te richten al een toepassing die worden vermeld in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: cf496e448cbc11b9e986ca3b58c956c4cd18a34e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58113449"
---
# <a name="problem-configuring-user-provisioning-to-an-azure-ad-gallery-application"></a>Probleem bij het inrichten van gebruikers naar een Azure AD-galerie-toepassing configureren

Configureren van [automatisch gebruikers inrichten](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) voor een app (indien ondersteund), vereist dat specifieke instructies voor het voorbereiden van de toepassing voor automatische inrichting worden uitgevoerd. Vervolgens kunt u de Azure-portal de inrichtingsservice om te synchroniseren van gebruikersaccounts aan de toepassing wilt configureren.

U moet altijd eerst zoeken naar de setup-zelfstudie die specifiek zijn voor het instellen van de inrichting voor uw toepassing. Volg deze stappen voor het configureren van de app en de Azure AD de inrichting verbinding te maken. Een lijst met zelfstudies voor app kan worden gevonden op [lijst met zelfstudies over het SaaS-Apps integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

## <a name="how-to-see-if-provisioning-is-working"></a>Hoe om te zien als het inrichten werkt 

Nadat de service is geconfigureerd, kunnen de meeste inzicht in de werking van de service worden getekend vanaf twee plaatsen:

-   **Auditlogboeken** – de inrichting auditlogboeken beschikbaar zijn alle bewerkingen die worden uitgevoerd door de provisioning-service, met inbegrip van query's van Azure AD voor gebruikers die zich binnen het bereik toegewezen voor het inrichten van record. Query uitvoeren op de doel-app voor het bestaan van gebruikers, vergelijken van de gebruikersobjecten van het systeem. Vervolgens toevoegen, bijwerken of het gebruikersaccount in het doelsysteem op basis van de vergelijking uitschakelen. De inrichting auditlogboeken kunnen worden geopend in de Azure-portal in de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt; controleren logboeken** tabblad. De logboeken filteren op de **Accountinrichting** categorie alleen de inrichten om gebeurtenissen te bekijken voor die app.

-   **Inrichtingsstatus –** een samenvatting van de laatste inrichting uitvoeren voor een bepaalde app kan worden weergegeven de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt;Inrichting** sectie aan de onderkant van het scherm onder de service-instellingen. In deze sectie bevat een overzicht van het aantal gebruikers (en/of groepen) worden momenteel gesynchroniseerd tussen de twee systemen, en als er fouten zijn. Details van deze fout zich in de auditlogboeken. Houd er rekening mee dat de Inrichtingsstatus niet mogen worden gevuld totdat een volledige initiële synchronisatie is voltooid tussen Azure AD en de app.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Algemene probleemgebieden met inrichting u rekening moet houden

Hieronder volgt een lijst van de algemene probleemgebieden die u inzoomen kunt op als u een idee van waar u hebben wilt beginnen.

* [Inrichting van de service niet wordt weergegeven om te starten](#provisioning-service-does-not-appear-to-start)
* Kan de configuratie vanwege app-referenties niet werkt niet opslaan
* [Auditlogboeken zegt gebruikers zijn 'overgeslagen' en niet is ingericht, zelfs als ze zijn toegewezen](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Inrichting van de service niet wordt weergegeven om te starten

Als u de **Inrichtingsstatus** moet **op** in de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt;Provisioning** sectie van de Azure portal. Er zijn geen andere statusgegevens zijn echter op die pagina weergegeven nadat volgende geladen. Is het waarschijnlijk dat de service wordt uitgevoerd, maar niet nog een eerste synchronisatie is voltooid. Controleer de **auditlogboeken** die hierboven worden beschreven om te bepalen welke bewerkingen die de service wordt uitgevoerd, en als er fouten zijn.

>[!NOTE]
>Een eerste synchronisatie kan duren vanaf 20 minuten tot enkele uren, afhankelijk van de grootte van de Azure AD-map en het aantal gebruikers in het bereik voor het inrichten. Volgende synchronisaties na de initiële synchronisatie worden sneller, zoals de inrichtingsservice watermerken die staan voor de status van beide systemen na de initiële synchronisatie, het verbeteren van de prestaties van de volgende synchronisatie worden opgeslagen.
>
>

## <a name="cant-save-configuration-due-to-app-credentials-not-working"></a>Kan de configuratie vanwege app-referenties niet werkt niet opslaan

In de volgorde voor het inrichten als u wilt werken, vereist Azure AD geldige referenties die toe te staan om verbinding maken met de API geleverd door de app voor het Gebruikersbeheer van een. Als deze referenties niet werken, of u niet weet wat ze zijn, raadpleegt u de zelfstudie voor het instellen van deze app, die eerder zijn beschreven.

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Auditlogboeken dat gebruikers worden overgeslagen en niet ingericht zelfs als ze zijn toegewezen

Wanneer een gebruiker wordt weergegeven als 'overgeslagen' in de controlelogboeken, is het heel belangrijk dat u de uitgebreide details in het logboekbericht om de reden vast te lezen. Hieronder vindt u veelvoorkomende oorzaken en oplossingen:

- **Een bereikfilter is geconfigureerd** **die is gefilterd op de gebruiker op basis van een kenmerkwaarde**. Zie voor meer informatie over het bereikfilters <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

- **De gebruiker is "niet effectief recht".** Als u dit bericht specifieke fout ziet, is omdat er een probleem met de gebruiker een record die zijn opgeslagen in Azure AD. Los dit probleem, toewijzing de gebruiker (of groep) uit de app en weer opnieuw toewijzen. Zie voor meer informatie over toewijzing <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

- **Een vereist kenmerk is ontbreekt of niet ingevuld voor een gebruiker.** Een belangrijke dingen die u moet overwegen bij het instellen van de inrichting worden om te controleren en configureren van de kenmerktoewijzingen en werkstromen die welke gebruiker (of groep) eigenschappen stroom van Azure AD aan de toepassing definiëren. Dit omvat instellen van de 'overeenkomende eigenschap"die worden gebruikt voor het uniek identificeren en overeenkomen met de gebruikers/groepen tussen de twee systemen. Zie voor meer informatie over dit belangrijk proces <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.

  * **Kenmerktoewijzingen voor groepen:** Inrichting van de groepsnaam en de Groepsdetails, naast de leden, als dit wordt ondersteund voor sommige toepassingen. U kunt inschakelen of uitschakelen van deze functionaliteit met in- of uitschakelen de **toewijzing** voor groepsobjecten worden weergegeven in de **Provisioning** tabblad. Als u groepen inrichten is ingeschakeld, worden de kenmerktoewijzingen om te controleren of dat het juiste veld wordt gebruikt voor de "overeenkomende ID" Lees. Dit is de weergegeven naam of e-mailalias), zoals de groep en de bijbehorende leden niet worden ingericht als de overeenkomende eigenschap leeg of niet ingevuld voor een groep in Azure AD is.

## <a name="next-steps"></a>Volgende stappen
[Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](user-provisioning.md) (Automatisch gebruikers inrichten en de inrichting ongedaan maken voor SaaS-toepassingen met Azure Active Directory)
