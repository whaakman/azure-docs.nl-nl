---
title: 'Azure AD Connect: Het oplossen van problemen tijdens de synchronisatie | Microsoft Docs'
description: Legt uit hoe u fouten aangetroffen tijdens de synchronisatie met Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 2209d5ce-0a64-447b-be3a-6f06d47995f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: aaa374d5a11ef5b5860f83a87386ff981319189f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshooting-errors-during-synchronization"></a>Het oplossen van problemen tijdens de synchronisatie
Er kunnen optreden wanneer identiteitsgegevens van Windows Server Active Directory (AD DS) is gesynchroniseerd met Azure Active Directory (Azure AD). Dit artikel bevat een overzicht van de verschillende soorten synchronisatiefouten enkele van de mogelijke scenario's die ervoor zorgen dat deze fouten en mogelijke manieren de fouten te herstellen. Dit artikel bevat de algemene fouttypen en kan geen betrekking op alle mogelijke fouten.

 In dit artikel wordt ervan uitgegaan dat de lezer bekend bent met de onderliggende [ontwerpen concepten van Azure AD en Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Met de meest recente versie van Azure AD Connect \(augustus 2016 of hoger\), een rapport van synchronisatie van fouten is beschikbaar in de [Azure Portal](https://aka.ms/aadconnecthealth) als onderdeel van Azure AD Connect Health voor synchroniseren.

Vanaf 1 September 2016 [Azure Active Directory dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) functie wordt standaard ingeschakeld voor alle de *nieuwe* Azure Active Directory-Tenants. Deze functie wordt automatisch ingeschakeld voor bestaande tenants in de komende maanden.

Azure AD Connect voert 3 soorten bewerkingen van de mappen synchroon blijven: Import, synchronisatie en exporteren. Fouten kunnen worden uitgevoerd in alle bewerkingen. In dit artikel richt zich voornamelijk op fouten tijdens het exporteren naar Azure AD.

## <a name="errors-during-export-to-azure-ad"></a>Fouten tijdens de Export naar Azure AD
Volgende sectie worden verschillende soorten synchronisatiefouten die kunnen optreden tijdens de exportbewerking naar Azure AD met behulp van de Azure AD-connector beschreven. Deze connector kan worden geïdentificeerd door de indeling wordt 'contoso. *onmicrosoft.com*'.
Fouten tijdens de Export naar Azure AD aangeeft dat de bewerking \(toevoegen, bijwerken en verwijderen enzovoort\) geprobeerd met Azure AD Connect \(synchronisatie-Engine\) op Azure Active Directory is mislukt.

![Overzicht van fouten exporteren](./media/active-directory-aadconnect-troubleshoot-sync-errors/Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Gegevens komen niet overeen fouten
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Beschrijving
* Wanneer Azure AD Connect \(synchronisatie-engine\) Hiermee geeft u Azure Active Directory toevoegen of bijwerken van objecten, Azure AD overeenkomt met het binnenkomende object met behulp van de **sourceAnchor** kenmerk de **onveranderbare id genoemd**  kenmerk van objecten in Azure AD. Deze overeenkomst, heet een **harde overeen met**.
* Wanneer Azure AD **vindt geen** elk object dat overeenkomt met de **onveranderbare id genoemd** kenmerk met de **sourceAnchor** kenmerk van het binnenkomende object, voordat u een nieuwe inricht object valt terug voor het gebruik van de kenmerken ProxyAddresses en UserPrincipalName te vinden. Deze overeenkomst, heet een **zachte overeen met**. Het zachte overeenkomen met is ontworpen om overeen met objecten die al aanwezig in Azure AD (die afkomstig zijn in Azure AD) met de nieuwe objecten worden toegevoegd/bijgewerkt tijdens de synchronisatie die dezelfde entiteit (gebruikers, groepen) on-premises vertegenwoordigen.
* **InvalidSoftMatch** fout treedt op wanneer een overeenkomst met de vaste geen overeenkomende objecten vindt **en** zachte overeen met een overeenkomende object vindt, maar dat object heeft een andere waarde van *onveranderbare id genoemd* dan het binnenkomende object *SourceAnchor*, opmaken of het overeenkomende object is gesynchroniseerd met een ander object van on premises Active Directory.

Met andere woorden, in volgorde voor het zachte overeenkomt met het werk worden zachte komt overeen met het object mag geen geen waarde voor de *onveranderbare id genoemd*. Als een met object *onveranderbare id genoemd* set met een waarde is niet mogelijk de harde-match, maar die voldoet aan de soft-match-criteria, de bewerking resulteert in een InvalidSoftMatch synchronisatie-fout.

Azure Active Directory-schema is niet toegestaan voor twee of meer objecten hebben dezelfde waarde voor de volgende kenmerken. \(Dit is geen uitputtende lijst.\)

* ProxyAddresses
* UserPrincipalName
* onPremisesSecurityIdentifier
* ObjectId

> [!NOTE]
> [Azure AD-kenmerk dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) functie wordt ook worden geïnstalleerd als het standaardgedrag van Azure Active Directory.  Zo beperkt u het aantal synchronisatiefouten gezien door Azure AD Connect (evenals andere clients sync) door Azure AD meer flexibiliteit in de wijze die gedupliceerde ProxyAddresses UserPrincipalName kenmerken en aanwezig is in op de lokale AD-omgevingen worden verwerkt. Deze functie biedt de duplicatie fouten niet worden hersteld. De gegevens moeten dus nog steeds worden hersteld. Maar kunt inrichten van nieuwe objecten die anders is geblokkeerd vanwege dubbele waarden wordt ingericht in Azure AD. Zo ook beperkt u het aantal synchronisatiefouten geretourneerd naar de synchronisatie-client.
> Als deze functie is ingeschakeld voor uw Tenant, ziet u de synchronisatie van InvalidSoftMatch fouten weergegeven tijdens het inrichten van nieuwe objecten.
>
>

#### <a name="example-scenarios-for-invalidsoftmatch"></a>Voorbeeldscenario's voor InvalidSoftMatch
1. Twee of meer objecten met dezelfde waarde van kenmerk ProxyAddresses bestaat in on-premises Active Directory. Slechts één wordt ophalen ingericht in Azure AD.
2. Twee of meer objecten met dezelfde waarde voor userPrincipalName bestaat in on-premises Active Directory. Slechts één wordt ophalen ingericht in Azure AD.
3. Een object is toegevoegd in de on-premises Active Directory met dezelfde waarde voor kenmerk ProxyAddresses als die van een bestaand object in Azure Active Directory. Het object toegevoegd on-premises is niet ophalen van ingericht in Azure Active Directory.
4. Een object is toegevoegd in on-premises Active Directory met dezelfde waarde voor kenmerk userPrincipalName als die van een account in Azure Active Directory. Het object is niet ophalen van ingericht in Azure Active Directory.
5. Een gesynchroniseerde-account is verplaatst in Forest A naar Forest B. Azure AD Connect (sync engine) kenmerk ObjectGUID werd gebruikt voor het berekenen van de SourceAnchor. Na het verplaatsen forest is de waarde van de SourceAnchor anders. Het nieuwe object (van Forest B) kan niet worden gesynchroniseerd met het bestaande object in Azure AD.
6. Een gesynchroniseerd object hebt per ongeluk verwijderd uit on-premises Active Directory en een nieuw object is gemaakt in Active Directory voor dezelfde entiteit (zoals gebruiker) zonder te verwijderen van de account in Azure Active Directory. Het nieuwe account kan niet synchroniseren met de bestaande Azure AD-object.
7. Azure AD Connect is verwijderd en opnieuw geïnstalleerd. Tijdens het opnieuw installeren, is een ander kenmerk gekozen als de SourceAnchor. Alle objecten die eerder was gesynchroniseerd gestopt synchroniseren met InvalidSoftMatch fout.

#### <a name="example-case"></a>Voorbeeld van de case:
1. **Bob Smith** is een gesynchroniseerde gebruiker in Azure Active Directory vanuit de on-premises Active Directory van *contoso.com*
2. Bob Smith **UserPrincipalName** is ingesteld als  **bobs@contoso.com** .
3. **' abcdefghijklmnopqrstuv == "** is de **SourceAnchor** berekend door Azure AD Connect met Bob Smith **objectGUID** van on-premises Active Directory dit is de  **onveranderbare id genoemd** voor Bob Smith bij Azure Active Directory.
4. Bob heeft ook de volgende waarden voor de **proxyAddresses** kenmerk:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
5. Een nieuwe gebruiker **Bob Taylor**, wordt toegevoegd aan de on-premises Active Directory.
6. Bob Taylor van **UserPrincipalName** is ingesteld als  **bobt@contoso.com** .
7. **' abcdefghijkl0123456789 == ""** is de **sourceAnchor** berekend door Azure AD Connect met Bob Taylor van **objectGUID** premises uit op Active Directory. Bob Taylor van object is niet gesynchroniseerd met Azure Active Directory.
8. Bob Taylor heeft de volgende waarden voor het kenmerk proxyAddresses
   * smtp:bobt@contoso.com
   * smtp:bob.taylor@contoso.com
   * **smtp:bob@contoso.com**
9. Azure AD Connect wordt herkend door de toevoeging van Bob Taylor in on-premises Active Directory en vraagt u Azure AD de dezelfde wijzigingen aanbrengen tijdens de synchronisatie.
10. Azure AD wordt eerst harde overeen uitvoeren. Dat wil zeggen, zoekt als er een object met de onveranderbare id genoemd gelijk is aan ' abcdefghijkl0123456789 == ". Vaste overeen zal mislukken als er geen andere objecten in Azure AD die onveranderbare id genoemd hebben.
11. Azure AD probeert vervolgens te zacht-match Bob Taylor. Dat wil zeggen, zoekt als er een object met proxyAddresses gelijk aan de drie waarden is, met inbegrip vansmtp:bob@contoso.com
12. Azure AD vindt van Bob Smith-object om te voldoen aan de soft-match-criteria. Maar dit object heeft de waarde van onveranderbare id genoemd = "abcdefghijklmnopqrstuv ==". wat aangeeft dat dit object is gesynchroniseerd vanuit een ander object van on-premises Active Directory. Dus Azure AD kan niet soft-match deze objecten en resulteert in een **InvalidSoftMatch** synchronisatiefout.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Het InvalidSoftMatch fout oplossen
De meest voorkomende reden voor de fout InvalidSoftMatch is twee objecten met verschillende SourceAnchor \(onveranderbare id genoemd\) hebben dezelfde waarde voor de ProxyAddresses en/of UserPrincipalName kenmerken, die worden gebruikt tijdens de soft-match verwerken op Azure AD. Voor het vaststellen van de overeenkomst met de ongeldige zachte

1. Identificeer de gedupliceerde proxyAddresses, userPrincipalName of andere kenmerkwaarde die de fout wordt veroorzaakt. Ook bepalen welke twee \(of meer\) objecten betrokken zijn bij het conflict. Het rapport is gegenereerd door [Azure AD Connect Health voor synchroniseren](https://aka.ms/aadchsyncerrors) kunt u de twee objecten identificeren.
2. Welk object moet doorgaan naar een dubbele waarde hebben en welk object beter niet identificeren.
3. Verwijder de dubbele waarde uit het object dat de waarde niet hebben. Houd er rekening mee dat moet u de wijziging in de map waar het object afkomstig is uit. In sommige gevallen moet u mogelijk een van de objecten in conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de on-premises AD, kunt u Azure AD Connect is de wijziging te synchroniseren.

Houd er rekening mee dat synchronisatie foutenrapport in Azure AD Connect Health voor synchroniseren elke 30 minuten bijgewerkt wordt en de fouten van de laatste synchronisatiepoging bevat.

> [!NOTE]
> Onveranderbare id genoemd, moet per definitie niet wijzigen in de levensduur van het object. Als Azure AD Connect is niet geconfigureerd met enkele van de scenario's in gedachten in de bovenstaande lijst, kan het uiteindelijke in een situatie waarin Azure AD Connect wordt berekend met een andere waarde op voor de SourceAnchor voor de AD-object met dezelfde entiteit (dezelfde gebruiker of groep / Neem contact op met enzovoort) die een bestaand Azure AD-Object, dat u wilt doorgaan met heeft.
>
>

#### <a name="related-articles"></a>Gerelateerde artikelen
* [Dubbel of ongeldige kenmerken te voorkomen dat directory-synchronisatie in Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Beschrijving
Wanneer Azure AD probeert te zacht overeen met twee objecten, is het mogelijk dat twee objecten van verschillende "objecttype' (zoals een gebruiker, groep-, enz. Neem contact op met) hebben dezelfde waarden voor de kenmerken die wordt gebruikt voor het uitvoeren van de zachte overeenstemmen. Als deze kenmerken worden gedupliceerd is niet toegestaan in Azure AD, kan de bewerking resulteert in synchronisatiefout 'ObjectTypeMismatch'.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Voorbeeldscenario's voor ObjectTypeMismatch fout
* Een e-mailtoegang beveiligingsgroep wordt gemaakt in Office 365. Beheerder voegt een nieuwe gebruiker of contactpersoon in on-premises AD (die niet wordt gesynchroniseerd naar Azure AD nog) met dezelfde waarde voor het kenmerk ProxyAddresses als die van de Office 365-groep.

#### <a name="example-case"></a>Voorbeeld van de aanvraag
1. De beheerder maakt een nieuwe beveiligingsgroep voor e-mailtoegang in Office 365 voor de btw-afdeling en biedt een e-mailadres als tax@contoso.com. Hiermee wijst u het kenmerk ProxyAddresses voor deze groep met de waarde van toe**smtp:tax@contoso.com**
2. Een nieuwe gebruiker koppelt Contoso.com en een account is gemaakt voor de gebruiker on-premises met de proxyAddress als**smtp:tax@contoso.com**
3. Als Azure AD Connect het nieuwe gebruikersaccount synchroniseert zal, krijgt deze de fout 'ObjectTypeMismatch'.

#### <a name="how-to-fix-objecttypemismatch-error"></a>Het ObjectTypeMismatch fout oplossen
De meest voorkomende reden voor de fout ObjectTypeMismatch is twee objecten van het andere type (gebruiker, groep-, enz. Neem contact op met) hebben dezelfde waarde voor het kenmerk ProxyAddresses. Voor het vaststellen van de ObjectTypeMismatch:

1. Identificeer de gedupliceerde proxyAddresses (of een ander kenmerk)-waarde die de fout wordt veroorzaakt. Ook bepalen welke twee \(of meer\) objecten betrokken zijn bij het conflict. Het rapport is gegenereerd door [Azure AD Connect Health voor synchroniseren](https://aka.ms/aadchsyncerrors) kunt u de twee objecten identificeren.
2. Welk object moet doorgaan naar een dubbele waarde hebben en welk object beter niet identificeren.
3. Verwijder de dubbele waarde uit het object dat de waarde niet hebben. Houd er rekening mee dat moet u de wijziging in de map waar het object afkomstig is uit. In sommige gevallen moet u mogelijk een van de objecten in conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de on-premises AD, kunt u Azure AD Connect is de wijziging te synchroniseren. Synchronisatie foutenrapport in Azure AD Connect Health voor synchroniseren wordt elke 30 minuten bijgewerkt en de fouten van de laatste synchronisatiepoging bevat.

## <a name="duplicate-attributes"></a>Dubbele kenmerken
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Beschrijving
Azure Active Directory-schema is niet toegestaan voor twee of meer objecten hebben dezelfde waarde voor de volgende kenmerken. Dat elk object in Azure AD moet een unieke waarde van deze kenmerken hebben op een opgegeven exemplaar is.

* ProxyAddresses
* UserPrincipalName

Als Azure AD Connect probeert om een nieuw object toevoegen of bijwerken van een bestaand object met een waarde voor de bovenstaande kenmerken die al is toegewezen aan een ander object in Azure Active Directory, wordt de bewerking resulteert in de synchronisatiefout 'AttributeValueMustBeUnique'.

#### <a name="possible-scenarios"></a>Mogelijke scenario's:
1. Dubbele waarde is toegewezen aan een object al gesynchroniseerde, wat een conflict met een ander gesynchroniseerd object veroorzaakt.

#### <a name="example-case"></a>Voorbeeld van de case:
1. **Bob Smith** is een gesynchroniseerde gebruiker in Azure Active Directory vanuit de on-premises Active Directory contoso.com
2. Bob Smith **UserPrincipalName** on-premises is ingesteld als  **bobs@contoso.com** .
3. Bob heeft ook de volgende waarden voor de **proxyAddresses** kenmerk:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
4. Een nieuwe gebruiker **Bob Taylor**, wordt toegevoegd aan de on-premises Active Directory.
5. Bob Taylor van **UserPrincipalName** is ingesteld als  **bobt@contoso.com** .
6. **Bob Taylor** heeft de volgende waarden voor de **ProxyAddresses** kenmerk i. smtp:bobt@contoso.com ii. smtp:bob.taylor@contoso.com
7. Bob Taylor van object wordt gesynchroniseerd met Azure AD is.
8. Beheerder besloten om bij te werken van Bob Taylor **ProxyAddresses** kenmerk met de volgende waarde: ik. **smtp:bob@contoso.com**
9. Azure AD probeert te Bob Taylor van object bijwerken in Azure AD met de bovenstaande waarde, maar die bewerking mislukken als dat ProxyAddresses waarde al aan Bob Smith toegewezen is, wat resulteert in 'AttributeValueMustBeUnique'-fout.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Het AttributeValueMustBeUnique fout oplossen
De meest voorkomende reden voor de fout AttributeValueMustBeUnique is twee objecten met verschillende SourceAnchor \(onveranderbare id genoemd\) hebben dezelfde waarde voor de kenmerken ProxyAddresses en/of UserPrincipalName. Om te kunnen AttributeValueMustBeUnique fout oplossen

1. Identificeer de gedupliceerde proxyAddresses, userPrincipalName of andere kenmerkwaarde die de fout wordt veroorzaakt. Ook bepalen welke twee \(of meer\) objecten betrokken zijn bij het conflict. Het rapport is gegenereerd door [Azure AD Connect Health voor synchroniseren](https://aka.ms/aadchsyncerrors) kunt u de twee objecten identificeren.
2. Welk object moet doorgaan naar een dubbele waarde hebben en welk object beter niet identificeren.
3. Verwijder de dubbele waarde uit het object dat de waarde niet hebben. Houd er rekening mee dat moet u de wijziging in de map waar het object afkomstig is uit. In sommige gevallen moet u mogelijk een van de objecten in conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de on-premises AD, kunt u Azure AD Connect synchroniseren van de wijziging voor de fout naar ' vast ' ophalen.

#### <a name="related-articles"></a>Gerelateerde artikelen
-[Dubbel of ongeldige kenmerken te voorkomen dat directory-synchronisatie in Office 365](https://support.microsoft.com/en-us/kb/2647098)

## <a name="data-validation-failures"></a>Mislukte gegevensvalidatie
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Beschrijving
Azure Active Directory wordt afgedwongen voor verschillende beperkingen op de gegevens zelf alvorens toe te staan dat de gegevens worden geschreven naar de map. Dit is om ervoor te zorgen dat eindgebruikers de best mogelijke ervaring tijdens het gebruik van de toepassingen die afhankelijk van deze gegevens zijn ophalen.

#### <a name="scenarios"></a>Scenario's
a. De waarde van het kenmerk UserPrincipalName heeft ongeldig/niet-ondersteunde tekens.
b. Het kenmerk UserPrincipalName voldoet niet aan de vereiste indeling.

#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Het IdentityDataValidationFailed fout oplossen
a. Zorg ervoor dat het kenmerk userPrincipalName tekens en de vereiste indeling heeft ondersteund.

#### <a name="related-articles"></a>Gerelateerde artikelen
* [Voorbereiden voor het inrichten van gebruikers door directorysynchronisatie op Office 365](https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)

### <a name="federateddomainchangeerror"></a>FederatedDomainChangeError
#### <a name="description"></a>Beschrijving
Dit is een specifieke aanvraag die resulteert in een **'FederatedDomainChangeError'** synchronisatiefout wanneer het achtervoegsel van de UserPrincipalName van een gebruiker uit een federatieve domein naar een ander federatieve domein wordt gewijzigd.

#### <a name="scenarios"></a>Scenario's
Het achtervoegsel UserPrincipalName is voor een gesynchroniseerde gebruiker gewijzigd van één federatieve domein naar een ander federatieve domein on-premises. Bijvoorbeeld: *UserPrincipalName = bob@contoso.com*  is gewijzigd in *UserPrincipalName = bob@fabrikam.com* .

#### <a name="example"></a>Voorbeeld
1. Bob Smith, een account voor Contoso.com is, wordt toegevoegd als een nieuwe gebruiker in Active Directory met de UserPrincipalNamebob@contoso.com
2. Bob wordt verplaatst naar een andere afdeling contoso.com Fabrikam.com genoemd en zijn UserPrincipalName is gewijzigd inbob@fabrikam.com
3. Contoso.com en fabrikam.com domeinen zijn gefedereerde met Azure Active Directory-domeinen.
4. Berend userPrincipalName niet wordt bijgewerkt en resulteert in een synchronisatiefout 'FederatedDomainChangeError'.

#### <a name="how-to-fix"></a>Op te lossen
Als een gebruiker UserPrincipalName achtervoegsel is bijgewerkt van bob @**contoso.com** naar bob @**fabrikam.com**, waarbij beide **contoso.com** en **fabrikam.com** zijn **domeinen federatieve**, klikt u vervolgens als volgt te werk om de synchronisatiefout te verhelpen

1. Bijwerken van de gebruiker UserPrincipalName in Azure AD van bob@contoso.com naar bob@contoso.onmicrosoft.com. Met de Azure AD PowerShell-Module kunt u de volgende PowerShell-opdracht:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`
2. Toestaan dat de volgende synchronisatiecyclus om synchronisatie. Deze tijdsynchronisatie kan worden geverifieerd en de UserPrincipalName van Bob om te worden bijgewerkt bob@fabrikam.com zoals verwacht.

#### <a name="related-articles"></a>Gerelateerde artikelen
* [Wijzigingen zijn niet gesynchroniseerd door de Azure Active Directory-synchronisatie nadat u de UPN van een gebruikersaccount voor het gebruik van een ander federatieve domein wijzigen](https://support.microsoft.com/en-us/help/2669550/changes-aren-t-synced-by-the-azure-active-directory-sync-tool-after-you-change-the-upn-of-a-user-account-to-use-a-different-federated-domain)

## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Beschrijving
Wanneer een kenmerk overschrijdt de maximale toegestane grootte, de maximale lengte of de limiet voor het aantal ingesteld met Azure Active Directory-schema, de synchronisatiebewerking resulteert in het **LargeObject** of **ExceededAllowedLength**synchronisatiefout. Deze fout treedt doorgaans op voor de volgende kenmerken

* userCertificate
* userSMIMECertificate
* thumbnailPhoto
* proxyAddresses

### <a name="possible-scenarios"></a>Mogelijke scenario 's
1. Berend userCertificate kenmerk is te veel certificaten die zijn toegewezen aan Bob opslaan. Deze kunnen oudere, verlopen certificaten bevatten. De vaste limiet is 15 certificaten. Raadpleeg voor meer informatie over het afhandelen van fouten met het kenmerk userCertificate LargeObject artikel [LargeObject afhandeling van fouten als gevolg van userCertificate kenmerk](active-directory-aadconnectsync-largeobjecterror-usercertificate.md).
2. Berend userSMIMECertificate kenmerk is te veel certificaten die zijn toegewezen aan Bob opslaan. Deze kunnen oudere, verlopen certificaten bevatten. De vaste limiet is 15 certificaten.
3. Berend thumbnailPhoto instellen in Active Directory is te groot om in Azure AD worden gesynchroniseerd.
4. Tijdens het vullen met automatische van het kenmerk ProxyAddresses in Active Directory heeft een object te veel ProxyAddresses toegewezen.

### <a name="how-to-fix"></a>Op te lossen
1. Zorg ervoor dat het kenmerk dat de fout veroorzaakt binnen de toegestane limiet.

## <a name="related-links"></a>Verwante koppelingen
* [Het vinden van Active Directory-objecten in Active Directory-beheercentrum](https://technet.microsoft.com/library/dd560661.aspx)
* [Azure Active Directory een query voor een object met behulp van Azure Active Directory PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
