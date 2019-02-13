---
title: Groep op basis van aanvullende scenario's - Azure Active Directory-licentieverlening | Microsoft Docs
description: Meer scenario's voor Azure Active Directory-groep op basis van licentieverlening
services: active-directory
keywords: Azure AD-licenties
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7b194b8aeb6df54814069a3acd2e2f0f3ad1a13
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56180671"
---
# <a name="scenarios-limitations-and-known-issues-using-groups-to-manage-licensing-in-azure-active-directory"></a>Scenario's, beperkingen en bekende problemen met behulp van groepen beheren in Azure Active Directory-licentieverlening

Gebruik de volgende informatie en voorbeelden te krijgen van een uitgebreidere kennis van Azure Active Directory (Azure AD)-groep op basis van licentieverlening.

## <a name="usage-location"></a>Gebruikslocatie

Sommige services van Microsoft zijn niet op alle locaties beschikbaar. Voordat een licentie kan worden toegewezen aan een gebruiker, de beheerder heeft om op te geven de **gebruikslocatie** eigenschap van de gebruiker. In [de Azure-portal](https://portal.azure.com), kunt u opgeven in **gebruiker** &gt; **profiel** &gt; **instellingen**.

Voor licentietoewijzing groep, worden gebruikers geen gebruikslocatie opgegeven overgenomen in de locatie van de map. Als u gebruikers op meerdere locaties hebt, moet u om weer te geven die correct in uw gebruikersobjecten voordat u gebruikers toevoegt aan groepen met licenties.

> [!NOTE]
> Groep licentietoewijzing wordt nooit een bestaande waarde van de locatie gebruik van een gebruiker wijzigen. U wordt aangeraden dat u altijd gebruikslocatie ingesteld als onderdeel van de gebruikersstroom maken in Azure AD (bijvoorbeeld via AAD Connect-configuratie) - die ervoor het resultaat van licentietoewijzing dat zorgt altijd juist is en dat gebruikers ontvangen geen services op locaties die niet zijn toegestaan.

## <a name="use-group-based-licensing-with-dynamic-groups"></a>Gebruik Groepslicenties met dynamische groepen

U kunt gebruiken op basis van een groep met een beveiligingsgroep, wat betekent dat deze kan worden gecombineerd met dynamische groepen van Azure AD-licentieverlening. Dynamische groepen uitvoeren regels op basis van gebruiker-objectkenmerken om automatisch gebruikers toevoegen en verwijderen uit groepen.

Bijvoorbeeld, kunt u een dynamische groep voor een set producten die u wilt toewijzen aan gebruikers. Elke groep is ingevuld door een regel toe te voegen gebruikers door hun kenmerken en elke groep de licenties die u wilt laten ontvangen is toegewezen. U kunt de kenmerk on-premises toewijzen en deze synchroniseren met Azure AD, of kunt u het kenmerk rechtstreeks in de cloud beheren.

Licenties zijn toegewezen aan de gebruiker, kort nadat ze zijn toegevoegd aan de groep. Als het kenmerk wordt gewijzigd, wordt de gebruiker verlaat de groepen en de licenties worden verwijderd.

### <a name="example"></a>Voorbeeld

Bekijk het voorbeeld van een on-premises oplossing voor identiteitsbeheer die bepaalt welke gebruikers toegang krijgen tot webservices van Microsoft moeten hebben. Hierbij **extensionAttribute1** voor het opslaan van een string-waarde die aangeeft van de licenties voor de gebruiker moet beschikken. Azure AD Connect synchroniseert deze met Azure AD.

Gebruikers mogelijk moeten een licentie, maar niet nog een of beide nodig. Hier volgt een voorbeeld, waarin u Office 365 Enterprise E5 en Enterprise Mobility + Security (EMS)-licenties aan gebruikers in groepen distribueert:

#### <a name="office-365-enterprise-e5-base-services"></a>Office 365 Enterprise E5: basis-services

![Schermafbeelding van Office 365 Enterprise E5 base services](./media/licensing-group-advanced/o365-e5-base-services.png)

#### <a name="enterprise-mobility--security-licensed-users"></a>Enterprise Mobility + Security: gelicentieerde gebruikers

![Schermafbeelding van Enterprise Mobility + Security gelicentieerde gebruikers](./media/licensing-group-advanced/o365-e5-licensed-users.png)

In dit voorbeeld wijzigen van een gebruiker en hun extensionAttribute1 ingesteld op de waarde van `EMS;E5_baseservices;` als u wilt dat de gebruiker beide licenties hebt. Deze wijziging kunt u on-premises. Nadat de wijziging wordt gesynchroniseerd met de cloud, de gebruiker wordt automatisch toegevoegd aan beide groepen en licenties zijn toegewezen.

![Schermopname van het instellen van de gebruiker extensionAttribute1](./media/licensing-group-advanced/user-set-extensionAttribute1.png)

> [!WARNING]
> Wees voorzichtig bij het wijzigen van de regel voor verzameling van een bestaande groep. Als een regel wordt gewijzigd, is het lidmaatschap van de groep opnieuw geëvalueerd en gebruikers die niet meer overeen met de nieuwe regel worden verwijderd (gebruikers die nog steeds overeen met die de nieuwe regel wordt niet beïnvloed tijdens dit proces). Deze gebruikers hebben hun licenties verwijderd tijdens het proces die leiden verlies van de service of in sommige gevallen tot kan, verlies van gegevens.

> Als u een grote dynamische groep die u afhankelijk bent voor het toewijzen van licenties hebt, kunt u geen grote wijzigingen op een kleinere testgroep valideren voordat u deze toepast op de belangrijkste groep.

## <a name="multiple-groups-and-multiple-licenses"></a>Meerdere groepen en meerdere licenties

Een gebruiker kan lid zijn van meerdere groepen met licenties zijn. Hier volgen enkele aandachtspunten voor:

- Meerdere licenties voor hetzelfde product kunnen overlappen, en ze resulteren in alle ingeschakelde services die worden toegepast op de gebruiker. Het volgende voorbeeld ziet u twee licenties groepen: *E3 basisservices* de foundation-services voor het eerst implementeren voor alle gebruikers bevat. En *E3 services uitgebreid* bevat aanvullende services (Sway en Planner) om alleen op bepaalde gebruikers te implementeren. In dit voorbeeld wordt is de gebruiker toegevoegd aan beide groepen:

  ![Schermopname van het ingeschakelde services](./media/licensing-group-advanced/view-enabled-services.png)

  De gebruiker heeft als gevolg hiervan 7 van de 12 services in het product dat is ingeschakeld, terwijl u slechts één licentie voor dit product.

- Selecteren van de *E3* licentie bevat meer informatie, waaronder informatie over welke groepen veroorzaakt welke services zijn ingeschakeld voor de gebruiker.

  ![Schermopname van het ingeschakelde services per groep](./media/licensing-group-advanced/view-enabled-service-by-group.png)

## <a name="direct-licenses-coexist-with-group-licenses"></a>Licenties voor direct worden gecombineerd met Groepslicenties

Wanneer een gebruiker een licentie worden overgenomen uit een groep, kan u rechtstreeks verwijderen of wijzigen van deze toewijzing van licenties in de eigenschappen van de gebruiker. Wijzigingen moeten worden aangebracht in de groep en vervolgens doorgegeven aan alle gebruikers.

Het is echter wel mogelijk om toe te wijzen de dezelfde productlicentie rechtstreeks naar de gebruiker, naast de overgenomen licenties. U kunt aanvullende services van het product speciaal voor één gebruiker, zonder gevolgen voor andere gebruikers.

Direct toegewezen licenties kunnen worden verwijderd en niet van invloed op overgenomen licenties. Houd rekening met de gebruiker die een licentie voor Office 365 Enterprise E3 uit een groep overgenomen.

1. In eerste instantie, neemt de gebruiker over de licentie alleen vanaf de *E3 basisservices* groep waarmee vier service-abonnementen, zoals wordt weergegeven:

  ![Schermafbeelding van E3-groep ingeschakelde services](./media/licensing-group-advanced/e3-group-enabled-services.png)

2. U kunt selecteren **toewijzen** rechtstreeks een E3-licentie toewijzen aan de gebruiker. In dit geval wilt u alle serviceabonnementen, behalve Yammer Enterprise uitschakelen:

  ![Schermafbeelding van een licentie rechtstreeks aan een gebruiker toewijzen](./media/licensing-group-advanced/assign-license-to-user.png)

3. Als gevolg hiervan wordt de gebruiker nog steeds slechts één licentie van het product E3. Maar rechtstreekse toewijzing toe, schakelt u de Enterprise Yammer-service voor die gebruiker alleen. U kunt zien welke services zijn ingeschakeld door het groepslidmaatschap versus rechtstreekse toewijzing toe:

  ![Schermafbeelding van overgenomen ten opzichte van rechtstreekse toewijzing toe](./media/licensing-group-advanced/direct-vs-inherited-assignment.png)

4. Wanneer u directe toewijzing, worden de volgende bewerkingen zijn toegestaan:

  - Yammer-onderneming kan worden uitgeschakeld op het gebruikersobject rechtstreeks. De **aan/uit-** in-/ uitschakelen in de afbeelding is ingeschakeld voor deze service, in plaats van de andere service in-of uitschakelen. Omdat de service rechtstreeks op de gebruiker is ingeschakeld, kan worden gewijzigd.
  - Aanvullende services kunnen ook worden ingeschakeld, als onderdeel van de licentie rechtstreeks toegewezen.
  - De **verwijderen** knop kan worden gebruikt om de directe licentie verwijderen van de gebruiker. U kunt zien dat de gebruiker nu alleen de overgenomen groepslicentie heeft en alleen de oorspronkelijke services ingeschakeld blijven:

    ![Schermopname die laat zien hoe u directe toewijzing verwijderen](./media/licensing-group-advanced/remove-direct-license.png)

## <a name="managing-new-services-added-to-products"></a>Beheren van nieuwe services toegevoegd aan producten
Wanneer Microsoft een nieuwe service aan een product toevoegt, wordt deze standaard in alle groepen waarop u de productlicentie hebt toegewezen worden ingeschakeld. Gebruikers in uw tenant die zijn geabonneerd op meldingen over de productwijzigingen, e-mailberichten tevoren om ze te informeren over de aanstaande service-toevoegingen ontvangen.

U kunt als beheerder, alle groepen die worden beïnvloed door een wijziging bekijken en actie ondernemen, zoals het uitschakelen van de nieuwe service in elke groep. Bijvoorbeeld, als u groepen die zijn gericht op alleen specifieke services voor de implementatie gemaakt, kunt u terugkeren naar deze groepen en zorg ervoor dat alle nieuwe toegevoegde services zijn uitgeschakeld.

Hier volgt een voorbeeld van hoe dit proces eruit kan zien:

1. Oorspronkelijk is aan u toegewezen de *Office 365 Enterprise E5* product aan verschillende groepen. Een van deze groepen, genaamd *Office 365 E5 - alleen Exchange* is ontworpen om in te schakelen, alleen de *Exchange Online (abonnement 2)* service zodat de leden ervan.

2. U hebt een melding ontvangen van Microsoft dat het product E5 zal worden uitgebreid met een nieuwe service - *Microsoft Stream*. Wanneer de service beschikbaar is in uw tenant, kunt u het volgende doen:

3. Ga naar de [ **Azure Active Directory > licenties > alle producten** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) blade en selecteer *Office 365 Enterprise E5*en selecteer vervolgens **in licentie gegeven groepen** om een lijst van alle groepen met dat product weer te geven.

4. Klik op de groep die u wenst te controleren (in dit geval *Office 365 E5 - alleen Exchange*). Hiermee opent u de **licenties** tabblad. Te klikken op de licentie E5 wordt een blade geopend met alle ingeschakelde services.
> [!NOTE]
> De *Microsoft Stream* service is automatisch toegevoegd en ingeschakeld in deze groep, in aanvulling op de *Exchange Online* service:

  ![Schermafbeelding van de nieuwe service toegevoegd aan een groepslicentie](./media/licensing-group-advanced/manage-new-services.png)

5. Als u uitschakelen van de nieuwe service in deze groep wilt, klikt u op de **aan/uit-** naast de service in-/ uitschakelen en klik op de **opslaan** knop om de wijziging te bevestigen. Azure AD wordt nu alle gebruikers in de groep om toe te passen van de wijziging; verwerken nieuwe gebruikers toegevoegd aan de groep heeft niet de *Microsoft Stream* -service is ingeschakeld.

  > [!NOTE]
  > Gebruikers hebben mogelijk nog steeds de service is ingeschakeld via een andere licentietoewijzing (een andere groep ze lid zijn van of een rechtstreekse licentietoewijzing zijn).

6. Indien nodig, de dezelfde stappen uitvoeren voor andere groepen met dit product dat is toegewezen.

## <a name="use-powershell-to-see-who-has-inherited-and-direct-licenses"></a>PowerShell gebruiken om te zien wie heeft overgenomen en directe licenties
U kunt een PowerShell-script gebruiken om te controleren of gebruikers een licentie die rechtstreeks is toegewezen of overgenomen van een groep.

1. Voer de `connect-msolservice` cmdlet om te verifiëren en verbinding maken met uw tenant.

2. `Get-MsolAccountSku` kan worden gebruikt voor het detecteren van alle ingerichte productlicenties in de tenant.

  ![Schermafbeelding van de cmdlet Get-Msolaccountsku](./media/licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. Gebruik de *AccountSkuId* waarde voor de licentie die u geïnteresseerd in met bent [dit PowerShell-script](licensing-ps-examples.md#check-if-user-license-is-assigned-directly-or-inherited-from-a-group). Het resultaat is een lijst van gebruikers met deze licentie met de informatie over hoe de licentie is toegewezen.

## <a name="use-audit-logs-to-monitor-group-based-licensing-activity"></a>Auditlogboeken gebruiken voor het bewaken van licentieverlening activiteit op basis van een groep

U kunt [Azure AD-auditlogboeken](../reports-monitoring/concept-audit-logs.md#audit-logs) om te zien van alle activiteiten die betrekking hebben op groepen gebaseerde licentieverlening, met inbegrip van:
- wie licenties voor groepen heeft gewijzigd
- Wanneer het systeem de verwerking van een wijziging van de licentie groep, en wanneer deze voltooid
- welke licentiewijzigingen zijn aangebracht aan een gebruiker als gevolg van een groep licentietoewijzing.

>[!NOTE]
> Auditlogboeken zijn beschikbaar op de meeste in de sectie Azure Active Directory van de portal-blades. Afhankelijk van waar u ze openen, mogelijk filters vooraf toegepast op de activiteit alleen relevant zijn voor de context van de blade weergeven. Als u niet de verwachte resultaten ziet, controleert u [de filteropties](../reports-monitoring/concept-audit-logs.md#filtering-audit-logs) of toegang krijgen tot de ongefilterde auditlogboeken onder [ **Azure Active Directory > activiteit > auditlogboeken** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Audit).

### <a name="find-out-who-modified-a-group-license"></a>Ontdek wie een groepslicentie gewijzigd

1. Stel de **activiteit** filteren op *groepslicentie instellen* en klikt u op **toepassen**.
2. De resultaten bevatten alle gevallen van licenties worden ingesteld of gewijzigd op groepen.
>[!TIP]
> U kunt ook Typ de naam van de groep in de *doel* filter als bereik voor de resultaten.

3. Klik op een item in de lijstweergave kunt u de details van wat is gewijzigd. Onder *eigenschappen gewijzigd* oude en nieuwe waarden voor het toewijzen van licenties worden weergegeven.

Hier volgt een voorbeeld van recente groep licentiewijzigingen, met meer informatie:

![Schermafbeelding van groep licentiewijzigingen](./media/licensing-group-advanced/audit-group-license-change.png)

### <a name="find-out-when-group-changes-started-and-finished-processing"></a>Nagaan wanneer wijzigingen aan de slag en verwerking is voltooid

Wanneer een licentie voor een groep wordt gewijzigd, wordt Azure AD gestart de wijzigingen toepassen op alle gebruikers.

1. Om te zien wanneer groepen met de verwerking, stel de **activiteit** filteren op *Start toepassen op basis van groepslicentie op gebruikers*. Houd er rekening mee dat de actor voor de bewerking is *Microsoft Azure AD-groep op basis van licentieverlening* -een systeemaccount dat wordt gebruikt voor het uitvoeren van alle wijzigingen in licentie.
>[!TIP]
> Klik op een item in de lijst om te zien de *eigenschappen gewijzigd* field - deze ziet u de licentiewijzigingen die zijn opgehaald voor de verwerking. Dit is handig als u meerdere wijzigingen aan een groep aangebracht en u niet zeker weet welke is verwerkt.

2. Op dezelfde manier als u wilt zien wanneer groepen verwerking is voltooid, gebruiken de filterwaarde *toepassen op basis van groepslicentie op gebruikers voltooien*.
>[!TIP]
> In dit geval de *eigenschappen gewijzigd* veld bevat een overzicht van de resultaten: dit is handig om snel te controleren als verwerking leidde tot fouten. Voorbeelduitvoer:
> ```
Modified Properties
...
Name : Result
Old Value : []
New Value : [Users successfully assigned licenses: 6, Users for whom license assignment failed: 0.];
> ```

3. Als u wilt zien van het volledige logboek voor hoe een groep is verwerkt, met inbegrip van alle wijzigingen van de gebruiker, stel de volgende filters:
  - **Gestart door (Actor)**: "Microsoft Azure AD-groep op basis van licentieverlening"
  - **Datumbereik** (optioneel): aangepast bereik voor wanneer u een specifieke groep weet gestart en de verwerking is voltooid

In dit voorbeeld van de uitvoer ziet u het begin van de verwerking, alle resulterende wijzigingen van gebruiker en de einddatum van de verwerking.

![Schermafbeelding van groep licentiewijzigingen](./media/licensing-group-advanced/audit-group-processing-log.png)

>[!TIP]
> Te klikken op items die betrekking hebben op *gebruikerslicentie wijzigen* details voor de licentiewijzigingen toegepast op elke individuele gebruiker wordt weergegeven.

## <a name="deleting-a-group-with-an-assigned-license"></a>Als u een groep met een toegewezen licentie

Het is niet mogelijk om te verwijderen van een groep met een actieve licentie is toegewezen. Een beheerder kan een groep niet realiseren dat hierdoor licenties moeten worden verwijderd uit de gebruikers - om deze reden dat hebben we alle licenties voor het eerst worden verwijderd uit de groep, voordat deze kan worden verwijderd nodig verwijderen.

Bij het verwijderen van een groep in Azure portal ziet u mogelijk een foutmelding als volgt: ![Kan de schermopname groep verwijderen](./media/licensing-group-advanced/groupdeletionfailed.png)

Ga naar de **licenties** tabblad in de groep en zien of er licenties zijn toegewezen. Zo ja, deze licenties verwijderen en probeer opnieuw te verwijderen de groep.

Mogelijk ziet u vergelijkbare fouten bij het verwijderen van de groep via PowerShell of Graph API. Als u een groep gesynchroniseerd vanuit on-premises gebruikt, kan Azure AD Connect ook fouten rapporteren als het niet lukt om te verwijderen van de groep in Azure AD. In dergelijke gevallen moet u controleren of er licenties zijn toegewezen aan de groep zijn en deze eerst verwijderen.

## <a name="limitations-and-known-issues"></a>Beperkingen en bekende problemen

Als u licentieverlening op basis van een groep, is het een goed idee om vertrouwd te raken met de volgende lijst met beperkingen en bekende problemen.

- Groepslicenties op dit moment biedt geen ondersteuning voor groepen met andere groepen (geneste groepen). Als u een licentie toepast op een geneste groep, wordt die licentie alleen toegepast op de gebruikers die lid direct zijn van de groep.

- De functie kan alleen worden gebruikt met beveiligingsgroepen en Office 365-groepen waarvoor securityenabled moet = TRUE.

- De [Office 365-beheerportal](https://portal.office.com ) ondersteunt momenteel geen Groepslicenties. Als een gebruiker een licentie worden overgenomen uit een groep, wordt deze licentie in de Office-beheerportal weergegeven als een gewone gebruiker-licentie. Als u probeert te wijzigen die licentie of probeer te verwijderen van de licentie, retourneert de portal een foutbericht weergegeven. Overgenomen Groepslicenties worden niet rechtstreeks op een gebruiker gewijzigd.

- Wanneer de licenties worden toegewezen of worden gewijzigd voor een grote groep (bijvoorbeeld 100.000 gebruikers), kan dit de prestaties beïnvloeden. Het volume van de wijzigingen die worden gegenereerd door Azure AD-automatisering mogelijk specifiek, negatieve invloed hebben op de prestaties van uw directory-synchronisatie tussen Azure AD en on-premises systemen.

- Als u gebruikmaakt van dynamische groepen om het lidmaatschap van de gebruiker te beheren, controleert u of de gebruiker deel uitmaakt van de groep. Dit is noodzakelijk voor het toewijzen van licenties. Als dit niet het geval is, [controleert u de verwerkingsstatus voor de lidmaatschapsregel](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule#check-processing-status-for-a-membership-rule) van de dynamische groep. 

- In bepaalde situaties hoge belasting duurt lang verwerkt licentiewijzigingen voor groepen of wijzigingen in het lidmaatschap aan groepen met licenties. Als u uw wijzigingen worden meer dan 24 uur voor het verwerken van groep grootte van 60K gebruikers of minder, neem [open een ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/supportRequest) om toe te staan te onderzoeken. 

- Licentie management automation niet automatisch reageren op alle soorten wijzigingen in de omgeving. Bijvoorbeeld, u waarschijnlijk te weinig licenties, waardoor sommige gebruikers zich in een foutstatus. Aan vrij te maken van het aantal beschikbare stoelen, kunt u sommige rechtstreeks toegewezen licenties van andere gebruikers. Het systeem echter niet automatisch reageren op deze wijziging en gebruikers in de status van deze fout corrigeren.

  Als tijdelijke oplossing voor dit soort beperkingen, gaat u naar de **groep** -blade in Azure AD, en klikt u op **opnieuw verwerken**. Met deze opdracht alle gebruikers in die groep worden verwerkt en wordt de status fout, indien mogelijk opgelost.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over scenario’s voor licentiebeheer via licenties op basis van groepen, raadpleegt u:

* [Wat is licentieverlening in Azure Active Directory op basis van groep?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Licenties toewijzen aan een groep in Azure Active Directory](licensing-groups-assign.md)
* [Licentieproblemen voor een groep vaststellen en oplossen in Azure Active Directory](licensing-groups-resolve-problems.md)
* [Gebruikers met een afzonderlijke licentie migreren naar licenties op basis van groepen in Azure Active Directory](licensing-groups-migrate-users.md)
* [Het migreren van gebruikers tussen productlicenties groepsgebaseerde licentieverlening in Azure Active Directory gebruiken](../users-groups-roles/licensing-groups-change-licenses.md)
* [PowerShell-voorbeelden voor Groepslicenties in Azure Active Directory](../users-groups-roles/licensing-ps-examples.md)
