---
title: Oplossen van licentieproblemen voor een groep in Azure Active Directory | Microsoft Docs
description: Het identificeren en oplossen van toewijzing licentieproblemen wanneer u Azure Active Directory-groepen gebaseerde licentieverlening
services: active-directory
keywords: Azure AD-licentieverlening
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5bd28eeb8d67dc0dcb3303fdb0e3c20b32f7c431
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Het identificeren en oplossen van problemen met licentie-toewijzing voor een groep in Azure Active Directory

Op basis van een groep licentieverlening in Azure Active Directory (Azure AD), wordt het concept van gebruikers in een foutstatus licentieverlening geïntroduceerd. In dit artikel leggen we de redenen waarom de gebruikers in deze status terechtkomen mogelijk.

Als u licenties rechtstreeks naar afzonderlijke gebruikers zonder gebruik te maken op basis van een groep licentieverlening toewijst, mislukken de toewijzingsbewerking. Bijvoorbeeld bij het uitvoeren van de PowerShell-cmdlet `Set-MsolUserLicense` op een gebruikerssysteem, de cmdlet om verschillende redenen die gerelateerd zijn aan de zakelijke logica kan mislukken. Bijvoorbeeld, kunnen er onvoldoende licenties of een conflict tussen twee serviceniveaus die op hetzelfde moment kan niet worden toegewezen. Het probleem onmiddellijk gerapporteerd terug.

Wanneer u Groepsbeleid-licentieverlening, wordt de dezelfde fouten kunnen optreden, maar wordt ze op de achtergrond gebeurt, terwijl de Azure AD-service is licenties toewijzen. Daarom moet kunnen niet de fouten worden gecommuniceerd naar u onmiddellijk. Ze zijn in plaats daarvan geregistreerd op het gebruikersobject en vervolgens via de portal beheerdersrechten gerapporteerd. Het oorspronkelijke doel licentie voor de gebruiker is nooit verloren, maar deze wordt vastgelegd in een foutstatus voor toekomstig onderzoek en omzetten.

## <a name="how-to-find-license-assignment-errors"></a>Licentie toewijzingsfouten zoeken
**Licentie toewijzingsfouten vinden**

   1. Voor gebruikers in een foutstatus van een specifieke groep vinden, opent u het deelvenster voor de groep. Onder **licenties**, wordt een melding weergegeven als er geen gebruikers in een foutstatus.

   ![Groep foutmeldingen](media/active-directory-licensing-group-problem-resolution-azure-portal/group-error-notification.png)

   2. Selecteer de melding om een lijst van alle betrokken gebruikers te openen. U kunt elke gebruiker afzonderlijk voor meer informatie.

   ![Groep lijst met gebruikers in de foutstatus](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-users-with-errors.png)

   3. Alle groepen met ten minste één fout op de **Azure Active Directory** blade Selecteer **licenties**, en selecteer vervolgens **overzicht**. Een vak met informatie wordt weergegeven als groepen uw aandacht vereisen.

   ![Overzicht van informatie over groepen in de foutstatus](media/active-directory-licensing-group-problem-resolution-azure-portal/group-errors-widget.png)

   4. Selecteer het selectievakje voor een overzicht van alle groepen met fouten. U kunt elke groep voor meer informatie.

   ![Overzicht van lijst met groepen met fouten](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-groups-with-errors.png)


De volgende secties geven een beschrijving van elke potentieel probleem en de manier te verhelpen.

## <a name="not-enough-licenses"></a>Er zijn niet genoeg licenties

**Probleem:** er niet voldoende beschikbare licenties voor een van de producten die is opgegeven in de groep. U moet meer licenties koopt voor het product of niet-gebruikte licenties van andere gebruikers of groepen vrijmaken.

Hoeveel licenties beschikbaar zijn, Ga naar **Azure Active Directory** > **licenties** > **alle producten**.

Als u wilt zien welke gebruikers en groepen licenties verbruiken, selecteer een product. Onder **gelicentieerde gebruikers**, ziet u een lijst van alle gebruikers die direct of via een of meer groepen toegewezen licenties hebben gehad. Onder **groepen in licentie gegeven**, ziet u alle groepen waarop deze producten zijn toegewezen.

**PowerShell:** PowerShell-cmdlets rapporteer deze fout als _CountViolation_.

## <a name="conflicting-service-plans"></a>Conflicterende serviceabonnementen

**Probleem:** een van de producten die is opgegeven in de groep bevat een service-abonnement dat conflicteert met een andere service-abonnement al aan de gebruiker via een ander product toegewezen is. Sommige service-abonnementen zijn geconfigureerd op een manier die ze voor dezelfde gebruiker als een andere, gerelateerde service-abonnement kunnen niet worden toegewezen.

Bekijk het volgende voorbeeld. Een gebruiker een licentie voor Office 365 Enterprise heeft *E1* toegewezen rechtstreeks met de abonnementen die zijn ingeschakeld. De gebruiker is toegevoegd aan een groep met de Office 365 Enterprise *E3* product toegewezen. Het product E3 bevat service-abonnementen die elkaar niet overlappen met de abonnementen die zijn opgenomen in E1, zodat de licentietoewijzing van de groep is mislukt met de fout "Serviceplannen conflicterende". In dit voorbeeld zijn de conflicterende serviceplannen:

-   SharePoint Online (2 plannen) veroorzaakt een conflict met SharePoint Online (1 plannen).
-   Exchange Online (2 plannen) veroorzaakt een conflict met Exchange Online (1 plannen).

U lost dit conflict, moet u twee van de abonnementen uitschakelen. De licentie E1 die rechtstreeks aan de gebruiker toegewezen, kunt u uitschakelen. Of u moet de licentietoewijzing van de hele groep wijzigen en de plannen in de E3-licentie uitschakelen. U kunt ook moet u besluiten de licentie E1 van de gebruiker wordt verwijderd als deze in de context van de licentie E3 redundant.

De beslissing over het oplossen van conflicterende productlicenties altijd hoort bij de beheerder. Azure AD oplossen niet automatisch licentie conflicten.

**PowerShell:** PowerShell-cmdlets rapporteer deze fout als _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Andere producten zijn afhankelijk van deze licentie

**Probleem:** een van de producten die is opgegeven in de groep bevat een serviceplan dat moet worden ingeschakeld voor een andere service-abonnement in een ander product, te laten functioneren. Deze fout treedt op wanneer Azure AD probeert te verwijderen van de onderliggende service-abonnement. Dit kan bijvoorbeeld gebeuren wanneer de gebruiker te uit de groep verwijderen.

U lost dit probleem, moet u ervoor zorgen dat het vereiste plan nog is toegewezen aan gebruikers via een andere methode, of dat de afhankelijke services zijn uitgeschakeld voor deze gebruikers. Na dat doet, kunt u de groepslicentie goed verwijderen van deze gebruikers.

**PowerShell:** PowerShell-cmdlets rapporteer deze fout als _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Gebruikslocatie is niet toegestaan

**Probleem:** sommige Microsoft-services zijn niet beschikbaar op alle locaties vanwege lokale wet- en regelgeving. Voordat u een licentie aan een gebruiker toewijzen kunt, moet u de **gebruikslocatie** eigenschap voor de gebruiker. U kunt opgeven dat de locatie onder de **gebruiker** > **profiel** > **instellingen** sectie in de Azure-portal.

Wanneer Azure AD probeert een groepslicentie toewijzen aan een gebruiker waarvan gebruikslocatie wordt niet ondersteund, mislukt en registreert een fout op de gebruiker.

U lost dit probleem, gebruikers van de ondersteunde locaties van de gelicentieerde groep te verwijderen. U kunt ook als de huidige locatie waarden voor informatie over het gebruik niet de locatie van de werkelijke gebruiker vertegenwoordigen, kunt u ze aanpassen zodat de licenties correct volgende keer zijn toegewezen (als de nieuwe locatie wordt ondersteund).

**PowerShell:** PowerShell-cmdlets rapporteer deze fout als _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> Als Azure AD groep licenties toewijst, nemen alle gebruikers zonder een opgegeven gebruikslocatie de locatie van de map. Het is raadzaam dat beheerders het juiste gebruik locatie waarden instellen voor gebruikers voordat u op basis van een groep licensing om te voldoen aan de lokale wet- en regelgeving.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Wat gebeurt er wanneer er meer dan één productlicentie op een groep?

U kunt meer dan één productlicentie toewijzen aan een groep. U kunt bijvoorbeeld Office 365 Enterprise E3 en Enterprise Mobility + Security toewijzen aan een groep voor alle inbegrepen services voor gebruikers eenvoudig in te schakelen.

Azure AD probeert toe te wijzen alle licenties die zijn opgegeven in de groep aan elke gebruiker. Als Azure AD kan niet een van de producten vanwege zakelijke logica problemen toewijst, won't deze ofwel de andere licenties in de groep toewijzen. Een voorbeeld is als er niet voldoende licenties voor alle, of als er conflicten met andere services die zijn ingeschakeld op de gebruiker zijn.

Hier ziet u de gebruikers die worden toegewezen en te controleren welke producten worden beïnvloed door dit probleem is mislukt.

## <a name="how-do-you-manage-licenses-for-products-with-prerequisites"></a>Hoe beheert u licenties voor producten met vereisten?

Sommige Microsoft Online-producten die u mogelijk de eigenaar zijn *invoegtoepassingen*. Invoegtoepassingen vereisen een vereiste service-abonnement worden ingeschakeld voor een gebruiker of een groep voordat ze een licentie kunnen worden toegewezen. Met Groepsbeleid-licentieverlening vereist het systeem dat de vereiste en de invoegtoepassing serviceplannen aanwezig zijn in dezelfde groep. Dit wordt gedaan om ervoor te zorgen dat gebruikers die zijn toegevoegd aan de groep het product volledig werkend kunnen ontvangen. Laten we eens bekijken in het volgende voorbeeld:

Microsoft werkplek Analytics is een invoegtoepassing. Het bevat een één service-abonnement met dezelfde naam. We kunnen dit service-abonnement alleen toewijzen aan een gebruiker of groep, wanneer een van de volgende vereisten is ook toegewezen:
- Exchange Online (Plan 1) 
- Exchange Online (Plan 2)

Als we dit product op een eigen toewijzen aan een groep proberen, wordt in de portal een fout geretourneerd. Als u de foutmelding ziet u de volgende details:

![Groep, vereiste ontbreekt](media/active-directory-licensing-group-problem-resolution-azure-portal/group-prerequisite-required.png)

Als we de details selecteert, ziet u het volgende foutbericht weergegeven:

>Licentie-bewerking is mislukt. Zorg ervoor dat de groep de benodigde services heeft vóór het toevoegen of verwijderen van een afhankelijke service. **De service Microsoft werkplek Analytics vereist Exchange Online (2 plannen) ook worden ingeschakeld.**

Als u wilt deze invoegtoepassing licentie toewijzen aan een groep, moet we ervoor zorgen dat de groep bevat ook het vereiste service-abonnement. We kunnen bijvoorbeeld bijwerken van een bestaande groep die al deel uit van het volledige product van de Office 365 E3 en vervolgens de invoegproduct toe te voegen aan deze.

Het is ook mogelijk te maken van een zelfstandige groep met alleen de minimale vereiste producten om de invoegtoepassing werkt. Het kan worden gebruikt alleen de geselecteerde gebruikers voor het product extra licenties aan. In dit voorbeeld hebben we de volgende producten toegewezen aan dezelfde groep:
- Office 365 Enterprise E3 met alleen het Exchange Online (Plan 2) service-abonnement ingeschakeld
- Microsoft werkplek Analytics

![Groep vereiste opgenomen](media/active-directory-licensing-group-problem-resolution-azure-portal/group-addon-with-prerequisite.png)

Alle gebruikers die zijn toegevoegd aan deze groep verbruiken van nu aan een licentie van het product E3 en een licentie van het product werkplek Analytics. Op hetzelfde moment die gebruikers kunnen uitmaken van een andere groep waarmee ze het volledige E3-product en ze nog steeds slechts één licentie voor dit product gebruiken.

> [!TIP]
> U kunt meerdere groepen voor elke vereiste service-abonnement maken. Als u Office 365 Enterprise E1 en Office 365 Enterprise E3 voor uw gebruikers gebruiken, kunt u bijvoorbeeld twee groepen aan licentie Microsoft werkplek Analytics maken: een E1 gebruikt als een vereiste en de andere die gebruikmaakt van E3. Hiermee kunt u de invoegtoepassing E1 en E3 gebruikers distribueren zonder aanvullende licenties verbruikt.

## <a name="license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online"></a>De licentietoewijzing niet achtergrond voor een gebruiker vanwege dubbele proxyadressen in Exchange Online

Als u Exchange Online gebruikt, kunnen sommige gebruikers in uw tenant niet juist zijn geconfigureerd met dezelfde waarde voor de proxy-adres. Wanneer een licentie toewijzen aan een gebruiker op basis van een groep licentieverlening probeert, mislukt en een fout niet worden geregistreerd. De fout voor het vastleggen van de fout in dit exemplaar is een beperking in de preview-versie van deze functie en gaan we voor het oplossen van het eerder *algemene beschikbaarheid*.

> [!TIP]
> Als u merkt dat sommige gebruikers geen licentie is ontvangen en er geen fout voor deze gebruikers vastgelegd is, moet u eerst controleren als ze een dubbele proxy-adres hebben.
> Uitvoeren om te zien of er een dubbele proxy-adres, de volgende PowerShell-cmdlet op basis van Exchange Online:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> Zie voor meer informatie over dit probleem [foutbericht 'Proxy-adres wordt al gebruikt' in Exchange Online](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online). Het artikel bevat ook informatie over [verbinding maken met Exchange Online met externe PowerShell](https://technet.microsoft.com/library/jj984289.aspx).

Nadat u eventuele problemen proxy-adres van de betrokken gebruikers hebt opgelost, zorg er dan voor dat forceren licentie-verwerking op de groep om ervoor te zorgen dat de licenties nu kunnen worden toegepast.

## <a name="how-do-you-force-license-processing-in-a-group-to-resolve-errors"></a>Hoe u de verwerking van de licentie in een groep fouten op te lossen afdwingen?

Afhankelijk van welke stappen u de fouten op te lossen hebt gemaakt, kan het nodig zijn voor de verwerking van een groep bijwerken van de status van de gebruiker handmatig opnieuw starten.

Bijvoorbeeld, als u een aantal licenties vrijmaken door de directe licentietoewijzingen van gebruikers te verwijderen, moet u voor het activeren van de verwerking van de groepen die eerder volledig licentie voor alle leden van de gebruiker is mislukt. Als u wilt opnieuw met het verwerken van een groep, gaat u naar het deelvenster groep open **licenties**, en selecteer vervolgens de **opnieuw verwerken** op de werkbalk.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over andere scenario's voor Licentiebeheer via groepen:

* [Licenties toewijzen aan een groep in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Wat is licentieverlening in Azure Active Directory op basis van groep?](active-directory-licensing-whatis-azure-portal.md)
* [Het migreren van afzonderlijke gebruikers met een licentie aan op basis van een groep licentieverlening in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory-licentieverlening voor aanvullende scenario's op basis van groep](active-directory-licensing-group-advanced.md)
