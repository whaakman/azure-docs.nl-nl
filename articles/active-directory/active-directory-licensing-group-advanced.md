---
title: Azure Active Directory-licentieverlening voor aanvullende scenario's op basis van groep | Microsoft Docs
description: Meer scenario's voor Azure Active Directory op basis van een groep licentieverlening
services: active-directory
keywords: Azure AD-licentieverlening
documentationcenter: 
author: curtand
manager: mtillman
editor: piotrci
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/02/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cf18076c81ecf7471771674fe40d36dba3b9866d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="scenarios-limitations-and-known-issues-using-groups-to-manage-licensing-in-azure-active-directory"></a>Scenario's, beperkingen en bekende problemen met behulp van groepen voor licentieverlening in Azure Active Directory beheren

Gebruik de volgende informatie en voorbeelden om te krijgen van een uitgebreidere kennis van Azure Active Directory (Azure AD) op basis van een groep licenties.

## <a name="usage-location"></a>Gebruikslocatie

Sommige Microsoft-services zijn niet beschikbaar op alle locaties. Voordat u een licentie kan worden toegewezen aan een gebruiker, de beheerder heeft om op te geven de **gebruikslocatie** eigenschap van de gebruiker. In [de Azure-portal](https://portal.azure.com), kunt u opgeven in **gebruiker** &gt; **profiel** &gt; **instellingen**.

Voor de licentietoewijzing van de groep, worden alle gebruikers zonder een gebruikslocatie opgegeven overgenomen door de locatie van de map. Als u gebruikers op meerdere locaties hebt, moet u om weer te geven die correct in uw gebruikersobjecten voordat u gebruikers toevoegt aan groepen met licenties.

> [!NOTE]
> Licentie groepstoewijzing wordt nooit een bestaande waarde van de locatie van informatie over het gebruik van een gebruiker wijzigen. U wordt aangeraden dat u altijd gebruikslocatie instellen als onderdeel van de stroom van de gebruiker maken in Azure AD (bijvoorbeeld via AAD Connect-configuratie) - het resultaat van de licentietoewijzing zo dat altijd juist is en gebruikers ontvangen geen services op locaties die niet zijn toegestaan.

## <a name="use-group-based-licensing-with-dynamic-groups"></a>Gebruik op basis van een groep met dynamische groepen-licentieverlening

U kunt gebruiken met een beveiligingsgroep, wat betekent dat kan worden gecombineerd met Azure AD-dynamische groepen op basis van een groep licenties. Dynamische groepen uitvoeren regels op basis van gebruiker objectkenmerken automatisch toevoegen en gebruikers verwijderen uit groepen.

Bijvoorbeeld, kunt u een dynamische groep voor bepaalde set van producten die u wilt toewijzen aan gebruikers. Elke groep wordt gevuld door een regel voor het toevoegen van gebruikers met hun kenmerken, en elke groep wordt toegewezen licenties die u wilt ontvangen. U kunt de kenmerk on-premises toewijzen en gesynchroniseerd met Azure AD, of kunt u het kenmerk rechtstreeks in de cloud beheren.

Licenties zijn toegewezen aan de gebruiker kort nadat ze zijn toegevoegd aan de groep. Wanneer het kenmerk is gewijzigd, wordt de gebruiker verlaat de groepen en de licenties worden verwijderd.

### <a name="example"></a>Voorbeeld

Bekijk het voorbeeld van een lokale oplossing voor identiteitsbeheer die bepaalt welke gebruikers toegang krijgen tot webservices Microsoft moeten hebben. Hierbij **extensionAttribute1** voor het opslaan van een string-waarde die aangeeft van de licenties die de gebruiker mag hebben. Azure AD Connect wordt deze gesynchroniseerd met Azure AD.

Gebruikers mogelijk moeten een licentie maar niet nog een of beide moet. Hier volgt een voorbeeld, waarin u Office 365 Enterprise E5 en Enterprise Mobility + Security (EMS)-licenties voor gebruikers in groepen distribueert:

#### <a name="office-365-enterprise-e5-base-services"></a>Office 365 Enterprise E5: base services

![Schermopname van Office 365 Enterprise E5 basisservices](media/active-directory-licensing-group-advanced/o365-e5-base-services.png)

#### <a name="enterprise-mobility--security-licensed-users"></a>Enterprise Mobility + Security: gelicentieerde gebruikers

![Schermopname van Enterprise Mobility + Security gelicentieerde gebruikers](media/active-directory-licensing-group-advanced/o365-e5-licensed-users.png)

Bijvoorbeeld, een gebruiker wijzigt en hun extensionAttribute1 ingesteld op de waarde van `EMS;E5_baseservices;` als u wilt dat de gebruiker beide licenties hebt. Deze wijziging kunt u lokale. Nadat de wijziging wordt gesynchroniseerd met de cloud, de gebruiker wordt automatisch toegevoegd aan beide groepen en licenties zijn toegewezen.

![Schermopname van het instellen van de gebruiker extensionAttribute1](media/active-directory-licensing-group-advanced/user-set-extensionAttribute1.png)

> [!WARNING]
> Wees voorzichtig bij het wijzigen van de regel voor lidmaatschap van een bestaande groep. Wanneer een regel wordt gewijzigd, is het lidmaatschap van de groep opnieuw geëvalueerd en gebruikers die niet langer overeenkomen met de nieuwe regel worden verwijderd (gebruikers die nog steeds overeen met die de nieuwe regel worden niet beïnvloed tijdens dit proces). Deze gebruikers hebben de licenties worden verwijderd tijdens het proces wat leiden verlies van de service of in sommige gevallen tot kan, verlies van gegevens.

> Als u een grote dynamische groep die u afhankelijk van de licentietoewijzing hebt, overweeg geen grote wijzigingen op een kleinere testgroep valideren voordat u deze toepast op de belangrijkste groep.

## <a name="multiple-groups-and-multiple-licenses"></a>Meerdere groepen en meerdere licenties

Een gebruiker kan lid zijn van meerdere groepen met licenties zijn. Hier volgen enkele overwegingen:

- Er kunnen meerdere licenties voor hetzelfde product overlappen en ze leiden tot alle ingeschakelde services wordt toegepast op de gebruiker. Het volgende voorbeeld ziet u twee licentieverlening groepen: *E3 basisservices* de foundation-services voor het eerst implementeren voor alle gebruikers bevat. En *E3 services uitgebreide* bevat aanvullende services (invloed en Planner) alleen om bepaalde gebruikers implementeren. In dit voorbeeld wordt is de gebruiker toegevoegd aan beide groepen:

  ![Schermopname van ingeschakelde services](media/active-directory-licensing-group-advanced/view-enabled-services.png)

  De gebruiker heeft als gevolg hiervan 7 van de 12 services in het product ingeschakeld tijdens het gebruik van slechts één licentie voor dit product.

- Als u de *E3* licentie bevat meer informatie, zoals informatie over welke groepen veroorzaakt wat services zijn ingeschakeld voor de gebruiker.

  ![Schermopname van ingeschakelde services per groep](media/active-directory-licensing-group-advanced/view-enabled-service-by-group.png)

## <a name="direct-licenses-coexist-with-group-licenses"></a>Directe licenties worden gecombineerd met de groep licenties

Wanneer een gebruiker een licentie uit een groep, u niet rechtstreeks verwijderen of wijzigen die de licentietoewijzing in de eigenschappen van de gebruiker. Wijzigingen moeten worden aangebracht in de groep en vervolgens doorgegeven aan alle gebruikers.

Het is echter mogelijk, de dezelfde productlicentie rechtstreeks naar de gebruiker, naast de overgenomen licentie toewijzen. U kunt extra services vanaf de product-slechts voor één gebruiker inschakelen zonder gevolgen voor andere gebruikers.

Direct toegewezen licenties kunnen worden verwijderd, en niet van invloed op overgenomen licenties. Houd rekening met de gebruiker die een licentie voor Office 365 Enterprise E3 van een groep overneemt.

1. In eerste instantie, neemt de gebruiker over de licentie alleen via de *E3 basisservices* groep, waardoor vier serviceplannen, zoals wordt weergegeven:

  ![Schermopname van E3 groep services ingeschakeld](media/active-directory-licensing-group-advanced/e3-group-enabled-services.png)

2. U kunt selecteren **toewijzen** rechtstreeks een E3-licentie toewijzen aan de gebruiker. U gaat in dit geval alle service-abonnementen, behalve Yammer Enterprise uitschakelen:

  ![Schermafbeelding van een licentie toewijzen rechtstreeks aan een gebruiker](media/active-directory-licensing-group-advanced/assign-license-to-user.png)

3. Als gevolg hiervan, de gebruiker nog steeds slechts één licentie van het product E3 gebruikt. Maar rechtstreekse toewijzing toe, schakelt u de service Enterprise Yammer voor alleen die gebruiker. U kunt zien welke services zijn ingeschakeld door het groepslidmaatschap versus rechtstreekse toewijzing toe:

  ![Schermopname van overgenomen versus rechtstreekse toewijzing](media/active-directory-licensing-group-advanced/direct-vs-inherited-assignment.png)

4. Wanneer u rechtstreekse toewijzing, mogen de volgende bewerkingen:

  - Yammer Enterprise kan worden uitgeschakeld op het gebruikersobject rechtstreeks. De **in- en uitgeschakeld** in-of uitschakelen in de illustratie is ingeschakeld voor deze service, in plaats van de andere service in-of uitschakelen. Omdat de service rechtstreeks op de gebruiker is ingeschakeld, kan worden gewijzigd.
  - Extra services kunnen ook als onderdeel van de rechtstreeks toegewezen licentie worden ingeschakeld.
  - De **verwijderen** knop kan worden gebruikt voor het verwijderen van de directe licentie van de gebruiker. U kunt zien dat de gebruiker nu alleen de overgenomen groepslicentie heeft, en alleen de oorspronkelijke services ingeschakeld blijven:

    ![Schermopname die laat zien hoe u kunt rechtstreekse toewijzing verwijderen](media/active-directory-licensing-group-advanced/remove-direct-license.png)

## <a name="managing-new-services-added-to-products"></a>Het beheren van nieuwe services toegevoegd aan de producten
Wanneer Microsoft een nieuwe service aan een product toevoegt, wordt het standaard in alle groepen waarop u de productlicentie hebt toegewezen worden ingeschakeld. Gebruikers in uw tenant die zijn geabonneerd op meldingen over de productwijzigingen, e-mailberichten tevoren melding over de aanstaande service toevoegingen ontvangen.

Als beheerder, kunt u bekijken van alle groepen waarop de wijziging en de actie uitvoert, bijvoorbeeld het uitschakelen van de nieuwe service in elke groep. Bijvoorbeeld, als u groepen als doel voor de implementatie van alleen specifieke services hebt gemaakt, kunt u terugkeren naar deze groepen en zorg ervoor dat alle nieuw toegevoegde services zijn uitgeschakeld.

Hier volgt een voorbeeld van hoe dit proces eruit mogelijk:

1. Oorspronkelijk is aan u toegewezen de *Office 365 Enterprise E5* product verschillende beheergroepen. Een van deze groepen, genaamd *O365 E5 - alleen Exchange* is ontworpen om in te schakelen, alleen de *Exchange Online (2 plannen)* service voor de bijbehorende leden.

2. U hebt een melding ontvangen van Microsoft die het product E5 zal worden uitgebreid met een nieuwe service - *Microsoft Stream*. Zodra de service beschikbaar in uw tenant, kunt u het volgende doen:

3. Ga naar de [ **Azure Active Directory > licenties > alle producten** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) blade en selecteer *Office 365 Enterprise E5*, selecteer daarna **in licentie gegeven groepen** voor een lijst van alle groepen met dat product.

4. Klik op de groep die u wilt controleren (in dit geval *O365 E5 - alleen Exchange*). Hiermee opent u de **licenties** tabblad. Te klikken op de E5-licentie wordt een blade geopend met alle ingeschakelde services.
> [!NOTE]
> De *Microsoft Stream* service is automatisch toegevoegd en ingeschakeld in deze groep, naast de *Exchange Online* service:

  ![Schermafbeelding van de nieuwe service die wordt toegevoegd aan een groepslicentie](media/active-directory-licensing-group-advanced/manage-new-services.png)

5. Als u uitschakelen van de nieuwe service in deze groep wilt, klikt u op de **in- en uitgeschakeld** naast de service in-of uitschakelen en klik op de **opslaan** knop de wijziging te bevestigen. Azure AD wordt nu verwerkt alle gebruikers in de groep toe te passen, de wijziging; nieuwe gebruikers toegevoegd aan de groep heeft niet de *Microsoft Stream* -service is ingeschakeld.

  > [!NOTE]
  > Gebruikers hebben mogelijk nog steeds de service is ingeschakeld via een aantal andere licentietoewijzing (een andere groep zijn leden van of de licentietoewijzing van een directe).

6. Indien nodig, de dezelfde stappen uitvoeren voor andere groepen met dit product is toegewezen.

## <a name="use-powershell-to-see-who-has-inherited-and-direct-licenses"></a>PowerShell gebruiken om te zien wie heeft overgenomen en direct licenties
U kunt een PowerShell-script gebruiken om te controleren of gebruikers beschikken over een licentie toegewezen rechtstreeks of overgenomen van een groep.

1. Voer de `connect-msolservice` cmdlet om te verifiëren en verbinding maken met uw tenant.

2. `Get-MsolAccountSku`kan worden gebruikt voor het detecteren van alle ingerichte productlicenties in de tenant.

  ![Schermafbeelding van de cmdlet Get-Msolaccountsku](media/active-directory-licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. Gebruik de *AccountSkuId* waarde voor de licentie dat u geïnteresseerd in met bent [dit PowerShell-script](./active-directory-licensing-ps-examples.md#check-if-user-license-is-assigned-directly-or-inherited-from-a-group). Het resultaat is een lijst met gebruikers die deze licentie met de informatie over hoe de licentie is toegewezen.

## <a name="use-audit-logs-to-monitor-group-based-licensing-activity"></a>Controlelogboeken gebruiken voor het bewaken van licentieverlening activiteit op basis van een groep

U kunt [auditlogboeken van Azure AD](./active-directory-reporting-activity-audit-logs.md#audit-logs) voor een overzicht van alle activiteiten gerelateerd aan licenties op basis van een groep, met inbegrip van:
- die licenties van groepen die zijn gewijzigd
- Wanneer het systeem begonnen met het verwerken van een wijziging in de groep licentie en wanneer het voltooid
- welke licentie wijzigingen zijn aangebracht aan een gebruiker als gevolg van een licentie groepstoewijzing.

>[!NOTE]
> Controlelogboeken zijn beschikbaar op de meeste blades in de Azure Active Directory-sectie van de portal. Afhankelijk van waar u toegang toe, kunnen de filters zijn vooraf toegepast op de activiteit alleen relevant zijn voor de context van de blade weergeven. Als u niet de verwachte resultaten ziet, controleert u [de filteropties](./active-directory-reporting-activity-audit-logs.md#filtering-audit-logs) of toegang tot de ongefilterde controlelogboeken onder [ **Azure Active Directory > activiteit > controlelogboeken** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Audit).

### <a name="find-out-who-modified-a-group-license"></a>Weten wie een groepslicentie gewijzigd

1. Stel de **activiteit** filteren op *Set groepslicentie* en klik op **toepassen**.
2. De resultaten bevatten alle gevallen van de licenties worden ingesteld of gewijzigd op groepen.
>[!TIP]
> U kunt ook typen met de naam van de groep in de *doel* filter als bereik voor de resultaten.

3. Klik op een item in de lijstweergave kunt u de details van wat is er gewijzigd. Onder *eigenschappen gewijzigd* oude en nieuwe waarden voor de licentietoewijzing staan.

Hier volgt een voorbeeld van recente groep licentie wijzigingen met details:

![Schermopname licentie wijzigingen](media/active-directory-licensing-group-advanced/audit-group-license-change.png)

### <a name="find-out-when-group-changes-started-and-finished-processing"></a>Nagaan wanneer wijzigingen gestart en wordt uitgevoerd

Wanneer een licentie voor een groep wordt gewijzigd, moet u Azure AD gaat de wijzigingen toepassen op alle gebruikers.

1. U kunt bekijken als groepen verwerking gestart, stellen de **activiteit** filteren op *Start groep gebaseerd licentie wordt toegepast op gebruikers*. Opmerking: de actor voor de bewerking is *Microsoft Azure AD op basis van een groep Licensing* -een systeem-account dat wordt gebruikt voor het uitvoeren van alle wijzigingen van de groep licentie.
>[!TIP]
> Klik op een item in de lijst om te zien de *eigenschappen gewijzigd* veld - wordt de licentie-wijzigingen die zijn opgepikt voor verwerking. Dit is handig als u meerdere wijzigingen in een groep aangebracht en u niet zeker weet welk is verwerkt.

2. Op deze manier om te zien waarop verwerking is beëindigd, groepen, gebruiken de filterwaarde *voltooien groep gebaseerd licentie wordt toegepast op gebruikers*.
>[!TIP]
> In dit geval de *eigenschappen gewijzigd* veld bevat een overzicht van de resultaten - dit is handig om snel te controleren als verwerking geleid tot fouten. Voorbeelduitvoer:
> ```
Modified Properties
...
Name : Result
Old Value : []
New Value : [Users successfully assigned licenses: 6, Users for whom license assignment failed: 0.];
> ```

3. Stel de volgende filters voor het volledige logboek voor hoe een groep is verwerkt, met inbegrip van alle wijzigingen van de gebruiker, Zie:
  - **Gestart door (Actor)**: ' Microsoft Azure AD op basis van een groep Licensing '
  - **Datumbereik** (optioneel): aangepast bereik voor wanneer u een specifieke groep weet gestart en wordt uitgevoerd

Deze voorbeelduitvoer bevat de begindatum van de verwerking, alle resulterende wijzigingen van gebruiker en de einddatum van de verwerking.

![Schermopname licentie wijzigingen](media/active-directory-licensing-group-advanced/audit-group-processing-log.png)

>[!TIP]
> Te klikken op items die zijn gerelateerd aan *wijziging gebruikerslicentie* details voor licentie-wijzigingen die zijn toegepast op elke individuele gebruiker wordt weergegeven.

## <a name="deleting-a-group-with-an-assigned-license"></a>Als u een groep met een toegewezen licentie

Het is niet mogelijk om te verwijderen van een groep met een actieve licentie toegewezen. Een beheerder kan verwijderen van een groep niet realiseren dat hierdoor licenties worden verwijderd uit de gebruikers - om deze reden moet alle licenties voor het eerst worden verwijderd uit de groep, voordat deze kan worden verwijderd.

Wanneer u probeert te verwijderen van een groep in de Azure-portal wordt er een foutmelding als volgt: ![schermafbeelding groep verwijderen is mislukt](media/active-directory-licensing-group-advanced/groupdeletionfailed.png)

Ga naar de **licenties** tabblad in de groep en zien of er licenties die zijn toegewezen zijn. Zo ja, deze licenties verwijderen en probeer opnieuw te verwijderen de groep.

Mogelijk ziet u dergelijke fouten tijdens het verwijderen van de groep via PowerShell of Graph API. Als u een groep gesynchroniseerd van on-premises gebruikt, kan Azure AD Connect ook fouten rapporteren als het niet lukt om te verwijderen van de groep in Azure AD. Controleer of er licenties die zijn toegewezen aan de groep zijn in dergelijke gevallen en deze eerst verwijderen.

## <a name="limitations-and-known-issues"></a>Bekende problemen en beperkingen

Als u Groepsbeleid-licentieverlening, is het een goed idee om vertrouwd te raken met de volgende lijst met bekende problemen en beperkingen.

- Op dit moment op basis van een groep licensing biedt geen ondersteuning voor groepen die geen andere groepen (geneste groepen). Als u een licentie aan een groep geneste toepast, hebben alleen de leden van de groep onmiddellijke eerste niveau gebruiker de licenties die zijn toegepast.

- De functie kan alleen worden gebruikt met beveiligingsgroepen. Office-groepen worden momenteel niet ondersteund en u zich niet kunnen worden gebruikt in het toewijzingsproces van licentie.

- De [Office 365-beheerportal](https://portal.office.com ) momenteel niet ondersteund op basis van een groep licenties. Als u een licentie voor een gebruiker overneemt van een groep, wordt deze licentie weergegeven in de Office-beheerportal als een gewone gebruiker-licentie. Als u probeert te wijzigen die licentie of probeert te verwijderen van de licentie, wordt een foutbericht geretourneerd in de portal. Overgenomen groep licenties worden niet rechtstreeks op een gebruiker gewijzigd.

- Wanneer een gebruiker uit een groep wordt verwijderd en de licentie verliest, service-abonnementen die licentie (bijvoorbeeld, SharePoint Online) zijn ingesteld op een **onderbroken** status. De service-abonnementen zijn niet ingesteld op een definitieve, uitgeschakelde status. Deze voorzorgsmaatregel kunt onbedoeld verwijderen van gebruikersgegevens, voorkomen als een beheerder een fout in het groepsbeheer lidmaatschap maakt.

- Wanneer licenties zijn toegewezen of gewijzigd voor een grote groep (bijvoorbeeld 100.000 gebruikers), kan dit de prestaties beïnvloeden. In het bijzonder de omvang van de wijzigingen die zijn gegenereerd door Azure AD-automatisering mogelijk negatieve invloed hebben op de prestaties van uw directory-synchronisatie tussen Azure AD en on-premises systemen.

- In bepaalde situaties hoge belasting licentie verwerking mogelijk een vertraging en wijzigingen zoals het toevoegen/verwijderen van een groep licentie of gebruikers toevoegen/verwijderen uit groep kunnen lang duren om te worden verwerkt. Als u de wijzigingen worden meer dan 24 uur om te verwerken, neem [een ondersteuningsticket opent](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/supportRequest) om toe te staan om te onderzoeken. We prestatiekenmerken van deze functie verbetert voordat deze bereiken *algemene beschikbaarheid*.

- Licentie management automation reageert automatisch niet op alle soorten wijzigingen in de omgeving. Bijvoorbeeld, u mogelijk hebt uitgevoerd buiten de licenties, waardoor sommige gebruikers zich in een foutstatus. Aan om het aantal beschikbare seat vrij te maken kunt u sommige rechtstreeks toegewezen licenties van andere gebruikers. Het systeem echter niet automatisch reageren op deze wijziging en gebruikers in de status van deze fout corrigeren.

  Als tijdelijke oplossing voor deze typen beperkingen, gaat u naar de **groep** blade in Azure AD en klik op **opnieuw verwerken**. Met deze opdracht alle gebruikers in die groep verwerkt en wordt de status fout indien mogelijk opgelost.

- Op basis van een groep licentieverlening registreert geen fouten wanneer een licentie kan niet worden toegewezen aan een gebruiker als gevolg van een dubbele proxy-adresconfiguratie in Exchange Online; Deze gebruikers worden overgeslagen tijdens de licentietoewijzing. Zie voor meer informatie over het identificeren en oplossen van dit probleem [in deze sectie](./active-directory-licensing-group-problem-resolution-azure-portal.md#license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over andere scenario's voor Licentiebeheer via Groepsbeleid licentieverlening:

* [Wat is licentieverlening in Azure Active Directory op basis van groep?](active-directory-licensing-whatis-azure-portal.md)
* [Licenties toewijzen aan een groep in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Opsporen en oplossen van licentieproblemen met een voor een groep in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Het migreren van afzonderlijke gebruikers met een licentie aan op basis van een groep licentieverlening in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
