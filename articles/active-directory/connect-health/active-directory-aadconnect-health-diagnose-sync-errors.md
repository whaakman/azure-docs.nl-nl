---
title: Azure AD Connect Health - sporen gedupliceerd synchronisatiefouten kenmerk | Microsoft Docs
description: Dit document beschrijft het proces spoor dubbel kenmerk synchronisatie van fouten en mogelijke oplossing van het zwevende object scenario's rechtstreeks vanuit de Azure-portal.
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
ms.openlocfilehash: 0a345fd3443fb33d6f5912c8a04677091e20ecc8
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="duplicate-attribute-sync-errors-diagnosis-and-remediation"></a>Dubbele kenmerk synchronisatie fouten diagnose en herstel 

## <a name="overview"></a>Overzicht
Nog een stapje verder markering voor synchronisatiefouten duurt, introduceert Azure Active Directory Connect Health een ervaring bij herstel bij selfservice voor synchronisatiefouten dubbel kenmerk en het oplossen van zwevende objecten vanuit Azure AD. Het belangrijkste voordeel van de functie diagnose:
- Diagnostische procedure uit om te beperken dubbel kenmerk sync fout scenario's en geven aan dat specifieke oplossingen bieden
- Oplossing voor specifieke scenario's van Azure AD los de fout in een enkele klik toepassen
- Er is geen upgrade of de configuratie is vereist voor deze functie inschakelt.
Lees meer informatie over Azure AD [gedupliceerd herstelmogelijkheden](https://aka.ms/dupattributeresdocs).

## <a name="problems"></a>Problemen
### <a name="common-scenario"></a>Gangbare scenario
Wanneer **QuarantinedAttributeValueMustBeUnique** en **AttributeValueMustBeUnique** synchronisatiefouten optreden, wordt meestal User Principal-naam of proxyadressen conflict Zie in Azure AD. U mag de synchronisatiefouten oplossen door het bijwerken van het bronobject conflicterende premises zijde. De synchronisatiefout wordt na de volgende synchronisatie worden opgelost. Bijvoorbeeld de volgende afbeelding geeft aan dat twee gebruikers hebben met het conflict van hun UserPrincipalName als *Joe.J@contoso.com*. De conflicterende objecten in Azure AD in quarantaine zijn geplaatst. 

![Algemene synchronisatiescenario fout onderzoeken](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Zwevende object scenario
Soms misschien vindt u dat een bestaande gebruiker verliest Bronanker. De verwijdering van bronobject opgetreden in on-premises AD, maar de wijziging van de verwijdering signaal nooit is gesynchroniseerd naar Azure AD. Dit kan gebeuren oorzaken zoals synchronisatie-engine probleem of domein migratie. Wanneer u hetzelfde object hebt hersteld of opnieuw worden gemaakt, is logisch bestaande gebruiker moet de gebruiker om te synchroniseren vanaf de Bronanker. Voor bestaande gebruiker als het enige object cloud u ziet ook conflicterende gebruiker gesynchroniseerd naar Azure AD en gesynchroniseerd aan het bestaande object niet overeenkomt. Er is geen directe manier Bronanker opnieuw toewijzen. Meer informatie over de [bestaande KB](https://support.microsoft.com/help/2647098). Bijvoorbeeld, bewaart het bestaande object in Azure AD de licentie van Joe. Nieuw gesynchroniseerd object met verschillende bronanker is opgetreden in gedupliceerde kenmerk status in Azure AD. Wijzigingen van Jan op on-premises AD niet kan worden toegepast op Jan de oorspronkelijke gebruiker (bestaande object) in Azure AD.  

![Fout-zwevende object synchronisatiescenario onderzoeken](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Diagnose en probleemoplossing van de stappen in Connect Health 
Diagnose functie ondersteunt gebruikersobjecten met de volgende dubbele kenmerken:

| Naam kenmerk | Synchronisatietype fout|
| ------------------ | -----------------|
| UserPrincipalName | QuarantinedAttributeValueMustBeUnique of AttributeValueMustBeUnique | 
| ProxyAddresses | QuarantinedAttributeValueMustBeUnique of AttributeValueMustBeUnique | 
| sipProxyAddress | AttributeValueMustBeUnique | 
| onPremiseSecurityIdentifier |  AttributeValueMustBeUnique |

>[!IMPORTANT]
> Hiervoor **globale beheerder** machtiging of **Inzender** van RBAC-instellingen kunnen toegang tot deze functie. 
>

Volgende stappen uit Azure-portal zich u de foutdetails sync beperken en bieden meer specifieke oplossingen:

![Diagnose van synchronisatie fout onderzoeken stappen](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixSteps.png)

Vanuit de Azure-portal kunt u zich kunt doorlopen een paar stappen voor het identificeren van specifieke corrigeerbare scenario's:  
1.  In de kolom status spoor wordt zien de status als er een potentiële problemen stromen beperken het geval van de fout en mogelijk oplossen rechtstreeks vanuit de Azure Active Directory.
| Status | Wat betekent dit? |
| ------------------ | -----------------|
| Niet gestart | U hebt dit proces diagnose niet bezocht. Is afhankelijk van het diagnostische resultaat er is mogelijk een manier om de synchronisatiefout vanuit de portal rechtstreeks kan verhelpen. |
| Handmatige oplossing vereist | De fout past niet in de criteria van beschikbare correcties vanuit de portal. De aanvraag kan worden (1) conflicterende object typen niet gebruikers zijn (2) u al hebben doorlopen diagnostische stappen en er geen oplossing beschikbaar is via de portal oplossen. In dit geval steeds fix van on-premises kant nog een van de oplossingen. [Meer informatie over lokale oplossing](https://support.microsoft.com/help/2647098) | 
| In afwachting van synchronisatie | Correctie is toegepast. Er wordt gewacht tot de volgende synchronisatiecyclus om te wissen van de fout. |
>[!IMPORTANT]
> De kolom diagnostische status worden ingesteld na elke synchronisatiecyclus. 
>

2.  Door te klikken op **spoor** knop in de blade fout details hebt enkele vragen beantwoorden en identificeren van de details van de synchronisatie-fout. De vragen te beantwoorden kunt het geval van zwevende object scenario identificeren. 

3.  Als er een **sluiten** knop aan het einde van de diagnostische gegevens, betekent dit dat er geen oplossing snel beschikbaar vanuit de portal op basis van de opgegeven antwoorden. Raadpleeg de oplossing wordt weergegeven in de laatste stap. FIX van on-premises nog steeds de oplossingen. Nadat de gebruiker op de knop sluiten, vindt u de status van de huidige synchronisatiefout gaat worden **handmatige oplossing vereist**. De status behouden tijdens de huidige synchronisatiecyclus.

4.  Zodra het zwevende object geval wordt geïdentificeerd, wordt u mogelijk zijn om op te lossen de gedupliceerde kenmerken synchronisatiefouten rechtstreeks vanuit de portal. Klik op de **toepassen los** knop voor het activeren van het proces. De status van de huidige synchronisatiefout wordt bijgewerkt om te worden **in afwachting van synchronisatie**.

5.  De fout na de volgende synchronisatiecyclus moet worden verwijderd uit de lijst.

## <a name="how-to-answer-the-diagnosis-questions"></a>Hoe de diagnose vragen beantwoorden 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>Bestaat de gebruiker in uw on-premises Active Directory?

De vraag wordt geprobeerd om te identificeren bronobject van bestaande on-premises Active Directory-gebruiker.  
1.  Controleer of uw Active Directory een object met de opgegeven UserPrincipalName heeft. Zo Nee, beantwoorden Nee.
2.  Zo ja, controleer dan of het object nog steeds in het bereik is voor Synchroniseren.  
  - Zoek in het Azure AD-connectorgebied met de DN.
  - Als het object is gevonden met de **in behandeling toevoegen** status, het antwoord Nee. Azure AD Connect kan geen verbinding maken met het object van het juiste AD-Object.
  - Als het object niet gevonden is, klik op Ja.

> Het volgende diagram duurt, bijvoorbeeld de vraag wordt geprobeerd om te bepalen of *Joe Jackson* nog steeds aanwezig zijn in on-premises Active Directory.
Voor **veelvoorkomende scenario**, beide gebruiker *Joe Johnson* en *Joe Jackson* aanwezig zijn in uw on-premises Active Directory. Er zijn twee verschillende gebruikers in quarantaine geplaatste objecten.

![Algemene synchronisatiescenario fout onderzoeken](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

> Voor **zwevende object scenario**, slechts één gebruiker – *Joe Johnson* aanwezig zijn in on-premises Active Directory:

![Fout-zwevende object synchronisatiescenario onderzoeken](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

### <a name="do-both-these-accounts-belong-to-the-same-user"></a>Horen deze beide accounts bij dezelfde gebruiker?
De vraag controleert binnenkomende conflicterende gebruiker en het bestaande gebruikersobject in Azure AD om te controleren of ze deel uitmaken van dezelfde gebruiker.  
1.  Conflicterende object is zojuist gesynchroniseerd met Azure Active Directory. Vergelijken van het object in de:  
  - Weergavenaam
  - User principal name
  - Object-id
2.  Als het mislukt om ze te vergelijken, controleert u dat uw Active Directory-objecten met de opgegeven UserPrincipalNames heeft. Nee als beide worden gevonden.  

> In het onderstaande geval de twee objecten voor dezelfde gebruiker behoort *Joe Johnson*.

![Fout-zwevende object synchronisatiescenario onderzoeken](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)


## <a name="what-happened-after-fix-is-applied-for-orphaned-object-scenario"></a>Wat is er gebeurd nadat de oplossing wordt toegepast voor zwevende object scenario
Op basis van de antwoorden van verhoogde vragen, kunt u zich kunt zien **toepassen los** knop als er een oplossing beschikbaar is via de Azure AD. In dit geval wordt de op lokaal object wordt gesynchroniseerd met een onverwachte Azure AD-object. De twee objecten zijn toegewezen met behulp van 'Bronanker'. De wijziging toepassen voert de stappen uit zoals:
- Bij Bronanker worden bijgewerkt naar het juiste object in Azure AD.
- Als dit account, moet u de conflicterende object verwijderen in Azure AD.

![Synchronisatiefout sporen na het herstel](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixAfterFix.png)

>[!IMPORTANT]
> De wijziging toepassen los gelden alleen voor de gevallen zwevende object.
>

Na de bovengenoemde stappen volgen zich de gebruiker toegang tot de oorspronkelijke bron, met de koppeling naar een bestaand object. De **Diagnose status** waarde in de lijstweergave worden bijgewerkt zodat deze worden **in behandeling Sync**. Synchronisatiefout wordt na de volgende synchronisatie worden opgelost. Connect Health opgelost synchronisatiefout in de lijst niet meer weergegeven. 


## <a name="faq"></a>Veelgestelde vragen
 -  Wat is er gebeurd als de uitvoering van de van toepassing is mislukt?  
Als de uitvoering is mislukt, is het mogelijk Azure AD Connect exporteren-fout op het moment dat wordt uitgevoerd. De portal-pagina vernieuwen en probeer het opnieuw na de volgende synchronisatie. De synchronisatiecyclus standaardwaarde is 30 minuten. 

 -  Wat gebeurt er als de **bestaand object** moet het object wilt verwijderen?  
Als het bestaande object in dat geval moet worden verwijderd, is het proces geen gebruikgemaakt van Bronanker is gewijzigd. U moet kunnen om dit te herstellen van uw on-premises AD.  

 -  Wat is de machtiging voor de gebruiker kan de oplossing toepassen?  
Globale beheerder of bijdrager van RBAC-instellingen worden gemachtigd voor toegang tot de diagnose en de procedure voor probleemoplossing.

 -  Ik moet config AAD Connect of Azure AD Connect Health-agent voor deze functie bijwerken?  
Nee, diagnose-proces is een complete cloud-gebaseerde functie.

 -  Als het bestaande object voorlopig verwijderd is, wordt het proces spoor terugzetten het object opnieuw actief zijn?  
Nee, wordt de oplossing niet bijwerken voor objectkenmerk dan Bronanker. 
