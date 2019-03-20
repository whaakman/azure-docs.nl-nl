---
title: Azure AD Connect Health - Diagnose dubbel kenmerk synchronisatiefouten | Microsoft Docs
description: Dit document beschrijft het proces diagnose van de synchronisatiefouten dubbel kenmerk en een mogelijke oplossing van het zwevende object scenario's rechtstreeks vanuit de Azure portal.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: maheshu
editor: billmath
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: zhiweiw
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbdeef7c591221756ad206bf2f3dd78ac3d26c4f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57885314"
---
# <a name="diagnose-and-remediate-duplicated-attribute-sync-errors"></a>Vaststellen en herstellen van de synchronisatiefouten dubbel kenmerk

## <a name="overview"></a>Overzicht
Meer doen met één stap duurt om te markeren synchronisatiefouten, introduceert Azure Active Directory (Azure AD) Connect Health zelf te verhelpen. Het Hiermee lost u dubbel kenmerk synchronisatiefouten en correcties van objecten die zijn zwevende van Azure AD.
De functie voor diagnose heeft tot deze voordelen:
- Het biedt een diagnostische procedure die de taalinstelling van de synchronisatiefouten dubbel kenmerk. En het specifieke oplossingen biedt.
- Het is van toepassing een oplossing voor specifieke scenario's van Azure AD om op te lossen van de fout in één stap.
- Er is geen upgrade of de configuratie is vereist voor deze functie inschakelen.
Zie voor meer informatie over Azure AD, [duplicatiekenmerken identificeren voor synchronisatie- en](how-to-connect-syncservice-duplicate-attribute-resiliency.md).

## <a name="problems"></a>Problemen met
### <a name="a-common-scenario"></a>Een veelvoorkomend scenario
Wanneer **QuarantinedAttributeValueMustBeUnique** en **AttributeValueMustBeUnique** synchronisatiefouten optreden, is het gebruikelijk om te zien een **UserPrincipalName** of **Proxyadressen** conflict in Azure AD. U kunt de synchronisatiefouten oplossen door het bijwerken van de conflicterende bronobject vanaf de on-premises. De synchronisatiefout worden opgelost na de volgende synchronisatie. Deze afbeelding geeft bijvoorbeeld aan dat twee gebruikers beschikken over een conflict van hun **UserPrincipalName**. Beide zijn **Joe.J\@contoso.com**. De conflicterende objecten in Azure AD in quarantaine zijn geplaatst.

![Algemeen scenario voor synchronisatie fout vaststellen](./media/how-to-connect-health-diagnose-sync-errors/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Zwevende object scenario
Soms is het wellicht dat een bestaande gebruiker verliest de **Bronanker**. Het verwijderen van het bronobject is er gebeurd in on-premises Active Directory. Maar de wijziging van het signaal van de verwijdering niet is gesynchroniseerd met Azure AD. Dit verlies gebeurt voor redenen, zoals problemen met synchronisatie-engine of migreren. Als hetzelfde object wordt teruggezet of opnieuw worden gemaakt, logisch, een bestaande gebruiker moet de gebruiker te synchroniseren vanaf de **Bronanker**. 

Als een bestaande gebruiker een alleen-cloud-object is, kunt u ook zien dat de conflicterende gebruiker gesynchroniseerd met Azure AD. De gebruiker is niet gesynchroniseerd naar het bestaande object overeen. Er is geen directe manier opnieuw toewijzen de **Bronanker**. Zie meer informatie over de [bestaande kennisdatabase](https://support.microsoft.com/help/2647098). 

Het bestaande object in Azure AD bewaart als u bijvoorbeeld de licentie van Joe. Een nieuw gesynchroniseerd object met een andere **Bronanker** vindt plaats in de status van een dubbel kenmerk in Azure AD. Wijzigingen voor Jaap in on-premises Active Directory niet toegepast op Jaaps oorspronkelijke gebruiker (bestaande object) in Azure AD.  

![Fout-zwevende object synchronisatiescenario vaststellen](./media/how-to-connect-health-diagnose-sync-errors/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Diagnostische gegevens en de stappen in Connect Health voor probleemoplossing 
De functie diagnose biedt ondersteuning voor gebruikersobjecten met de volgende dubbele kenmerken:

| De naam van kenmerk | Typen voor synchronisatie|
| ------------------ | -----------------|
| UserPrincipalName | QuarantinedAttributeValueMustBeUnique of AttributeValueMustBeUnique | 
| ProxyAddresses | QuarantinedAttributeValueMustBeUnique of AttributeValueMustBeUnique | 
| SipProxyAddress | AttributeValueMustBeUnique | 
| OnPremiseSecurityIdentifier |  AttributeValueMustBeUnique |

>[!IMPORTANT]
> Toegang tot deze functie **globale beheerder** machtiging, of **Inzender** toestemming van de RBAC-instellingen is vereist.
>

Volg de stappen van de Azure portal om te beperken de foutdetails voor synchronisatie en meer specifieke oplossingen te bieden:

![Synchronisatie fout diagnose stappen](./media/how-to-connect-health-diagnose-sync-errors/IIdFixSteps.png)

Uitvoeren vanuit de Azure-portal, een paar stappen voor het identificeren van specifieke corrigeerbare scenario's:  
1.  Controleer de **Diagnose status** kolom. De status wordt aangegeven als er een manier om op te lossen van een synchronisatiefout rechtstreeks uit Azure Active Directory. Een probleemoplossing stroom bestaat die met andere woorden, kan het geval is fout verfijnen en op te lossen mogelijk.

| Status | Wat betekent dit? |
| ------------------ | -----------------|
| Niet gestart | U kunt dit proces diagnose nog niet hebt bezocht. Afhankelijk van het diagnostisch resultaat is er een mogelijke manier om op te lossen de synchronisatiefout rechtstreeks vanuit de portal. |
| Handmatige oplossing vereist | De fout niet aanpassen aan de criteria van de beschikbare oplossingen in de portal. Een van beide conflicterende objecttypen zijn niet-gebruikers, of u al de diagnostische stappen doorlopen en geen oplossing die oplossing is beschikbaar via de portal. In dat laatste geval is een oplossing vanaf de on-premises nog steeds een van de oplossingen. [Meer informatie over on-premises oplossingen](https://support.microsoft.com/help/2647098). | 
| In afwachting van synchronisatie | Een oplossing is toegepast. De portal wacht op de volgende synchronisatiecyclus om te wissen van de fout. |

  >[!IMPORTANT]
  > De statuskolom van de diagnostische worden opnieuw ingesteld na elke synchronisatiecyclus. 
  >

1. Selecteer de **vaststellen** knop onder de foutdetails. U een aantal vragen beantwoorden en identificeren van de details van de synchronisatie-fout. Antwoorden op de vragen kunt een aanvraag zwevende object identificeren.

1. Als een **sluiten** knop wordt weergegeven aan het einde van de diagnostische gegevens, er is geen snelle correctie beschikbaar is via de portal op basis van uw antwoorden. Raadpleeg de oplossing wordt weergegeven in de laatste stap. Oplossingen van on-premises zijn nog steeds de oplossingen. Selecteer de **sluiten** knop. De status van de huidige synchronisatiefout schakelt over naar de **handmatige oplossing vereist**. De status blijft tijdens de huidige synchronisatiecyclus.

1. Nadat een aanvraag zwevende object wordt geïdentificeerd, kunt u de dubbele kenmerken synchronisatiefouten rechtstreeks vanuit de portal oplossen. Voor het activeren van het proces, selecteer de **op oplossing toepassen** knop. De status van de huidige synchronisatie fout updates voor **in afwachting van synchronisatie**.

1. Na de volgende synchronisatiecyclus, moet de fout worden verwijderd uit de lijst.

## <a name="how-to-answer-the-diagnosis-questions"></a>Hoe u de diagnose vragen beantwoorden 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>Bestaat de gebruiker in uw on-premises Active Directory?

Deze vraag wordt geprobeerd om de bronobject van de bestaande gebruiker van on-premises Active Directory te identificeren.  
1. Controleer of Azure Active Directory een object met de opgegeven heeft **UserPrincipalName**. Zo niet, antwoord **Nee**.
2. Als dit wel gebeurt, controleert u of het object nog steeds binnen het bereik voor het synchroniseren is.  
   - Zoeken in de Azure AD-connectorgebied met behulp van de DN-naam.
   - Als het object is gevonden de **toevoegen in behandeling** status, beantwoorden **Nee**. Azure AD Connect kan geen verbinding met het maken van het object op het juiste Azure AD-object.
   - Als het object niet wordt gevonden, beantwoorden **Ja**.

In deze voorbeelden wordt de vraag wordt geprobeerd om te identificeren of **Joe Jackson** nog steeds aanwezig in on-premises Active Directory.
Voor de **veelvoorkomend scenario**, beide gebruikers **Joe Johnson** en **Joe Jackson** aanwezig zijn in on-premises Active Directory. Er zijn twee verschillende gebruikers in quarantaine geplaatste objecten.

![Algemeen scenario voor synchronisatie fout vaststellen](./media/how-to-connect-health-diagnose-sync-errors/IIdFixCommonCase.png)

Voor de **zwevende object scenario**, alleen de één gebruiker **Joe Johnson** aanwezig is in on-premises Active Directory:

![Zwevende synchronisatieobject fout diagnosticeren * gebruiker bestaat * scenario](./media/how-to-connect-health-diagnose-sync-errors/IIdFixOrphanedCase.png)

### <a name="do-both-of-these-accounts-belong-to-the-same-user"></a>Horen beide van deze accounts bij dezelfde gebruiker?
Deze vraag controleert een binnenkomende conflicterende gebruiker en het bestaande gebruikersobject in Azure AD om te zien als ze deel uitmaken van dezelfde gebruiker.  
1. De conflicterende object wordt pas gesynchroniseerd met Azure Active Directory. Vergelijk de objecten kenmerken:  
   - Weergavenaam
   - User principal name
   - Object-id
2. Als Azure AD om ze te vergelijken is mislukt, controleert u of Active Directory-objecten met de opgegeven heeft **UserPrincipalNames**. Antwoord **Nee** als u beide vinden.

In het volgende voorbeeld wordt de twee objecten deel uitmaken van dezelfde gebruiker **Joe Johnson**.

![Zwevende synchronisatieobject fout diagnosticeren * dezelfde gebruiker * scenario](./media/how-to-connect-health-diagnose-sync-errors/IIdFixOrphanedCase.png)


## <a name="what-happens-after-the-fix-is-applied-in-the-orphaned-object-scenario"></a>Wat gebeurt er wanneer de oplossing wordt toegepast in het scenario voor zwevende object
Op basis van de voorgaande vragen worden beantwoord, ziet u de **op oplossing toepassen** knop wanneer er een oplossing beschikbaar is via Azure AD. In dit geval de on-premises object gesynchroniseerd met een onverwacht Azure AD-object. De twee objecten worden toegewezen met behulp van de **Bronanker**. De **op oplossing toepassen** wijziging wordt in deze of gelijksoortige stappen:
1. Updates de **Bronanker** op het juiste object in Azure AD.
2. Hiermee verwijdert u de conflicterende object in Azure AD, indien aanwezig.

![Synchronisatiefout nadat het probleem vaststellen](./media/how-to-connect-health-diagnose-sync-errors/IIdFixAfterFix.png)

>[!IMPORTANT]
> De **op oplossing toepassen** wijziging geldt alleen voor zwevende object gevallen.
>

Nadat de voorgaande stappen hebt kan de gebruiker toegang tot de oorspronkelijke bron, die een koppeling naar een bestaand object. De **Diagnose status** waarde in de lijstweergave met updates voor **synchronisatie in behandeling**. De synchronisatiefout worden opgelost na de volgende synchronisatie. Verbinding maken met status wordt niet langer weergeven de opgelost synchronisatiefout in de lijstweergave.

## <a name="failures-and-error-messages"></a>Fouten en foutberichten
**Gebruiker met een conflicterend kenmerk is voorlopig verwijderd uit de Azure Active Directory. Zorg ervoor dat de gebruiker is moeilijk worden verwijderd voordat de nieuwe pogingen.**  
De gebruiker met een conflicterend kenmerk in Azure AD moet worden opgeschoond voordat u de oplossing kunt toepassen. Bekijk [permanent verwijderen van de gebruiker in Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-restore) voordat opnieuw wordt geprobeerd de oplossing. De gebruiker wordt ook automatisch verwijderd na 30 dagen definitief voorlopig verwijderde status heeft. 

**Bijwerken bronanker voor cloud-gebaseerde gebruiker in uw tenant wordt niet ondersteund.**  
Cloud-gebaseerde gebruiker in Azure AD mag geen bronanker hebben. Bijwerken bronanker wordt in dit geval niet ondersteund. Handmatige oplossing is vereist vanuit on-premises. 

## <a name="faq"></a>Veelgestelde vragen
**V:** Wat gebeurt er als de uitvoering van de **op oplossing toepassen** mislukt?  
**A:** Als de uitvoering is mislukt, is het mogelijk dat Azure AD Connect een exportfout wordt uitgevoerd. Vernieuw de portal-pagina en probeer het opnieuw na de volgende synchronisatie. De standaard-synchronisatiecyclus is 30 minuten. 


**V:** Wat gebeurt er als de **bestaande object** moet het object moet worden verwijderd?  
**A:** Als de **bestaande object** moet worden verwijderd, het proces heeft geen betrekking op een wijziging van **Bronanker**. Meestal kunt u het oplossen van on-premises Active Directory. 


**V:** Welke machtigingen heeft een gebruiker nodig om toe te passen van de oplossing?  
**A:** **Globale beheerder**, of **Inzender** uit de RBAC-instellingen, toegang heeft tot de diagnostische gegevens en de procedure voor probleemoplossing.


**V:** Heb ik Azure AD Connect configureren of bijwerken van de Azure AD Connect Health-agent voor deze functie?  
**A:** Nee, de diagnose-proces is een complete cloud-gebaseerde functie.


**V:** Als het bestaande object voorlopig verwijderd is, wordt het proces diagnose dat het object active opnieuw?  
**A:** Niet de oplossing kenmerken van het object niet, dan bijwerken **Bronanker**.
