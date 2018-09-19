---
title: Tolerantie voor synchronisatie- en kenmerk | Microsoft Docs
description: Nieuwe gedrag van het verwerken van objecten met de UPN of ProxyAddress conflicten tijdens de directorysynchronisatie met Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 537a92b7-7a84-4c89-88b0-9bce0eacd931
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f8fe86712b59a896d1f431ef677f592ced382db8
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46312072"
---
# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Tolerantie voor synchronisatie- en duplicatiekenmerken identificeren
Tolerantie van dubbele kenmerk is een functie in Azure Active Directory die wordt veroorzaakt door problemen elimineren **UserPrincipalName** en **ProxyAddress** veroorzaakt een conflict bij het uitvoeren van een van de Microsoft hulpprogramma's voor synchronisatie.

Deze twee kenmerken in het algemeen moeten uniek zijn in alle **gebruiker**, **groep**, of **Neem contact op met** objecten in een bepaalde Azure Active Directory-tenant.

> [!NOTE]
> Alleen gebruikers kunnen de UPN's hebben.
> 
> 

Het nieuwe gedrag waarmee deze functie is in de cloud-gedeelte van de synchronisatie-pijplijn, daarom is het client agnostische en relevant zijn voor een Microsoft-synchronisatie product met inbegrip van Azure AD Connect, DirSync en MIM-Connector. De algemene term 'synchronisatieclient' wordt gebruikt in dit document om weer te geven van een van deze producten.

## <a name="current-behavior"></a>Gedrag van het huidige
Als er een poging voor het inrichten van een nieuw object met een UPN of ProxyAddress-waarde die in strijd is met deze beperking voor uniekheid, blokkeert Azure Active Directory dat object worden gemaakt. Op dezelfde manier als een object wordt bijgewerkt met een niet-unieke UPN of ProxyAddress, mislukt de update. De inrichting poging of de update wordt opnieuw uitgevoerd door de synchronisatieclient bij elke cyclus exporteren en blijft mislukken, totdat het conflict opgelost is. Een e-mailbericht fout rapport wordt gegenereerd na elke poging en een fout wordt vastgelegd door de synchronisatieclient.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Probleem met de tolerantie van dubbel kenmerk
In plaats van volledig mislukken in te richten of bijwerken van een object met een dubbel kenmerk, Azure Active Directory 'in quarantaine plaatst' de dubbel kenmerk die de beperking voor uniekheid schendt. Als dit kenmerk vereist is voor het inrichten, zoals UserPrincipalName, wordt een tijdelijke aanduiding door de service toegewezen. De indeling van deze tijdelijke waarden is  
"***<OriginalPrefix>+ < 4DigitNumber > @<InitialTenantDomain>. onmicrosoft.com***'.  
Als het kenmerk niet vereist is, zoals een **ProxyAddress**, Azure Active Directory gewoon in quarantaine plaatst het conflict-kenmerk en wordt voortgezet met het maken van het object of de update.

Bij het kenmerk in quarantaine plaatsen, wordt informatie over het conflict in de dezelfde fout rapport e-mail die wordt gebruikt in het oude gedrag verzonden. Deze informatie wordt alleen weergegeven in het foutenrapport één keer als de quarantaine gebeurt, het is niet blijven echter moeten worden vastgelegd in toekomstige e-mailberichten. Ook, omdat de uitvoer van dit object is geslaagd, de synchronisatieclient Meld u niet een fout en probeert niet opnieuw voor het maken / bijwerken van de bewerking op de volgende synchronisatiecycli.

Ter ondersteuning van dit gedrag is een nieuw kenmerk toegevoegd aan de gebruiker, groep en neem contact op met object-klassen:  
**DirSyncProvisioningErrors**

Dit is een kenmerk met meerdere waarden dat wordt gebruikt voor het opslaan van de conflicterende kenmerken die schendt de beperking voor uniekheid moeten ze worden toegevoegd normaal. Een achtergrondtaak van de timer is ingeschakeld in Azure Active Directory, die elk uur naar dubbel kenmerk conflicten die zijn opgelost, en worden de kenmerken in kwestie automatisch verwijderd uit quarantaine wordt uitgevoerd.

### <a name="enabling-duplicate-attribute-resiliency"></a>Inschakelen van tolerantie van dubbel kenmerk
Tolerantie van dubbele kenmerk is het standaardgedrag voor alle Azure Active Directory-tenants. Dit is op die standaard voor alle tenants die ingeschakeld synchronisatie voor de eerste keer op 22 augustus 2016 of hoger. Tenants die ingeschakeld synchroniseren vóór deze datum is de functie is ingeschakeld in batches. Deze implementatie zal beginnen in September 2016 en een e-mailmelding verzonden naar contact op met technische berichtgeving van elke tenant met de specifieke datum wanneer de functie wordt ingeschakeld.

> [!NOTE]
> Nadat de tolerantie van dubbel-kenmerk is ingeschakeld kan niet worden uitgeschakeld.

Als u wilt controleren of de functie is ingeschakeld voor uw tenant, kunt u doen door de meest recente versie van de Azure Active Directory PowerShell-module downloaden en uitvoeren:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

> [!NOTE]
> U kunt de cmdlet Set-MsolDirSyncFeature niet meer gebruiken proactief de tolerantie van dubbele kenmerk-functie inschakelen voordat deze is ingeschakeld voor uw tenant. Als u de functie testen, moet u een nieuwe Azure Active Directory-tenant maken.

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Objecten met DirSyncProvisioningErrors identificeren
Er zijn momenteel twee methoden voor het identificeren van objecten die u deze fouten als gevolg van conflicten met dubbele eigenschap, Azure Active Directory PowerShell en de Office 365-beheerportal hebt. Er zijn plannen om uit te breiden naar extra portal op basis van rapportage in de toekomst.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
Het volgende is van de waarde true voor de PowerShell-cmdlets in dit onderwerp:

* Alle van de volgende cmdlets zijn hoofdlettergevoelig.
* De **– ErrorCategory PropertyConflict** moet altijd worden opgenomen. Er zijn momenteel geen andere typen **ErrorCategory**, maar dit kan in de toekomst worden uitgebreid.

Eerst aan de slag door te voeren **Connect-MsolService** en referenties in te voeren voor een tenantbeheerder.

Gebruik vervolgens de volgende cmdlets en operators om fouten op verschillende manieren weer te geven:

1. [Alles weergeven](#see-all)
2. [Door de eigenschapstype](#by-property-type)
3. [Door de conflicterende waarde](#by-conflicting-value)
4. [Met behulp van een tekenreeks zoeken](#using-a-string-search)
5. [Gesorteerd](#sorted)
6. [In een beperkt aantal of alle](#in-a-limited-quantity-or-all)

#### <a name="see-all"></a>Alles bekijken
Wanneer verbinding is gemaakt, een algemeen overzicht van het kenmerk inrichten uitvoeren fouten in de tenant:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Dit resulteert in een resultaat als volgt uit:  
 ![Get-MsolDirSyncProvisioningError](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  

#### <a name="by-property-type"></a>Door de eigenschapstype
Toevoegen als fouten wilt bekijken door de eigenschapstype, de **- PropertyName** markeren met de **UserPrincipalName** of **ProxyAddresses** argument:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

of

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Door de conflicterende waarde
Om te zien van fouten met betrekking tot een bepaalde eigenschap toevoegen de **- eigenschapwaarde** vlag (**- PropertyName** moet ook worden gebruikt bij het toevoegen van deze vlag):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`

#### <a name="using-a-string-search"></a>Met behulp van een tekenreeks zoeken
Een brede reeks search gebruik doen de **- SearchString** vlag. Dit kan worden gebruikt onafhankelijk van alle bovenstaande vlaggen, met uitzondering van **- ErrorCategory PropertyConflict**, die is vereist:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>In een beperkt aantal of alle
1. **MaxResults <Int>**  kan worden gebruikt om de query naar een specifiek aantal waarden te beperken.
2. **Alle** kan worden gebruikt om te controleren of alle resultaten worden opgehaald in het geval dat een groot aantal fouten bestaat.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Office 365-beheerportal
U kunt directory synchronisatiefouten weergeven in het Office 365-beheercentrum. Het rapport in de Office 365-beheerportal geeft alleen **gebruiker** objecten op waarvoor deze fouten. Het wordt niet weergegeven informatie over conflicten tussen **groepen** en **contactpersonen**.

![Actieve gebruikers](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/1234.png "actieve gebruikers")

Zie voor instructies over het weergeven van directory synchronisatie van fouten in het Office 365-beheercentrum [directory synchronisatiefouten in Office 365 identificeren](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).

### <a name="identity-synchronization-error-report"></a>Rapport over mappensynchronisatie fout identiteit
Wanneer een object met dubbel kenmerk conflict wordt verwerkt met dit nieuwe gedrag een melding is opgenomen in de standaard Identity synchronisatie foutenrapport e-mail die wordt verzonden naar de technische berichtgeving contact op met de knop voor de tenant. Er is echter een belangrijke wijziging in dit gedrag. In het verleden, zou informatie over een dubbel kenmerk conflict worden opgenomen in elke volgende foutrapport totdat het conflict is opgelost. Met dit nieuwe gedrag, wordt de foutmelding voor een bepaalde conflict alleen weergegeven eenmaal - op het moment dat de Conflicterend kenmerk in quarantaine is geplaatst.

Hier volgt een voorbeeld van hoe het e-mailmelding voor een conflict ProxyAddress uitziet:  
    ![Actieve gebruikers](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/6.png "actieve gebruikers")  

## <a name="resolving-conflicts"></a>Het oplossen van conflicten
Het oplossen van de strategie en de resolutie tactieken voor deze fouten moet niet verschillen van de manier waarop dubbel kenmerk fouten in het verleden zijn verwerkt. Het enige verschil is dat de taak timer vervalmechanisme via de tenant aan de servicezijde automatisch het kenmerk in kwestie aan het juiste object toevoegen nadat het conflict opgelost is.

Het volgende artikel geeft een overzicht van de verschillende strategieën gehanteerd oplossen van problemen: [dubbele of ongeldige kenmerken te voorkomen dat de directory-synchronisatie in Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Bekende problemen
Geen van deze bekende problemen zorgt ervoor dat gegevens verloren gaan of de service degradatie. Sommige hiervan zijn aesthetic, anderen ertoe leiden dat standaard "*vooraf tolerantie*" dubbel kenmerk fouten moeten worden gegenereerd in plaats van het conflict-kenmerk en een andere in quarantaine plaatsen zorgt ervoor dat bepaalde fouten vereist extra handmatig bijwerken.

**Core gedrag:**

1. Objecten met een specifiek kenmerk configuraties blijven ontvangen ze foutberichten uitvoer in plaats van de dubbele kenmerken in quarantaine wordt geplaatst.  
   Bijvoorbeeld:
   
    a. Nieuwe gebruiker wordt gemaakt in AD met een UPN van **Joe@contoso.com** en ProxyAddress **smtp:Joe@contoso.com**
   
    b. De eigenschappen van dit object in strijd zijn met een bestaande groep, waarbij ProxyAddress is **SMTP:Joe@contoso.com**.
   
    c. Bij het exporteren, een **ProxyAddress conflict** fout wordt gegenereerd in plaats van de kenmerken van het conflict in quarantaine geplaatst. De bewerking wordt opnieuw geprobeerd bij elke volgende synchronisatiecyclus als zijn zou voordat de tolerantiefunctie is ingeschakeld.
2. Als twee groepen zijn gemaakt van on-premises met hetzelfde SMTP-adres, een mislukt om in te richten op de eerste poging met een standard dubbele **ProxyAddress** fout. De dubbele waarde is echter goed in quarantaine op de volgende synchronisatiecyclus.

**Office-Portal rapport**:

1. Het gedetailleerde foutbericht voor twee objecten in een conflict UPN is hetzelfde. Hiermee wordt aangegeven dat ze hebben beide al hun UPN gewijzigd / in quarantaine geplaatst wanneer u in feite alleen een van deze waren voor alle gegevens zijn gewijzigd.
2. Het gedetailleerde foutbericht voor een conflict UPN ziet u de verkeerde displayName voor een gebruiker die hun UPN gewijzigd heeft/in quarantaine geplaatst. Bijvoorbeeld:
   
    a. **Gebruiker A** synchroniseert eerst met een **UPN = User@contoso.com** .
   
    b. **Gebruiker B** wordt geprobeerd om te worden gesynchroniseerd volgende met **UPN = User@contoso.com** .
   
    c. **Gebruiker B** UPN is gewijzigd in **User1234@contoso.onmicrosoft.com** en **User@contoso.com** wordt toegevoegd aan **DirSyncProvisioningErrors**.
   
    d. Het foutbericht voor **gebruiker B** moet geven aan dat **gebruiker A** al **User@contoso.com** zoals weergegeven in een UPN, maar het **van gebruiker B** eigen displayName.

**Rapport over mappensynchronisatie fout identiteit**:

De koppeling voor *stappen over het oplossen van dit probleem* is onjuist:  
    ![Actieve gebruikers](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/6.png "actieve gebruikers")  

Deze moet verwijzen naar [ https://aka.ms/duplicateattributeresiliency ](https://aka.ms/duplicateattributeresiliency).

## <a name="see-also"></a>Zie ook
* [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
* [Identificeren van de synchronisatiefouten directory in Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

