---
title: 'Azure AD Connect: Synchronisatiefouten oplossen | Microsoft Docs'
description: Wordt uitgelegd hoe u fouten aangetroffen tijdens de synchronisatie met Azure AD Connect oplossen.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 2209d5ce-0a64-447b-be3a-6f06d47995f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: c94ecc223c4e2c0533c23e58823bb203064ceef6
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50250447"
---
# <a name="troubleshooting-errors-during-synchronization"></a>Synchronisatiefouten oplossen
Fouten kunnen zich voordoen wanneer identiteitsgegevens worden gesynchroniseerd vanuit Windows Server Active Directory (AD DS) naar Azure Active Directory (Azure AD). Dit artikel bevat een overzicht van verschillende typen synchronisatiefouten enkele van de mogelijke scenario's die ertoe leiden dat deze fouten en mogelijke manieren de fouten te herstellen. In dit artikel bevat de algemene fouttypen en kan geen betrekking op alle mogelijke fouten.

 In dit artikel wordt ervan uitgegaan dat de lezer bekend bent met de onderliggende is [ontwerpconcepten van Azure AD en Azure AD Connect](plan-connect-design-concepts.md).

Met de meest recente versie van Azure AD Connect \(augustus 2016 of hoger\), een rapport over synchronisatiefouten is beschikbaar in de [Azure-portal](https://aka.ms/aadconnecthealth) als onderdeel van Azure AD Connect Health voor synchroniseren.

Vanaf 1 September 2016 [Azure Active Directory dubbel kenmerk tolerantie](how-to-connect-syncservice-duplicate-attribute-resiliency.md) functie wordt standaard ingeschakeld voor alle de *nieuwe* Azure Active Directory-Tenants. Deze functie wordt automatisch ingeschakeld voor bestaande tenants in de komende maanden.

Azure AD Connect drie typen bewerkingen uitvoert vanaf de mappen die zijn gesynchroniseerd blijven: Import, synchronisatie en exporteren. Fouten plaatsvinden in alle bewerkingen. In dit artikel richt zich voornamelijk op fouten tijdens het exporteren naar Azure AD.

## <a name="errors-during-export-to-azure-ad"></a>Fouten tijdens het exporteren naar Azure AD
Volgende sectie wordt beschreven verschillende soorten synchronisatiefouten die zich kunnen voordoen tijdens de exportbewerking naar Azure AD met behulp van de Azure AD-connector. Deze connector kan worden geïdentificeerd door de indeling wordt 'contoso. *onmicrosoft.com*'.
Fouten tijdens het exporteren naar Azure AD geven aan dat de bewerking \(toevoegen, bijwerken en verwijderen enz.\) door Azure AD Connect geprobeerd \(synchronisatie-Engine\) op Azure Active Directory is mislukt.

![Overzicht van de fouten exporteren](./media/tshoot-connect-sync-errors/Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Fouten niet-overeenkomende gegevens
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Beschrijving
* Wanneer Azure AD Connect \(synchronisatie-engine\) geeft Azure Active Directory wilt toevoegen of bijwerken van objecten, Azure AD overeenkomt met de binnenkomende object via de **sourceAnchor** kenmerk aan de **immutableId**  kenmerk van objecten in Azure AD. Deze overeenkomst is met de naam een **harde overeenkomen met**.
* Wanneer Azure AD **geen vindt** elk object dat overeenkomt met de **immutableId** kenmerk met de **sourceAnchor** kenmerk van het binnenkomende object, voordat u een nieuwe inricht een object, wordt gebruikgemaakt van voor het gebruik van de kenmerken ProxyAddresses en UserPrincipalName te vinden. Deze overeenkomst is met de naam een **zachte overeenkomen met**. De zachte overeenkomen met is ontworpen om objecten die al aanwezig in Azure AD (die afkomstig zijn in Azure AD) met de nieuwe objecten worden toegevoegd/bijgewerkt tijdens de synchronisatie die staan voor de dezelfde entiteit (gebruikers, groepen) on-premises te koppelen.
* **InvalidSoftMatch** fout treedt op wanneer een overeenkomst met de vaste geen overeenkomende objecten vindt **en** zachte match vindt u een overeenkomend object maar dat object is een andere waarde van *immutableId* dan de binnenkomende object *SourceAnchor*, voorstellen dat het overeenkomende object is gesynchroniseerd met een ander object vanuit on-premises Active Directory.

Met andere woorden, in volgorde voor de zachte match om te werken, het object dat moet worden voorlopig vergeleken met de mogen geen waarde voor de *immutableId*. Als een met object *immutableId* instellen met een waarde is niet mogelijk de harde-overeenkomst, maar die voldoen aan de criteria zachte match, de bewerking resulteert in een synchronisatiefout InvalidSoftMatch.

Azure Active Directory-schema is niet toegestaan voor twee of meer objecten hebben dezelfde waarde van de volgende kenmerken. \(Dit is geen uitputtende lijst.\)

* proxyAddresses
* UserPrincipalName
* onPremisesSecurityIdentifier
* ObjectId

> [!NOTE]
> [Azure AD-kenmerk dubbel kenmerk tolerantie](how-to-connect-syncservice-duplicate-attribute-resiliency.md) functie wordt ook als het standaardgedrag van Azure Active Directory wordt uitgerold.  Zo beperkt u het aantal synchronisatiefouten gezien door Azure AD Connect (evenals andere clients sync) door Azure AD beter bestand is tegen in de manier waarop die dubbele ProxyAddresses en UserPrincipalName kenmerken aanwezig zijn in on-premises AD-omgevingen worden verwerkt. Deze functie de duplicatie fouten niet wordt opgelost. De gegevens moeten dus nog steeds worden hersteld. Maar het inrichten van nieuwe objecten die anders zijn geblokkeerd vanwege dubbele waarden wordt ingericht in Azure AD. Zo beperkt u ook het aantal synchronisatiefouten geretourneerd naar de synchronisatie-client.
> Als deze functie is ingeschakeld voor uw Tenant, ziet u niet de InvalidSoftMatch synchronisatie-fouten weergegeven tijdens het inrichten van nieuwe objecten.
>
>

#### <a name="example-scenarios-for-invalidsoftmatch"></a>Voorbeeldscenario's voor InvalidSoftMatch
1. Er bestaan twee of meer objecten met dezelfde waarde voor het kenmerk ProxyAddresses in on-premises Active Directory. Slechts één wordt ophalen ingericht in Azure AD.
2. Twee of meer objecten met dezelfde waarde voor het kenmerk userPrincipalName bestaat in on-premises Active Directory. Slechts één wordt ophalen ingericht in Azure AD.
3. Een object is toegevoegd aan de on-premises Active Directory met dezelfde waarde voor kenmerk ProxyAddresses als die van een bestaand object in Azure Active Directory. Het object toegevoegd on-premises wordt niet ophalen van ingericht in Azure Active Directory.
4. Een object is toegevoegd in on-premises Active Directory met dezelfde waarde voor userPrincipalName-kenmerk als die van een account in Azure Active Directory. Het object is niet ophalen van ingericht in Azure Active Directory.
5. Een gesynchroniseerde-account is verplaatst in Forest A in Forest B. Azure AD Connect (sync engine genoemd) kenmerk ObjectGUID werd gebruikt voor het berekenen van de SourceAnchor. De waarde van het SourceAnchor verschilt na de verplaatsing forest. Het nieuwe object (in Forest B) kan niet worden gesynchroniseerd met het bestaande object in Azure AD.
6. Een gesynchroniseerde object hebt u per ongeluk verwijderd uit on-premises Active Directory en een nieuw object is gemaakt in Active Directory voor dezelfde entiteit (bijvoorbeeld gebruiker) zonder te verwijderen van het account in Azure Active Directory. Het nieuwe account kan niet worden gesynchroniseerd met de bestaande Azure AD-object.
7. Azure AD Connect is verwijderd en opnieuw geïnstalleerd. Tijdens de installatie is een ander kenmerk gekozen als het SourceAnchor. Alle objecten die eerder was gesynchroniseerd gestopt synchroniseren met InvalidSoftMatch fout.

#### <a name="example-case"></a>Voorbeeld van de aanvraag:
1. **Bob Smith** is een gesynchroniseerde gebruiker in Azure Active Directory vanuit on-premises Active Directory van *contoso.com*
2. Bob Smith **UserPrincipalName** is ingesteld als **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** is de **SourceAnchor** berekend door Azure AD Connect met behulp van Bob Smith **objectGUID** van on-premises Active Directory, dit is de  **immutableId** voor Bob Smith in Azure Active Directory.
4. Bob heeft ook de volgende waarden voor de **proxyAddresses** kenmerk:
   * SMTP: bobs@contoso.com
   * SMTP: bob.smith@contoso.com
   * **SMTP: bob@contoso.com**
5. Een nieuwe gebruiker **Bob Taylor**, wordt toegevoegd aan de on-premises Active Directory.
6. Bob Taylor van **UserPrincipalName** is ingesteld als **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** is de **sourceAnchor** berekend door Azure AD Connect met behulp van Bob Taylor **objectGUID** uit op de lokale Active Directory. Bob Taylor van object is niet gesynchroniseerd met Azure Active Directory nog.
8. Bob Taylor heeft de volgende waarden voor het kenmerk proxyAddresses
   * SMTP: bobt@contoso.com
   * SMTP: bob.taylor@contoso.com
   * **SMTP: bob@contoso.com**
9. Azure AD Connect wordt tijdens de synchronisatie, herkent de toevoeging van Bob Taylor in on-premises Active Directory en Azure AD naar dezelfde wijziging vragen.
10. Azure AD wordt eerst harde overeenkomst uitvoeren. Dat wil zeggen, wordt de zoekopdracht uitgevoerd als er een object met de immutableId gelijk is aan "abcdefghijkl0123456789 ==". Vaste overeenkomst mislukken omdat er geen andere objecten in Azure AD die immutableId zal hebben.
11. Azure AD wordt vervolgens probeert te zachte match Bob Taylor. Dat wil zeggen, wordt de zoekopdracht uitgevoerd als er een object met proxyAddresses gelijk zijn aan de drie waarden, met inbegrip van smtp: bob@contoso.com
12. Azure AD vindt van Bob Smith-object om te voldoen aan de zachte match-criteria. Maar dit object heeft de waarde van immutableId = "abcdefghijklmnopqrstuv ==". wat aangeeft dat dit object is gesynchroniseerd vanuit een ander object vanuit on-premises Active Directory. Dus, Azure AD kan geen zachte match deze objecten en de resultaten in een **InvalidSoftMatch** -synchronisatiefout.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Voor het oplossen van InvalidSoftMatch fout
De meest voorkomende reden voor de fout InvalidSoftMatch is twee objecten met andere SourceAnchor \(immutableId\) hebben dezelfde waarde voor de kenmerken ProxyAddresses en/of de UserPrincipalName, die worden gebruikt tijdens de zachte match verwerken in Azure AD. Als u wilt de ongeldige zachte Match oplossen

1. Identificeer de dubbele proxyAddresses, userPrincipalName of andere kenmerkwaarde die de fout wordt veroorzaakt. Ook bepalen welke twee \(of meer\) objecten betrokken zijn bij het conflict. Het rapport dat is gegenereerd door [Azure AD Connect Health voor synchroniseren](https://aka.ms/aadchsyncerrors) kunt u de twee objecten identificeren.
2. Bepalen welk object moet nog steeds de dubbele waarde en welk object moet niet.
3. Verwijder de dubbele waarde uit het object dat die waarde mag geen. U moet de wijziging aanbrengt in de map waar het object is afkomstig uit. In sommige gevallen moet u mogelijk een van de objecten in conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de on-premises AD, kunt u Azure AD Connect synchroniseren van de wijziging.

Foutenrapporten synchronisatie in Azure AD Connect Health voor synchroniseren worden elke 30 minuten bijgewerkt en de fouten van de laatste synchronisatiepoging bevatten.

> [!NOTE]
> ImmutableId, moet per definitie niet wijzigen in de levensduur van het object. Als Azure AD Connect is niet geconfigureerd met een aantal van de scenario's waarmee u rekening moet in de bovenstaande lijst, u kan terechtkomen in een situatie waarin Azure AD Connect wordt berekend met een andere waarde van het SourceAnchor voor de AD-object met dezelfde entiteit (dezelfde gebruiker/groep / Neem contact op met enzovoort) waarvoor een bestaande Azure AD-Object, dat u wilt blijven gebruiken.
>
>

#### <a name="related-articles"></a>Gerelateerde artikelen
* [Dubbele of ongeldige kenmerken te voorkomen dat de directory-synchronisatie in Office 365](https://support.microsoft.com/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Beschrijving
Wanneer Azure AD probeert te zacht overeenkomen met twee objecten, is het mogelijk dat twee objecten van verschillende "objecttype' (zoals gebruiker, groep, enz. Neem contact op met) hebben de dezelfde waarden voor de kenmerken die wordt gebruikt voor het uitvoeren van de zachte match. Duplicatie van deze kenmerken is niet toegestaan in Azure AD, kan de bewerking 'ObjectTypeMismatch' synchronisatiefout leiden.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Voorbeeldscenario's voor ObjectTypeMismatch fout
* Een e-mailtoegang beveiligingsgroep wordt gemaakt in Office 365. Beheerder voegt een nieuwe gebruiker of contactpersoon in on-premises AD (die niet wordt gesynchroniseerd met Azure AD nog) met dezelfde waarde voor het kenmerk ProxyAddresses als die van de Office 365-groep.

#### <a name="example-case"></a>Voorbeeld van de aanvraag
1. Beheerder maakt een nieuwe e-mailtoegang-beveiligingsgroep in Office 365 voor de btw-afdeling en biedt een e-mailadres als tax@contoso.com. Deze groep is de waarde van het kenmerk ProxyAddresses van toegewezen **smtp: tax@contoso.com**
2. Een nieuwe gebruiker lid wordt van Contoso.com en een account is bedoeld voor de gebruiker on-premises met de proxyAddress als **smtp: tax@contoso.com**
3. Wanneer Azure AD Connect het nieuwe gebruikersaccount synchroniseert, krijgt deze de fout 'ObjectTypeMismatch'.

#### <a name="how-to-fix-objecttypemismatch-error"></a>Voor het oplossen van ObjectTypeMismatch fout
De meest voorkomende reden voor de fout ObjectTypeMismatch is twee objecten van het andere type (gebruiker, groep, enz. Neem contact op met) hebben dezelfde waarde voor het kenmerk ProxyAddresses. Als u wilt de ObjectTypeMismatch oplossen:

1. Identificeren van de dubbele proxyAddresses (of een ander kenmerk) waarde die de fout wordt veroorzaakt. Ook bepalen welke twee \(of meer\) objecten betrokken zijn bij het conflict. Het rapport dat is gegenereerd door [Azure AD Connect Health voor synchroniseren](https://aka.ms/aadchsyncerrors) kunt u de twee objecten identificeren.
2. Bepalen welk object moet nog steeds de dubbele waarde en welk object moet niet.
3. Verwijder de dubbele waarde uit het object dat die waarde mag geen. Houd er rekening mee dat moet u de wijziging hebt aangebracht in de map waar het object is afkomstig uit. In sommige gevallen moet u mogelijk een van de objecten in conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de on-premises AD, kunt u Azure AD Connect synchroniseren van de wijziging. Rapport over synchronisatiefouten in Azure AD Connect Health voor synchroniseren wordt elke 30 minuten bijgewerkt en de fouten van de laatste synchronisatiepoging bevat.

## <a name="duplicate-attributes"></a>Dubbele kenmerken
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Beschrijving
Azure Active Directory-schema is niet toegestaan voor twee of meer objecten hebben dezelfde waarde van de volgende kenmerken. Dat is dat elk object in Azure AD wordt gedwongen om een unieke waarde van deze kenmerken voor een bepaald geval.

* proxyAddresses
* UserPrincipalName

Als Azure AD Connect probeert om een nieuw object toevoegen of bijwerken van een bestaand object met een waarde voor de bovenstaande kenmerken die al is toegewezen aan een ander object in Azure Active Directory, wordt de bewerking resulteert in de synchronisatiefout 'AttributeValueMustBeUnique'.

#### <a name="possible-scenarios"></a>Mogelijke scenario's:
1. Dubbele waarde is toegewezen aan een reeds gesynchroniseerd object, veroorzaakt een met een ander gesynchroniseerd object conflict.

#### <a name="example-case"></a>Voorbeeld van de aanvraag:
1. **Bob Smith** is een gesynchroniseerde gebruiker in Azure Active Directory vanuit on-premises Active Directory van contoso.com
2. Bob Smith **UserPrincipalName** on-premises is ingesteld als **bobs@contoso.com**.
3. Bob heeft ook de volgende waarden voor de **proxyAddresses** kenmerk:
   * SMTP: bobs@contoso.com
   * SMTP: bob.smith@contoso.com
   * **SMTP: bob@contoso.com**
4. Een nieuwe gebruiker **Bob Taylor**, wordt toegevoegd aan de on-premises Active Directory.
5. Bob Taylor van **UserPrincipalName** is ingesteld als **bobt@contoso.com**.
6. **Bob Taylor** heeft de volgende waarden voor de **ProxyAddresses** kenmerk i. SMTP: bobt@contoso.com ii. SMTP: bob.taylor@contoso.com
7. Bob Taylor van object wordt gesynchroniseerd met Azure AD is.
8. Beheerder besloten om bij te werken van Bob Taylor **ProxyAddresses** kenmerk met de volgende waarde: ik. **SMTP: bob@contoso.com**
9. Azure AD probeert te Bob Taylor van object bijwerken in Azure AD met de bovenstaande waarde, maar waarbij de bewerking mislukt als dat ProxyAddresses-waarde al aan Bob Smith, toegewezen is "AttributeValueMustBeUnique" fout tot gevolg.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Voor het oplossen van AttributeValueMustBeUnique fout
De meest voorkomende reden voor de fout AttributeValueMustBeUnique is twee objecten met andere SourceAnchor \(immutableId\) dezelfde waarde voor de ProxyAddresses en/of de UserPrincipalName kenmerken hebben. Als u wilt AttributeValueMustBeUnique fout oplossen

1. Identificeer de dubbele proxyAddresses, userPrincipalName of andere kenmerkwaarde die de fout wordt veroorzaakt. Ook bepalen welke twee \(of meer\) objecten betrokken zijn bij het conflict. Het rapport dat is gegenereerd door [Azure AD Connect Health voor synchroniseren](https://aka.ms/aadchsyncerrors) kunt u de twee objecten identificeren.
2. Bepalen welk object moet nog steeds de dubbele waarde en welk object moet niet.
3. Verwijder de dubbele waarde uit het object dat die waarde mag geen. Houd er rekening mee dat moet u de wijziging hebt aangebracht in de map waar het object is afkomstig uit. In sommige gevallen moet u mogelijk een van de objecten in conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de on-premises AD, kunt u Azure AD Connect synchroniseren van de wijziging voor de fout is opgelost.

#### <a name="related-articles"></a>Gerelateerde artikelen
-[Dubbele of ongeldige kenmerken te voorkomen dat de directory-synchronisatie in Office 365](https://support.microsoft.com/kb/2647098)

## <a name="data-validation-failures"></a>Mislukte gegevensvalidatie
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Beschrijving
Azure Active Directory worden afgedwongen voor verschillende beperkingen met betrekking tot de gegevens zelf voordat toe te staan dat gegevens worden geschreven naar de map. Deze beperkingen zijn om ervoor te zorgen dat eindgebruikers de best mogelijke ervaring tijdens het gebruik van de toepassingen die afhankelijk van deze gegevens zijn krijgen.

#### <a name="scenarios"></a>Scenario's
a. De waarde van het kenmerk UserPrincipalName bevat de ongeldige/niet-ondersteunde tekens.
b. Het kenmerk UserPrincipalName voldoet niet aan de vereiste indeling.

#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Voor het oplossen van IdentityDataValidationFailed fout
a. Zorg ervoor dat het kenmerk userPrincipalName is ondersteund tekens en de vereiste indeling.

#### <a name="related-articles"></a>Gerelateerde artikelen
* [Voorbereiden voor het inrichten van gebruikers via adreslijstsynchronisatie op Office 365](https://support.office.com/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)

### <a name="federateddomainchangeerror"></a>FederatedDomainChangeError
#### <a name="description"></a>Beschrijving
Deze aanvraag resulteert in een **"FederatedDomainChangeError"** fout synchroniseren wanneer het achtervoegsel van de UserPrincipalName van een gebruiker is gewijzigd van een federatief domein in een ander federatief domein.

#### <a name="scenarios"></a>Scenario's
Voor een gesynchroniseerde gebruiker, is het achtervoegsel UserPrincipalName van een federatief domein gewijzigd in een ander federatief domein on-premises. Bijvoorbeeld, *UserPrincipalName = bob@contoso.com*  is gewijzigd in *UserPrincipalName = bob@fabrikam.com* .

#### <a name="example"></a>Voorbeeld
1. Bob Smith, een account voor Contoso.com is, wordt toegevoegd als een nieuwe gebruiker in Active Directory met de UserPrincipalName bob@contoso.com
2. Bob wordt verplaatst naar een andere afdeling van Contoso.com, Fabrikam.com genoemd en zijn UserPrincipalName is gewijzigd in bob@fabrikam.com
3. Domeinen voor contoso.com en fabrikam.com zijn federatieve domeinen met Azure Active Directory.
4. Berend userPrincipalName niet wordt bijgewerkt en resulteert in een synchronisatiefout 'FederatedDomainChangeError'.

#### <a name="how-to-fix"></a>Voor het oplossen van
Als de UserPrincipalName-achtervoegsel van een gebruiker is bijgewerkt van bob @**contoso.com** naar bob @**fabrikam.com**, waarbij beide **contoso.com** en **fabrikam.com** zijn **federatieve domeinen**, klikt u vervolgens als volgt te werk om op te lossen de synchronisatiefout

1. Bijwerken van de gebruiker UserPrincipalName in Azure AD van bob@contoso.com naar bob@contoso.onmicrosoft.com. Met de Azure AD PowerShell-Module kunt u de volgende PowerShell-opdracht: `Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`
2. Toestaan dat de volgende synchronisatiecyclus om synchronisatie. Deze tijdsynchronisatie is voltooid en de UserPrincipalName van Bob aan wordt bijgewerkt bob@fabrikam.com zoals verwacht.

#### <a name="related-articles"></a>Gerelateerde artikelen
* [Wijzigingen worden niet gesynchroniseerd met het hulpprogramma Azure Active Directory Sync nadat u de UPN van een gebruikersaccount voor het gebruik van een ander federatief domein wijzigen](https://support.microsoft.com/help/2669550/changes-aren-t-synced-by-the-azure-active-directory-sync-tool-after-you-change-the-upn-of-a-user-account-to-use-a-different-federated-domain)

## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Beschrijving
Wanneer een kenmerk overschrijdt de maximale toegestane grootte, de maximale lengte of de limiet voor het aantal ingesteld door Azure Active Directory-schema, de synchronisatiebewerking resulteert in de **LargeObject** of **ExceededAllowedLength**-synchronisatiefout. Deze fout treedt meestal op voor de volgende kenmerken

* userCertificate
* userSMIMECertificate
* thumbnailPhoto
* proxyAddresses

### <a name="possible-scenarios"></a>Mogelijke scenario 's
1. Te veel certificaten die zijn toegewezen aan Bob opslaat Berend userCertificate kenmerk. Deze kunnen oudere, verlopen certificaten bevatten. De vaste limiet is 15 certificaten. Raadpleeg voor meer informatie over het verwerken van LargeObject-fouten met het kenmerk userCertificate artikel [afhandeling van LargeObject-fouten die zijn veroorzaakt door userCertificate kenmerk](tshoot-connect-largeobjecterror-usercertificate.md).
2. Te veel certificaten die zijn toegewezen aan Bob opslaat Berend usersmimecertificate wordt door kenmerk. Deze kunnen oudere, verlopen certificaten bevatten. De vaste limiet is 15 certificaten.
3. Berend thumbnailphoto wordt door instellen in Active Directory is te groot om te worden gesynchroniseerd in Azure AD.
4. Tijdens automatische populatie van het ProxyAddresses-kenmerk in Active Directory heeft een object te veel ProxyAddresses toegewezen.

### <a name="how-to-fix"></a>Voor het oplossen van
1. Zorg ervoor dat het kenmerk dat de fout veroorzaakt binnen de toegestane beperking.

## <a name="existing-admin-role-conflict"></a>Conflict met bestaande beheerdersrol

### <a name="description"></a>Beschrijving
Een **bestaande Admin rol Conflict** van een gebruikersobject tijdens de synchronisatie wordt uitgevoerd wanneer dat gebruikersobject heeft:

- beheerdersmachtigingen en
- de dezelfde UserPrincipalName als een bestaande Azure AD-object

Azure AD Connect is niet toegestaan op zachte overeenkomst met een gebruikersobject van on-premises AD met een gebruikersobject in Azure AD en waaraan een beheerdersrol toegewezen.  Zie voor meer informatie [Azure AD UserPrincipalName populatie](plan-connect-userprincipalname.md)

![Bestaande Admin](media/tshoot-connect-sync-errors/existingadmin.png)


### <a name="how-to-fix"></a>Voor het oplossen van
Om op te lossen dit probleem als volgt een van de volgende:


- Wijzig de UserPrincipalName in een waarde die komt niet overeen met die van een gebruiker met beheerdersrechten in Azure AD - Hiermee u een nieuwe gebruiker in Azure AD met de overeenkomende UserPrincipalName maakt
- Verwijder de beheerdersrol van de gebruiker met beheerdersrechten in Azure AD, zodat de zachte match tussen het on-premises-gebruikersobject en het bestaande object van de Azure AD-gebruiker.

>[!NOTE]
>U kunt de beheerdersrol toewijzen aan het bestaande gebruikersobject opnieuw nadat de zachte match tussen het on-premises-gebruikersobject en het Azure AD-gebruikersobject is voltooid.

## <a name="related-links"></a>Verwante koppelingen
* [Ga naar Active Directory-objecten in Active Directory-beheercentrum](https://technet.microsoft.com/library/dd560661.aspx)
* [Hoe u query's van Azure Active Directory voor een object met behulp van Azure Active Directory PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
