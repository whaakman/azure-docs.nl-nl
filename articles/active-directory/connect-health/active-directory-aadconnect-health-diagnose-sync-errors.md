---
title: Azure AD Connect Health - sporen gedupliceerd synchronisatiefouten kenmerk | Microsoft Docs
description: Dit document beschrijft het proces diagnose van synchronisatiefouten dubbel kenmerk en een mogelijke oplossing van het zwevende object scenario's rechtstreeks vanuit de Azure-portal.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: billmath
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: zhiweiw
ms.openlocfilehash: 4a6e0924492c26c9bad4ed0af207ad9764c3cc5c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831894"
---
# <a name="diagnose-and-remediate-duplicated-attribute-sync-errors"></a>Analyseren en herstellen van de synchronisatiefouten dubbel kenmerk

## <a name="overview"></a>Overzicht
Duurt een stapje verder synchronisatiefouten markeren, introduceert Azure Active Directory (Azure AD) Connect Health zelf te verhelpen. Het Hiermee lost u dubbele kenmerk synchronisatiefouten en correcties van objecten die zijn zwevende van Azure AD.
De functie diagnose heeft deze voordelen:
- Het biedt een diagnostische procedure die wordt beperkt omlaag synchronisatiefouten dubbel kenmerk. En geeft specifieke oplossingen.
- Voorbeeld van Azure AD omzetten van de fout in één stap een oplossing voor specifieke scenario's.
- Er is geen upgrade of de configuratie is vereist voor deze functie inschakelt.
Zie voor meer informatie over Azure AD [tolerantie voor synchronisatie en dubbel kenmerk](https://aka.ms/dupattributeresdocs).

## <a name="problems"></a>Problemen
### <a name="a-common-scenario"></a>Een veelvoorkomend scenario
Wanneer **QuarantinedAttributeValueMustBeUnique** en **AttributeValueMustBeUnique** synchronisatiefouten optreden, is het gebruikelijk om te zien een **UserPrincipalName** of **Proxyadressen** conflict in Azure AD. De synchronisatiefouten kan mogelijk worden opgelost door de conflicterende bronobject vanaf de lokale bij te werken. De synchronisatiefout wordt na de volgende synchronisatie worden opgelost. Bijvoorbeeld: deze afbeelding geeft aan dat twee gebruikers conflicterende hebben hun **UserPrincipalName**. Beide zijn **Joe.J@contoso.com**. De conflicterende objecten in Azure AD in quarantaine zijn geplaatst.

![Algemene synchronisatiescenario fout onderzoeken](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Zwevende object scenario
In sommige gevallen kan het kan gebeuren dat een bestaande gebruiker verliest de **Bronanker**. Het verwijderen van het bronobject opgetreden in de lokale Active Directory. Maar de wijziging van de verwijdering signaal nooit is gesynchroniseerd naar Azure AD. Dit verlies gebeurt redenen zoals problemen met synchronisatie-engine of domeinmigratie. Wanneer hetzelfde object opgehaald hersteld of opnieuw, logisch gemaakt, een bestaande gebruiker moet de gebruiker te synchroniseren vanaf de **Bronanker**. 

Wanneer een bestaande gebruiker een object alleen in de cloud is, ziet u ook de conflicterende gebruiker gesynchroniseerd naar Azure AD. De gebruiker niet synchroon aan het bestaande object overeen. Er is geen directe manier opnieuw toewijzen de **Bronanker**. Zie voor meer informatie over de [bestaande kennisdatabase](https://support.microsoft.com/help/2647098). 

Het bestaande object in Azure AD bewaart als u bijvoorbeeld de licentie van Joe. Een nieuw gesynchroniseerd object met een andere **Bronanker** optreedt in een dubbele kenmerk-status in Azure AD. Wijzigingen voor Jan in de lokale Active Directory worden niet toegepast op Jan de oorspronkelijke gebruiker (bestaande object) in Azure AD.  

![Fout-zwevende object synchronisatiescenario onderzoeken](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Diagnose en probleemoplossing van de stappen in Connect Health 
De functie spoor ondersteunt gebruikersobjecten met de volgende dubbele kenmerken:

| Naam van kenmerk | Synchronisatietype fout|
| ------------------ | -----------------|
| UserPrincipalName | QuarantinedAttributeValueMustBeUnique of AttributeValueMustBeUnique | 
| ProxyAddresses | QuarantinedAttributeValueMustBeUnique of AttributeValueMustBeUnique | 
| sipProxyAddress | AttributeValueMustBeUnique | 
| onPremiseSecurityIdentifier |  AttributeValueMustBeUnique |

>[!IMPORTANT]
> Toegang tot deze functie **globale beheerder** toestemming hebben, of **Inzender** machtiging uit de RBAC-instellingen is vereist.
>

Volg de stappen in de Azure portal de foutdetails sync beperken en bieden meer specifieke oplossingen:

![Synchronisatie fout diagnose stappen](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixSteps.png)

Vanuit de Azure-portal een paar stappen nemen om specifieke scenario's voor corrigeerbare bepalen:  
1.  Controleer de **Diagnose status** kolom. De status wordt aangegeven als er een manier om op te lossen een synchronisatiefout rechtstreeks vanuit de Azure Active Directory. Met andere woorden, bestaat een stroom voor het oplossen van problemen die kunnen beperken van de foutaanvraag en dit mogelijk oplossen.
| Status | Wat betekent dit? |
| ------------------ | -----------------|
| Niet gestart | U kunt dit proces diagnose nog niet hebt bezocht. Er is een mogelijke manier om op te lossen de synchronisatiefout rechtstreeks vanuit de portal, afhankelijk van de diagnostische resultaat. |
| Handmatige oplossing vereist | De fout past niet de criteria van oplossingen beschikbaar vanuit de portal. Beide conflicterende objecttypen zijn geen gebruikers, of u al de diagnostische stappen doorlopen en geen oplossing oplossing is beschikbaar via de portal. In dat geval wordt nog een oplossing vanaf de lokale steeds een van de oplossingen. [Meer informatie over lokale oplossingen](https://support.microsoft.com/help/2647098). | 
| In afwachting van synchronisatie | Een oplossing is toegepast. De portal wacht op de volgende synchronisatiecyclus om te wissen van de fout. |
  >[!IMPORTANT]
  > De kolom diagnostische status wordt opnieuw ingesteld na elke synchronisatiecyclus. 
  >

2.  Selecteer de **spoor** knop onder de foutdetails. U moet enkele vragen beantwoorden en identificeren van de details van de synchronisatie-fout. Antwoorden op de vragen te identificeren van een aanvraag zwevende object.

3.  Als een **sluiten** knop wordt weergegeven aan het einde van de diagnostische gegevens, er is geen snelle correctie beschikbaar vanuit de portal op basis van uw antwoorden. Raadpleeg de oplossing wordt weergegeven in de laatste stap. Oplossingen van on-premises nog steeds de oplossingen. Selecteer de **sluiten** knop. De status van de huidige synchronisatiefout verandert in **handmatige oplossing vereist**. De status blijft tijdens de huidige synchronisatiecyclus.

4.  Nadat een aanvraag zwevende object is geïdentificeerd, kunt u de gedupliceerde kenmerken synchronisatiefouten rechtstreeks vanuit de portal oplossen. Om het proces activeert, selecteer de **toepassen los** knop. De status van de huidige synchronisatie fout updates **in afwachting van synchronisatie**.

5.  De fout na de volgende synchronisatiecyclus moet worden verwijderd uit de lijst.

## <a name="how-to-answer-the-diagnosis-questions"></a>Hoe de diagnose vragen beantwoorden 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>De gebruiker bestaat in uw lokale Active Directory?

Deze vraag probeert te identificeren van het bronobject van de bestaande gebruiker van de lokale Active Directory.  
1.  Controleer of de Azure Active Directory een object met de opgegeven is **UserPrincipalName**. Als dit niet het geval is, beantwoorden **Nee**.
2.  Als dit het geval is, controleert u of het object nog steeds binnen het bereik van het synchroniseren is.  
  - Zoeken in de ruimte van Azure AD-connector met de DN-naam.
  - Als het object is gevonden de **in behandeling toevoegen** status, beantwoorden **Nee**. Azure AD Connect kan geen verbinding van het object met het recht Azure AD-object.
  - Als het object is niet gevonden, beantwoorden **Ja**.

In deze voorbeelden wordt de vraag probeert om te identificeren of **Joe Jackson** nog bestaat in de lokale Active Directory.
Voor de **veelvoorkomende scenario**, beide gebruikers **Joe Johnson** en **Joe Jackson** aanwezig zijn in de lokale Active Directory. Er zijn twee verschillende gebruikers in quarantaine geplaatste objecten.

![Algemene synchronisatiescenario fout onderzoeken](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

Voor de **zwevende object scenario**, alleen de gebruiker die één **Joe Johnson** aanwezig is in de lokale Active Directory:

![Zwevende object sync fout onderzoeken * gebruiker bestaat * scenario](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

### <a name="do-both-of-these-accounts-belong-to-the-same-user"></a>Behoren zowel deze accounts voor dezelfde gebruiker?
Deze vraag controleert een binnenkomende conflicterende gebruiker en het bestaande gebruikersobject in Azure AD om te zien als ze tot dezelfde gebruiker behoren.  
1.  Het object conflicterende is zojuist gesynchroniseerd met Azure Active Directory. Vergelijk de objecten kenmerken:  
  - Weergavenaam
  - User principal name
  - Object-id
2.  Als Azure AD om ze te vergelijken is mislukt, Controleer of Active Directory-objecten met de opgegeven **UserPrincipalNames**. Antwoord **Nee** als u beide vinden.

In het volgende voorbeeld worden de twee objecten behoren voor dezelfde gebruiker **Joe Johnson**.

![Zwevende object sync fout onderzoeken * dezelfde gebruiker * scenario](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)


## <a name="what-happens-after-the-fix-is-applied-in-the-orphaned-object-scenario"></a>Wat gebeurt er wanneer het herstel is toegepast in het scenario voor zwevende object
Op basis van de antwoorden op de voorgaande vragen, ziet u de **toepassen los** knop wanneer er een oplossing beschikbaar is vanuit Azure AD. In dit geval wordt het lokale object wordt gesynchroniseerd met een onverwachte Azure AD-object. De twee objecten zijn toegewezen met behulp van de **Bronanker**. De **toepassen los** wijziging wordt van deze of gelijksoortige stappen:
1. Updates de **Bronanker** naar het juiste object in Azure AD.
2. Verwijdert het conflicterende object in Azure AD, indien aanwezig.

![Synchronisatiefout sporen na het herstel](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixAfterFix.png)

>[!IMPORTANT]
> De **toepassen los** wijziging geldt alleen zwevende object.
>

Na de voorgaande stappen hebt uitgevoerd, kan de gebruiker toegang tot de oorspronkelijke bron een koppeling naar een bestaand object is. De **Diagnose status** waarde in de lijstweergave bijgewerkt tot **in behandeling Sync**. De synchronisatiefout wordt na de volgende synchronisatie worden opgelost. Verbinding maken met status wordt niet langer gekomen de opgelost synchronisatiefout in de lijstweergave.


## <a name="faq"></a>Veelgestelde vragen
**V:** Wat gebeurt er als de uitvoering van de **toepassen los** mislukt?  
**A:** Als de uitvoering is mislukt, is het mogelijk dat Azure AD Connect een exportfout wordt uitgevoerd. De portal-pagina vernieuwen en probeer het opnieuw na de volgende synchronisatie. De standaard synchronisatiecyclus is 30 minuten. 


**V:** Wat gebeurt er als de **bestaand object** moet het object wilt verwijderen?  
**A:** Als de **bestaand object** moet worden verwijderd, het proces niet leidt tot een wijziging van **Bronanker**. Meestal kunt u het oplossen van de lokale Active Directory. 


**V:** Welke machtiging moet een gebruiker de oplossing toepassen?  
**A:** **Globale beheerder**, of **Inzender** uit de instellingen RBAC toegang heeft tot de diagnose en de procedure voor probleemoplossing.


**V:** Heb ik Azure AD Connect configureren of bijwerken van de Azure AD Connect Health-agent voor deze functie?  
**A:** Nee, is het proces diagnose een volledige cloud-gebaseerde functie.


**V:** Als het bestaande object voorlopig verwijderd is, wordt het proces diagnose activeren het object opnieuw?  
**A:** Niet de oplossing objectkenmerken Nee, anders dan bijgewerkt **Bronanker**.
