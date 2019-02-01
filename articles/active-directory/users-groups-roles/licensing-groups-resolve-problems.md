---
title: 'Oplossen van problemen met licentie-toewijzing voor een groep: Azure Active Directory | Microsoft Docs'
description: Het identificeren en oplossen van problemen toewijzing wanneer u Azure Active Directory-groep op basis van licentieverlening
services: active-directory
keywords: Azure AD-licenties
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.openlocfilehash: 2d12f07f8c65d3559f88de2c38c418ce4cc54188
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55510184"
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Identificeren en oplossen van problemen toewijzing voor een groep in Azure Active Directory

Groepsgebaseerde licentieverlening in Azure Active Directory (Azure AD) introduceert het concept van gebruikers in een licentie op de foutstatus. In dit artikel leggen we de redenen waarom gebruikers in deze status kunnen terechtkomen.

Wanneer u licenties rechtstreeks naar afzonderlijke gebruikers, toewijst zonder gebruik te maken op basis van een groep licenties, wordt de toewijzingsbewerking kan mislukken. Bijvoorbeeld, wanneer u de PowerShell-cmdlet uitvoert `Set-MsolUserLicense` op een gebruikerssysteem, de cmdlet om verschillende redenen die gerelateerd zijn aan zakelijke logica kan mislukken. Bijvoorbeeld, kunnen er onvoldoende licenties of een conflict tussen twee service-plannen die op hetzelfde moment kan niet worden toegewezen. Het probleem wordt onmiddellijk terug aan u gemeld.

Wanneer u op basis van een groep licenties, wordt de dezelfde fouten kunnen optreden, maar wordt ze optreden op de achtergrond, terwijl de Azure AD-service is licenties toewijzen. Om deze reden kunnen niet de fouten worden gecommuniceerd naar u onmiddellijk. In plaats daarvan ze geregistreerd op het gebruikersobject en wordt gerapporteerd via de portal met beheerdersrechten. Het oorspronkelijke doel voor een licentie van de gebruiker is nooit verloren, maar deze wordt vastgelegd in een foutstatus voor toekomstig onderzoek en de resolutie.

## <a name="how-to-find-license-assignment-errors"></a>Over het vinden van licentie fouten bij licentietoewijzing
**Licentie vinden fouten bij licentietoewijzing**

   1. Als u gebruikers in een foutstatus in een specifieke groep zoekt, opent u het deelvenster voor de groep. Onder **licenties**, een melding wordt weergegeven als er geen gebruikers in een foutstatus.

   ![Groep foutmelding](./media/licensing-groups-resolve-problems/group-error-notification.png)

   2. Selecteer de melding om een lijst van alle betrokken gebruikers te openen. U kunt elke gebruiker afzonderlijk voor meer informatie.

   ![Groep lijst met gebruikers in de foutstatus](./media/licensing-groups-resolve-problems/list-of-users-with-errors.png)

   3. Alle groepen met ten minste één fout op de **Azure Active Directory** blade selecteren **licenties**, en selecteer vervolgens **overzicht**. Een vak met informatie wordt weergegeven als groepen uw aandacht vereisen.

   ![Overzicht van informatie over groepen in de foutstatus](./media/licensing-groups-resolve-problems/group-errors-widget.png)

   4. Selecteer het selectievakje voor een overzicht van alle groepen met fouten. U kunt elke groep voor meer informatie.

   ![Overzicht van lijst met groepen met fouten](./media/licensing-groups-resolve-problems/list-of-groups-with-errors.png)


De volgende secties geven een beschrijving van elke mogelijke problemen en de manier om dit te verhelpen.

## <a name="not-enough-licenses"></a>Er zijn niet genoeg licenties

**Probleem:** Er zijn niet voldoende beschikbare licenties voor een van de producten die is opgegeven in de groep. U moet koop meer licenties voor het product of niet-gebruikte licenties van andere gebruikers of groepen vrij.

Als u wilt zien hoeveel licenties er beschikbaar zijn, gaat u naar **Azure Active Directory** > **licenties** > **alle producten**.

Als u wilt zien welke gebruikers en groepen licenties worden verbruikt, moet u een product selecteren. Onder **gelicentieerde gebruikers**, ziet u een lijst van alle gebruikers die licenties zijn toegewezen, rechtstreeks of via een of meer groepen hebben gehad. Onder **groepen in licentie gegeven**, ziet u alle groepen die deze producten zijn toegewezen.

**PowerShell:** PowerShell-cmdlets Meld deze fout als _CountViolation_.

## <a name="conflicting-service-plans"></a>Conflicterende serviceabonnementen

**Probleem:** Een van de producten die is opgegeven in de groep bevat een service-plan die conflicteert met een andere service-plan al toegewezen aan de gebruiker via een ander product. Sommige service-abonnementen zijn geconfigureerd op een manier dat ze niet worden toegewezen aan dezelfde gebruiker als een andere, gerelateerde service-plan.

Overweeg het volgende scenariovoorbeeld. Een gebruiker heeft een licentie voor Office 365 Enterprise *E1* toegewezen rechtstreeks met de abonnementen die zijn ingeschakeld. De gebruiker is toegevoegd aan een groep met de Office 365 Enterprise *E3* toegewezen aan het product. Het product E3 bevat service-plannen die elkaar niet overlappen met de abonnementen die zijn opgenomen in E1, zodat de groep licentietoewijzing is mislukt met de fout 'Conflicterende serviceabonnementen'. In dit voorbeeld zijn de conflicterende serviceabonnementen:

-   SharePoint Online (abonnement 2) veroorzaakt een conflict met SharePoint Online (abonnement 1).
-   Exchange Online (abonnement 2) veroorzaakt een conflict met Exchange Online (abonnement 1).

Als u wilt dit conflict is opgelost, moet u twee van de abonnementen uitschakelen. U kunt de E1-licentie die rechtstreeks toegewezen aan de gebruiker uitschakelen. Of u wilt wijzigen van de hele groep licentietoewijzing en uitschakelen van het abonnement in de E3-licentie. U kunt ook als u wilt verwijderen van de E1-licentie van de gebruiker als dit redundante in de context van de licentie E3.

De beslissing over het oplossen van conflicterende productlicenties altijd hoort bij de beheerder. Azure AD oplossen niet automatisch conflicten bij licentie.

**PowerShell:** PowerShell-cmdlets Meld deze fout als _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Andere producten zijn afhankelijk van deze licentie

**Probleem:** Een van de producten die is opgegeven in de groep bevat een service-plan dat moet worden ingeschakeld voor een andere service-plan, in een ander product, te laten functioneren. Deze fout treedt op wanneer Azure AD probeert te verwijderen van de onderliggende service-plan. Dit kan bijvoorbeeld gebeuren wanneer u de gebruiker uit de groep verwijderen.

U lost dit probleem, moet u om ervoor te zorgen dat de vereiste planning nog steeds aan gebruikers via een andere methode toegewezen wordt of dat de bijbehorende afhankelijke services zijn uitgeschakeld voor deze gebruikers. Nadat u dat doet, kunt u de groepslicentie correct verwijderen van deze gebruikers.

**PowerShell:** PowerShell-cmdlets Meld deze fout als _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Gebruikslocatie is niet toegestaan

**Probleem:** Sommige Microsoft-services zijn niet beschikbaar op alle locaties vanwege lokale wetten en voorschriften. Voordat u een licentie aan een gebruiker toewijzen kunt, moet u de **gebruikslocatie** eigenschap voor de gebruiker. Kunt u de locatie onder de **gebruiker** > **profiel** > **instellingen** sectie in Azure portal.

Wanneer Azure AD probeert een groepslicentie toewijzen aan een gebruiker waarvan gebruikslocatie wordt niet ondersteund, mislukt en registreert een fout op de gebruiker.

U lost dit probleem, gebruikers van de ondersteunde locaties van de gelicentieerde groep te verwijderen. U kunt ook als de huidige waarden voor de locatie van gebruik niet de locatie van het werkelijke vertegenwoordigen, kunt u ze aanpassen zodat de licenties correct volgende keer worden toegewezen (als de nieuwe locatie wordt ondersteund).

**PowerShell:** PowerShell-cmdlets Meld deze fout als _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> Wanneer Azure AD wordt toegewezen Groepslicenties, nemen alle gebruikers geen opgegeven gebruikslocatie de locatie van de map. U wordt aangeraden dat beheerders het juiste gebruik locatie waarden instellen voor gebruikers voordat u Groepslicenties om te voldoen aan de lokale wetten en voorschriften.

## <a name="duplicate-proxy-addresses"></a>Dubbele proxyadressen

Als u Exchange Online gebruikt, kunnen sommige gebruikers in uw tenant niet correct worden geconfigureerd met dezelfde waarde voor de proxy-adres. Wanneer Groepslicenties probeert een licentie toewijzen aan een gebruiker, mislukt en ziet u 'Proxy-adres is al in gebruik'.

> [!TIP]
> Om te zien of er een dubbele proxy-adres, voert u de volgende PowerShell-cmdlet voor Exchange Online:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> Zie voor meer informatie over dit probleem [foutbericht 'Proxy-adres is al in gebruik' in Exchange Online](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online). Het artikel bevat ook informatie over [verbinding maken met Exchange Online met behulp van externe PowerShell](https://technet.microsoft.com/library/jj984289.aspx). In dit artikel voor meer informatie Zie [op hoe het kenmerk proxyAddresses wordt gevuld in Azure AD](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad).

Nadat u eventuele problemen proxy-adres voor de betrokken gebruikers oplossen, zorg er dan voor dat forceren licentie-verwerking op de groep om ervoor te zorgen dat de licenties die nu kunnen worden toegepast.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Wat gebeurt er wanneer er meer dan één productlicentie op een groep?

U kunt meer dan één productlicentie toewijzen aan een groep. U kunt bijvoorbeeld Office 365 Enterprise E3 en Enterprise Mobility + Security toewijzen aan een groep inbegrepen services voor gebruikers eenvoudig inschakelen.

Azure AD wil alle licenties die zijn opgegeven in de groep aan elke gebruiker toe te wijzen. Als Azure AD kan niet een van de producten vanwege zakelijke logica problemen toewijst, wordt niet het ofwel de andere licenties in de groep toewijzen. Een voorbeeld is als er niet voldoende licenties voor alle, of als er conflicten met andere services die zijn ingeschakeld op de gebruiker zijn.

Hier ziet u de gebruikers die kunnen niet worden toegewezen en te controleren welke producten worden beïnvloed door dit probleem.

## <a name="what-happens-when-a-group-with-licenses-assigned-is-deleted"></a>Wat gebeurt er wanneer een groep met licenties die zijn toegewezen wordt verwijderd?

Alle licenties die zijn toegewezen aan een groep voordat u de groep kunt verwijderen, moet u verwijderen. Verwijderen van licenties van alle gebruikers in de groep kan echter tijd duren. Tijdens het toewijzen van licenties is verwijderd uit een groep, kunnen er fouten als de gebruiker heeft een afhankelijke licentie is toegewezen of als er een proxy-adres conflict probleem dat voorkomt het verwijderen van de licentie dat. Als een gebruiker een licentie die afhankelijk is van een licentie die wordt verwijderd vanwege verwijderen heeft, wordt de licentietoewijzing aan de gebruiker geconverteerd van overgenomen om te sturen.

Neem bijvoorbeeld een groep die Office 365 E3/E5 toegewezen met een Skype voor bedrijven-serviceplan ingeschakeld heeft. Ook Stel dat een aantal leden van de groep Audio vergaderingen licenties rechtstreeks toegewezen hebt. Wanneer de groep wordt verwijderd, probeert Groepslicenties te verwijderen voor alle gebruikers Office 365 E3/E5. Omdat de Audio-vergaderingen is afhankelijk van Skype voor bedrijven, voor alle gebruikers met een Audio-vergaderingen converteert toegewezen, groepsgebaseerde licentieverlening de Office 365 E3/E5-licenties om de licentietoewijzing.

## <a name="how-do-you-manage-licenses-for-products-with-prerequisites"></a>Hoe beheer ik licenties voor producten aan de vereisten?

Sommige Microsoft Online-producten die u mogelijk de eigenaar zijn *invoegtoepassingen*. Invoegtoepassingen is een vereiste service-plan voor een gebruiker of groep worden ingeschakeld voordat ze kunnen worden toegewezen licentie vereist. Met Groepslicenties, het systeem is vereist dat de vereiste en de invoegtoepassing voor service-abonnementen aanwezig zijn in dezelfde groep. Dit wordt gedaan om ervoor te zorgen dat alle gebruikers die zijn toegevoegd aan de groep het product volledig werkende kunnen ontvangen. Laten we eens bekijken in het volgende voorbeeld:

Microsoft Workplace Analytics is een invoegtoepassing. Het bevat een enkele service-plan met dezelfde naam. We kunnen dit service-plan alleen toewijzen aan een gebruiker of groep, wanneer een van de volgende vereisten is ook toegewezen:
- Exchange Online (abonnement 1) 
- Exchange Online (abonnement 2)

Als we dit product op een eigen toewijzen aan een groep willen, wordt er een fout geretourneerd in de portal. Selecteren van de foutmelding ziet u de volgende gegevens:

![Groep, vereiste ontbreekt](./media/licensing-groups-resolve-problems/group-prerequisite-required.png)

Als we de gegevens selecteert, ziet u de volgende strekking weergegeven:

>Licentiebewerking is mislukt. Zorg ervoor dat de groep de benodigde services bevat voordat toevoegen of verwijderen van een afhankelijke service. **De service Microsoft Workplace Analytics vereist Exchange Online (abonnement 2) ook worden ingeschakeld.**

Als u wilt deze invoegtoepassing-licentie toewijzen aan een groep, moet we ervoor zorgen dat de groep ook de vereiste service-plan bevat. We kunnen bijvoorbeeld bijwerken van een bestaande groep die al de volledige versie van Office 365 E3 bevat en vervolgens de invoegtoepassing-product toevoegen aan deze.

Het is ook mogelijk te maken van een zelfstandige-groep met alleen de minimaal vereiste producten als u wilt maken van de invoegtoepassing werken. Het kan worden gebruikt voor een licentie op alleen de geselecteerde gebruikers voor het product van de invoegtoepassing. In dit voorbeeld hebben we de volgende producten toegewezen aan dezelfde groep:
- Office 365 Enterprise E3 met alleen het Exchange Online (abonnement 2) service-plan ingeschakeld
- Microsoft Workplace Analytics

![Groep vereiste opgenomen](./media/licensing-groups-resolve-problems/group-addon-with-prerequisite.png)

Van nu af aan alle gebruikers die zijn toegevoegd aan deze groep een licentie van het product E3 en een licentie van het product Workplace Analytics gebruiken. Op hetzelfde moment, kunnen die gebruikers leden van een andere groep waarmee ze de volledige E3-product, en ze nog steeds er slechts één licentie voor het product gebruikt.

> [!TIP]
> U kunt meerdere groepen voor elke vereiste service-plan maken. Als u Office 365 Enterprise E1 en Office 365 Enterprise E3 voor uw gebruikers gebruiken, kunt u bijvoorbeeld twee groepen aan licentie Microsoft Workplace Analytics maken: een E1 gebruikt als een vereiste en de andere die gebruikmaakt van E3. Hiermee kunt u de invoegtoepassing E1 en E3 gebruikers distribueren zonder aanvullende licenties verbruikt.

## <a name="how-do-you-force-license-processing-in-a-group-to-resolve-errors"></a>Hoe Dwing u licentie-verwerking van een groep oplossen?

Afhankelijk van welke stappen u om op te lossen de fouten hebt gemaakt, kan het nodig zijn voor het handmatig activeren van de verwerking van een groep om bij te werken van de status van de gebruiker.

Bijvoorbeeld, als u licenties vrij door het verwijderen van rechtstreekse licentietoewijzingen van gebruikers, moet u voor het activeren van de verwerking van groepen die eerder is mislukt voor een volledige licentie op alle gebruikersleden van de. Als u wilt opnieuw met het verwerken van een groep, gaat u naar het deelvenster groep open **licenties**, en selecteer vervolgens de **opnieuw verwerken** op de werkbalk.

## <a name="how-do-you-force-license-processing-on-a-user-to-resolve-errors"></a>Hoe Dwing u licentie-verwerking van een gebruiker op te lossen fouten?

Afhankelijk van welke stappen u om op te lossen de fouten hebt gemaakt, kan het nodig zijn voor het handmatig activeren van de verwerking van een gebruiker aan het bijwerken van de status van de gebruikers.

Bijvoorbeeld, als u dubbele proxy-adres probleem voor een betrokken gebruiker hebt opgelost, moet u voor het activeren van de verwerking van de gebruiker. Als u wilt opnieuw verwerken van een gebruiker, gaat u naar de gebruiker-deelvenster, open **licenties**, en selecteer vervolgens de **opnieuw verwerken** op de werkbalk.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over andere scenario's voor Licentiebeheer via groepen:

* [Wat is licentieverlening in Azure Active Directory op basis van groep?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Licenties toewijzen aan een groep in Azure Active Directory](licensing-groups-assign.md)
* [Gebruikers met een afzonderlijke licentie migreren naar licenties op basis van groepen in Azure Active Directory](licensing-groups-migrate-users.md)
* [Het migreren van gebruikers tussen productlicenties groepsgebaseerde licentieverlening in Azure Active Directory gebruiken](licensing-groups-change-licenses.md)
* [Aanvullende scenario’s voor Azure Active Directory-licenties op basis van groepen](licensing-group-advanced.md)
* [PowerShell-voorbeelden voor Groepslicenties in Azure Active Directory](licensing-ps-examples.md)
