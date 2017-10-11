---
title: Tolerantie voor synchronisatie en dubbel kenmerk | Microsoft Docs
description: Nieuwe gedrag van het werken met objecten met UPN- of ProxyAddress conflicten tijdens de directorysynchronisatie via Azure AD Connect.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 537a92b7-7a84-4c89-88b0-9bce0eacd931
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.openlocfilehash: 7a8700e70f64851a0c5e5e8c6b31ec7a6884a96c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Tolerantie voor synchronisatie- en duplicatiekenmerken identificeren
Dubbele kenmerk tolerantie is een functie in Azure Active Directory, die wordt veroorzaakt door wrijving elimineren **UserPrincipalName** en **ProxyAddress** veroorzaakt een conflict bij het uitvoeren van een van de hulpprogramma's voor Microsoft synchronisatie.

Deze twee kenmerken in het algemeen moeten uniek zijn in alle **gebruiker**, **groep**, of **Contact** objecten in een bepaalde Azure Active Directory-tenant.

> [!NOTE]
> Alleen gebruikers kunnen de UPN's hebben.
> 
> 

De nieuwe functies die met deze functie kunt is in het gedeelte van de cloud van de synchronisatie-pijplijn en is daarom client agnostisch en relevant is voor een Microsoft-synchronisatie product met inbegrip van Azure AD Connect, DirSync en MIM-Connector. De algemene term 'sync-client' wordt gebruikt in dit document voor een van deze producten.

## <a name="current-behavior"></a>Huidige gedrag
Als er een poging tot het inrichten van een nieuw object met een UPN- of ProxyAddress-waarde die in strijd met deze beperking voor uniekheid, blokkeert Azure Active Directory dat object kunnen worden gemaakt. Op dezelfde manier als een object wordt bijgewerkt met een niet-unieke UPN of ProxyAddress, mislukt de update. De inrichting poging of de update opnieuw wordt gestart door de client synchronisatie bij elke cyclus exporteren en blijft mislukken totdat het conflict opgelost is. Een e-mailbericht fout rapport wordt gegenereerd na elke poging en een fout wordt vastgelegd door de synchronisatie-client.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Gedrag met dubbel kenmerk tolerantie
In plaats van volledig niet in te richten of bijwerken van een object met een dubbel kenmerk, quarantaine Azure Active Directory' ' het dubbele kenmerk dat de beperking voor uniekheid zou schenden. Als dit kenmerk vereist is voor het inrichten, zoals UserPrincipalName, wijst de service een tijdelijke aanduidingswaarde. De indeling van deze tijdelijke waarden is  
"***<OriginalPrefix>+ < 4DigitNumber > @<InitialTenantDomain>. onmicrosoft.com***'.  
Als het kenmerk niet vereist is, zoals een **ProxyAddress**, Azure Active Directory gewoon quarantaine plaatst het kenmerk conflict en wordt doorgegaan met het maken van het object of de update.

Bij het kenmerk in quarantaine plaatsen, wordt informatie over het conflict in de dezelfde fout rapport e-mail gebruikt in het oude gedrag verzonden. Deze informatie wordt alleen weergegeven in het foutenrapport één keer wanneer de quarantaine gebeurt, deze komt niet blijft echter in toekomstige e-mailberichten worden vastgelegd. Ook, omdat de uitvoer voor dit object is geslaagd, sync-client wordt niet geregistreerd voor een fout en probeert de maken niet opnieuw / update-bewerking op de volgende synchronisatiecycli.

Ter ondersteuning van dit gedrag is een nieuw kenmerk toegevoegd aan de gebruiker, groep en neem contact op met objectklassen:  
**DirSyncProvisioningErrors**

Dit is een kenmerk met meerdere waarden die wordt gebruikt voor het opslaan van de conflicterende kenmerken die de beperking voor uniekheid zou schenden moeten ze worden toegevoegd normaal. Een timer achtergrondtaak is ingeschakeld in Azure Active Directory, die elk uur op zoek naar dubbel kenmerk conflicten die zijn opgelost en de betreffende kenmerken automatisch verwijderd uit quarantaine wordt uitgevoerd.

### <a name="enabling-duplicate-attribute-resiliency"></a>Inschakelen van tolerantie voor dubbele kenmerk
Dubbele kenmerk tolerantie worden de nieuwe standaardgedrag over alle Azure Active Directory-tenants. Deze bevindt zich op standaard voor alle tenants die ingeschakeld synchronisatie voor het eerst 22 augustus 2016 of hoger. Tenants die ingeschakeld synchroniseren voordat deze datum wordt de functie is ingeschakeld in batches hebben. Deze implementatie begint September 2016 en een e-mailmelding verzonden naar elke tenant technische berichtgeving contact met de specifieke datum wanneer de functie wordt ingeschakeld.

> [!NOTE]
> Nadat de tolerantie voor dubbele kenmerk is ingeschakeld kan niet worden uitgeschakeld.

Als u wilt controleren of de functie is ingeschakeld voor uw tenant, kunt u doen door de meest recente versie van de Azure Active Directory PowerShell-module downloaden en uitvoeren:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

> [!NOTE]
> U kunt de cmdlet Set-MsolDirSyncFeature niet meer gebruiken proactief de tolerantie voor dubbele kenmerk-functie inschakelen voordat deze is ingeschakeld voor uw tenant. Als u de functie testen, moet u een nieuwe Azure Active Directory-tenant maken.

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Objecten met DirSyncProvisioningErrors identificeren
Er zijn momenteel twee methoden voor het identificeren van objecten die u deze fouten als gevolg van dubbele eigenschap conflicten, Azure Active Directory PowerShell en de Office 365-beheerportal hebt. Er zijn plannen om de aanvullende portal gebaseerde rapportage in de toekomst uit te breiden.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
Voor de PowerShell-cmdlets in dit onderwerp het volgende geldt:

* Alle van de volgende cmdlets zijn hoofdlettergevoelig.
* De **– ErrorCategory PropertyConflict** moet altijd worden opgenomen. Er zijn momenteel geen andere typen **ErrorCategory**, maar dit in de toekomst kan worden uitgebreid.

Eerst aan de slag door te voeren **Connect MsolService** en referenties voor een tenantbeheerder invoeren.

Vervolgens gebruikt u de volgende cmdlets en gebruiken om fouten op verschillende manieren weer te geven:

1. [Alle](#see-all)
2. [Door de eigenschapstype](#by-property-type)
3. [Met conflicterende waarde](#by-conflicting-value)
4. [Met behulp van een zoekopdracht in de tekenreeks](#using-a-string-search)
5. [Gesorteerd](#sorted)
6. [In een beperkt aantal of alle](#in-a-limited-quantity-or-all)

#### <a name="see-all"></a>Alles bekijken
Eenmaal zijn verbonden, een algemeen overzicht van het kenmerk inrichting uitvoeren fouten in de tenant:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Dit resulteert in een resultaat als volgt:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  

#### <a name="by-property-type"></a>Door de eigenschapstype
Toevoegen als fouten wilt bekijken door de eigenschapstype, de **- PropertyName** markeren met de **UserPrincipalName** of **ProxyAddresses** argument:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

of

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Met conflicterende waarde
Er fouten met betrekking tot een bepaalde eigenschap toevoegen in de **- PropertyValue** vlag (**- PropertyName** moet ook worden gebruikt bij het toevoegen van deze vlag):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`

#### <a name="using-a-string-search"></a>Met behulp van een zoekopdracht in de tekenreeks
Een zoekopdracht brede tekenreeks gebruik doen de **- zoekreeks** vlag. Dit kan worden gebruikt onafhankelijk van alle bovenstaande vlaggen, met uitzondering van **- ErrorCategory PropertyConflict**, die is vereist:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>In een beperkt aantal of alle
1. **MaxResults <Int>**  kan worden gebruikt om de query beperken tot een bepaald aantal waarden.
2. **Alle** kan worden gebruikt om te controleren of alle resultaten in het geval dat een groot aantal fouten bestaat worden opgehaald.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Office 365-beheerportal
U kunt directory-synchronisatiefouten weergeven in het Office 365-beheercentrum. Het rapport in de Office 365-beheerportal geeft alleen **gebruiker** objecten die u deze fouten hebt. Informatie over conflicten tussen wordt niet weergegeven **groepen** en **contactpersonen**.

![Actieve gebruikers](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "actieve gebruikers")

Zie voor instructies over het weergeven van directory-synchronisatiefouten in het Office 365-beheercentrum [identificeren van directory-synchronisatiefouten in Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).

### <a name="identity-synchronization-error-report"></a>Identiteit synchronisatie foutenrapport
Wanneer een object met een dubbel kenmerk conflict wordt verwerkt met dit nieuwe gedrag een melding is opgenomen in de standaard e-identiteit rapport dat wordt verzonden naar de melding technische contactpersoon voor de tenant. Er is echter een belangrijke wijziging in dit gedrag. In het verleden zou informatie over een dubbel kenmerk conflict worden opgenomen in elke volgende foutrapport totdat het conflict is opgelost. Met dit nieuwe gedrag wordt de foutmelding voor een bepaalde conflict alleen weergegeven eenmaal - op het moment dat het kenmerk conflicterende in quarantaine is geplaatst.

Hier volgt een voorbeeld van hoe de e-mailmelding voor een conflict ProxyAddress uitziet:  
    ![Actieve gebruikers](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "actieve gebruikers")  

## <a name="resolving-conflicts"></a>Het oplossen van conflicten
Strategie en resolutie tactieken voor deze fouten oplossen moet niet verschillen van de manier waarop dubbel kenmerk fouten in het verleden zijn verwerkt. Het enige verschil is dat de taak timer via de tenant aan de service-kant duplicaten naar het desbetreffende kenmerk automatisch toevoegen aan het juiste object zodra het conflict opgelost is.

Het volgende artikel bevat een overzicht van de verschillende strategieën voor het oplossen van problemen en resolutie: [dubbele of ongeldige kenmerken te voorkomen dat directory-synchronisatie in Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Bekende problemen
Geen van deze bekende problemen zorgt ervoor dat gegevens verloren gaan of de service nadelig beïnvloeden. Sommige hiervan zijn esthetische, anderen standaard veroorzaken '*vooraf tolerantie*' dubbel kenmerk fouten gegenereerd in plaats van in quarantaine plaatsen van het conflict-kenmerk en een andere zorgt ervoor dat bepaalde fouten vereist extra handmatig bijwerken.

**Core-gedrag:**

1. Objecten van het specifieke kenmerk configuraties blijven ontvangen fouten exporteren in plaats van de dubbele kenmerken die in quarantaine wordt geplaatst.  
   Bijvoorbeeld:
   
    a. Nieuwe gebruiker is gemaakt in AD met de UPN-  **Joe@contoso.com**  en ProxyAddress**smtp:Joe@contoso.com**
   
    b. De eigenschappen van dit object in conflict met een bestaande groep, waarbij ProxyAddress is  **SMTP:Joe@contoso.com** .
   
    c. Bij het exporteren, een **ProxyAddress conflict** fout wordt gegenereerd in plaats van de conflict kenmerken in quarantaine geplaatst. De bewerking wordt opnieuw geprobeerd bij elke cyclus van de volgende synchronisatie, zoals zijn zou voordat de tolerantiefunctie is ingeschakeld.
2. Als twee groepen worden gemaakt van lokale met hetzelfde SMTP-adres, een mislukt om in te richten op de eerste poging met een standaard duplicaat **ProxyAddress** fout. Dubbele waarden in goed quarantaine bij de volgende synchronisatiecyclus.

**Office-Portal rapport**:

1. Het gedetailleerde foutbericht voor twee objecten in een conflict UPN-set is hetzelfde. Dit betekent dat ze hebben beide de UPN gewijzigd / in quarantaine geplaatst wanneer in feite alleen een van deze had geen gegevens die zijn gewijzigd.
2. Het gedetailleerde foutbericht voor een conflict UPN toont de verkeerde weergavenaam voor een gebruiker die de UPN gewijzigd/in quarantaine heeft gehad. Bijvoorbeeld:
   
    a. **Gebruiker A** gesynchroniseerd eerst met **UPN = User@contoso.com** .
   
    b. **Gebruiker B** wordt geprobeerd om te worden gesynchroniseerd volgende met **UPN = User@contoso.com** .
   
    c. **Gebruiker B** UPN wordt gewijzigd naar  **User1234@contoso.onmicrosoft.com**  en  **User@contoso.com**  wordt toegevoegd aan **DirSyncProvisioningErrors**.
   
    d. Het foutbericht voor **gebruiker B** zou moeten aangeven die **gebruiker A** al  **User@contoso.com**  zoals u ziet een UPN, maar het **van gebruiker B** eigen weergavenaam.

**Identiteit synchronisatie foutenrapport**:

De koppeling voor *stappen over het oplossen van dit probleem* is onjuist:  
    ![Actieve gebruikers](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "actieve gebruikers")  

Het moet verwijzen naar [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).

## <a name="see-also"></a>Zie ook
* [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
* [Identificeren van directory-synchronisatiefouten in Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

