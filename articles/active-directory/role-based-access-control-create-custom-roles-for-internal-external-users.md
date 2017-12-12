---
title: Aangepaste op rollen gebaseerde toegang besturingselement functies maken en toewijzen aan de interne en externe gebruikers in Azure | Microsoft Docs
description: Aangepaste RBAC-rollen gemaakt met behulp van PowerShell en CLI voor interne en externe gebruikers toewijzen
services: active-directory
documentationcenter: 
author: andreicradu
manager: mtillman
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/06/2017
ms.author: a-crradu
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: b3b65812d453a9f7d93ee4381c4261e685a60376
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="intro-on-role-based-access-control"></a>Inleiding op rollen gebaseerd toegangsbeheer

Toegangsbeheer op basis van rollen is een Azure portal alleen functie waardoor de eigenaren van een abonnement gedetailleerde rollen toewijzen aan andere gebruikers die een specifieke bron scopes in hun omgeving kunnen beheren.

RBAC kunt beter beveiligingsbeheer voor grote organisaties en voor midden-en kleinbedrijf werkt met externe deelnemers, leveranciers of freelancers die toegang tot specifieke bronnen in uw omgeving, maar niet per se aan de gehele infrastructuur of alle scopes facturering-gerelateerde nodig. RBAC kunt de flexibiliteit van die eigenaar is van één Azure-abonnement beheerd door de administrator-account (service-beheerdersrol op abonnementsniveau) en hebben meerdere gebruikers uitgenodigd onder hetzelfde abonnement, maar zonder Administrator-rechten voor het werken. Vanuit een management en facturering perspectief wordt de functie RBAC blijkt te zijn van een tijd- en efficiënte optie voor het gebruik van Azure in verschillende scenario's.

## <a name="prerequisites"></a>Vereisten
Het gebruik van RBAC in de Azure-omgeving vereist:

* Een zelfstandige met Azure-abonnement aan de gebruiker toegewezen als eigenaar (abonnement rol)
* De rol van eigenaar van het Azure-abonnement hebt
* Toegang tot de [Azure-portal](https://portal.azure.com)
* Zorg ervoor dat u hebt de volgende Resource Providers geregistreerd voor het abonnement van de gebruiker: **Microsoft.Authorization**. Zie voor meer informatie over het registreren van de resourceproviders [Resource Manager-providers, regio's, API-versies en schema's](../azure-resource-manager/resource-manager-supported-services.md).

> [!NOTE]
> Abonnementen voor Office 365 of Azure Active Directory-licenties (bijvoorbeeld: toegang tot Azure Active Directory) vanaf de Office 365 Admin center niet in aanmerking komen voor het gebruik van RBAC wordt ingericht.

## <a name="how-can-rbac-be-used"></a>Hoe RBAC kan worden gebruikt
RBAC kan worden toegepast op drie verschillende bereiken in Azure. Van het hoogste bereik met de laagste zijn ze als volgt:

* Abonnement (hoogste)
* Resourcegroep
* Resource-bereik (het laagste toegangsniveau aanbieding gerichte machtigingen voor het bereik van een afzonderlijke Azure-resource)

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>RBAC-rollen op de scope abonnement toewijzen
Er zijn twee algemene voorbeelden wanneer RBAC is gebruikt (maar niet tot beperkt):

* Externe gebruikers van de organisaties hebben (die geen deel uit van de gebruiker admin Azure Active Directory-tenant) uitgenodigd om bepaalde bronnen of het hele abonnement te beheren
* Werken met gebruikers binnen de organisatie (ze zijn onderdeel van de gebruiker Azure Active Directory-tenant), maar deel uitmaken van verschillende teams of groepen die gedetailleerde toegang tot het hele abonnement of bepaalde resourcegroepen of resource-scopes in de omgeving nodig

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Toegang verlenen op het abonnementsniveau van een voor een gebruiker buiten Azure Active Directory
RBAC-rollen kunnen alleen worden toegekend **eigenaars** van het abonnement daarom de gebruiker met beheerdersrechten moet zijn aangemeld met een gebruikersnaam die heeft deze een vooraf toegewezen rol of het Azure-abonnement is gemaakt.

Nadat u aanmelden als beheerder, selecteert u vanuit de Azure-portal 'Abonnementen' in en kies de gewenste versie.
![abonnementsblade in Azure-portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) standaard als het Azure-abonnement heeft aangeschaft door de gebruiker met beheerdersrechten de gebruiker wordt weergegeven als **accountbeheerder**, dit wordt de rol van het abonnement. Zie voor meer informatie over de functies van Azure-abonnement, [toevoegen of wijzigen Azure-beheerdersrollen die het abonnement of de services beheren](/billing/billing-add-change-azure-subscription-administrator.md).

In dit voorbeeld wordt de gebruiker "alflanigan@outlook.com' is de **eigenaar** van de 'gratis proefversie' abonnement in het AAD-tenant 'tenant Azure Default'. Omdat deze gebruiker de maker van het Azure-abonnement met de eerste 'Outlook' van de Microsoft-Account (Microsoft-Account = Outlook, etc. Live) naam van het standaarddomein voor andere gebruikers die zijn toegevoegd aan deze tenant zijn **'@alflaniganuoutlook.onmicrosoft.com'**. Door het ontwerp van de syntaxis van het nieuwe domein wordt gevormd door het samenstellen van de naam van de gebruikersnaam en het domein van de gebruiker die de tenant wordt gemaakt en het toevoegen van de extensie **'. onmicrosoft.com '**.
Bovendien gebruikers kunnen aanmelden met een aangepaste domeinnaam in de tenant na het toevoegen van en controle van de voor de nieuwe tenant. Zie voor meer informatie over het controleren van een aangepaste domeinnaam in een Azure Active Directory-tenant [een aangepaste domeinnaam toevoegen aan uw directory](/active-directory/active-directory-add-domain).

In dit voorbeeld bevat de "standaard Azure ' tenantmap alleen gebruikers met de naam van het domein '@alflanigan.onmicrosoft.com'.

Na het selecteren van het abonnement, de gebruiker met beheerdersrechten moet op **Access Control (IAM)** en vervolgens **een nieuwe rol toevoegen**.





![onderdeel voor toegangsbeheer IAM-functie in Azure-portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![nieuwe gebruiker toevoegen in het onderdeel voor toegangsbeheer IAM-functie in Azure-portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

De volgende stap is het selecteren van de functie moet worden toegewezen en de gebruiker waaraan de RBAC-rol wordt toegewezen aan. In de **rol** het vervolgmenu van de gebruiker met beheerdersrechten ziet alleen de ingebouwde rollen RBAC die beschikbaar in Azure zijn. Zie voor meer uitleg van elke rol en hun toewijsbare bereiken gedetailleerde, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer](role-based-access-built-in-roles.md).

De gebruiker met beheerdersrechten moet de e-mailadres van de externe gebruiker toevoegen. Het verwachte gedrag is voor de externe gebruiker worden niet weergegeven in de bestaande tenant. Nadat de externe gebruiker heeft uitgenodigd, hij zijn zichtbaar onder **abonnementen > Access Control (IAM)** met de huidige gebruikers dat momenteel een RBAC-rol op het bereik van het abonnement zijn toegewezen.





![Voeg machtigingen toe aan het nieuwe RBAC-rol](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![lijst met RBAC-rollen op abonnementsniveau](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

De gebruiker "chessercarlton@gmail.com' heeft uitgenodigd om deel te worden een **eigenaar** voor het abonnement 'Gratis'. De externe gebruiker ontvangt een e-mailbevestiging met een activeringskoppeling, na het verzenden van de uitnodiging.
![e-uitnodiging voor RBAC-rol](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)

Alleen buiten de organisatie, heeft de nieuwe gebruiker geen bestaande kenmerken in de tenantmap 'Standaard Azure'. Ze worden gemaakt nadat de externe gebruiker toestemming heeft gegeven moeten worden vastgelegd in de map die is gekoppeld aan het abonnement dat hij een rol is toegewezen.





![e-mailbericht uitnodiging voor RBAC-rol](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

De externe gebruiker wordt weergegeven in de Azure Active Directory-tenant op als de externe gebruiker en deze kunnen worden weergegeven in de Azure portal.





![gebruikers blade azure active directory Azure-portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)



In de **gebruikers** weergave voor de externe gebruikers kunnen worden herkend door het type ander pictogram in de Azure-portal.

Echter verlenen **eigenaar** of **Inzender** toegang tot een externe gebruiker op de **abonnement** bereik, staat niet toe dat de toegang tot de admin gebruikerslijst, tenzij de **globale beheerder** is toegestaan. In de eigenschappen van de gebruiker, de **gebruikerstype** die heeft twee algemene parameters, **lid** en **Gast** kunnen worden geïdentificeerd. Een lid is van een gebruiker die is geregistreerd in de map terwijl een gast een gebruiker uitgenodigd voor de map van een externe bron is. Zie voor meer informatie [hoe Azure Active Directory-beheerders Voeg B2B-samenwerking gebruikers](active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Zorg ervoor dat na het invoeren van de referenties in de portal, de externe gebruiker de juiste map aan te melden om te worden geselecteerd. Dezelfde gebruiker kan toegang hebben tot meerdere directory's en selecteer een van deze door te klikken op de gebruikersnaam in de rechterbovenhoek in de Azure portal en kies vervolgens de juiste map in de vervolgkeuzelijst.

Terwijl een gast in de directory, de externe gebruiker alle resources voor het Azure-abonnement kunt beheren, maar heeft geen toegang tot de map.





![toegang beperkt tot azure active directory-Azure-portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

Azure Active Directory en een Azure-abonnement niet hebben een relatie van de onderliggende structuur met bovenliggende net als andere Azure-resources (bijvoorbeeld: virtuele machines, virtuele netwerken, web-apps, opslag, enzovoort) met een Azure-abonnement hebt. Alle deze is gemaakt, beheerd en onder een Azure-abonnement in rekening gebracht terwijl een Azure-abonnement wordt gebruikt voor het beheren van de toegang tot een Azure-map. Zie voor meer informatie [hoe een Azure-abonnement is gerelateerd aan Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).

Van alle ingebouwde RBAC rollen, **eigenaar** en **Inzender** bieden volledige beheertoegang tot alle bronnen in de omgeving, het verschil is dat een medewerker kan geen maken en nieuwe RBAC-rollen te verwijderen. De ingebouwde rollen, zoals **Virtual Machine Contributor** bieden volledige management alleen toegang tot de resources die zijn aangegeven door de naam, ongeacht de **resourcegroep** ze worden gemaakt in.

Toewijzen van de ingebouwde RBAC-rol van **Virtual Machine Contributor** op abonnementsniveau, betekent dit dat de gebruiker de rol toegewezen:

* Kan alle virtuele machines ongeacht hun datum van de implementatie en weergeven ze deel van uitmaken resourcegroepen
* Volledig beheertoegang heeft tot de virtuele machines in het abonnement
* Andere brontypen weergeven in het abonnement niet
* Kan niet worden uitgevoerd vanuit het oogpunt van facturering van wijzigingen

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Een ingebouwde RBAC-rol toewijzen aan een externe gebruiker
Voor een ander scenario in deze test de externe gebruiker "alflanigan@gmail.com' wordt toegevoegd als een **Virtual Machine Contributor**.




![virtuele machine ingebouwde rol van Inzender](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

Het normale gedrag voor deze externe gebruiker met deze ingebouwde functie is om te zien en beheren van alleen virtuele machines en hun aangrenzende Resource Manager alleen bronnen nodig zijn tijdens de implementatie. Deze beperkte rollen bieden standaard alleen toegang tot hun bijbehorende resources in de Azure portal gemaakt.



![overzicht van virtuele machines Inzender rol in Azure-portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Toegang verlenen op het abonnementsniveau van een voor een gebruiker in dezelfde map
De processtroom identiek is aan een externe gebruiker toe te voegen, zowel vanuit het perspectief beheerder is de RBAC-rol, evenals de gebruiker verlenen hun toegang wordt verleend aan de rol. Het verschil is hier is dat uitgenodigde de gebruiker niet alle e-uitnodigingen ontvangt als de resource-scopes in het abonnement beschikbaar in het dashboard zijn na het aanmelden.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>RBAC-rollen op het groepsbereik resource toewijzen
Toewijzen van een RBAC-rol op een **resourcegroep** scope heeft een identieke proces voor het toewijzen van de rol op het abonnementsniveau voor beide typen gebruikers - externe of interne (onderdeel van dezelfde directory). De gebruikers die zijn toegewezen de RBAC-rol is om te zien in hun omgeving alleen de resourcegroep waarin ze zijn toegewezen toegang vanaf de **resourcegroepen** pictogram in de Azure-portal.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>RBAC-rollen op het bereik van de resource toe te wijzen
Toewijzen van een RBAC-rol op een scope resource in Azure heeft een identieke proces voor het toewijzen van de rol op het abonnementsniveau of op het niveau van de resourcegroep, dezelfde werkstroom voor beide scenario's te volgen. De gebruikers die de RBAC-rol is toegewezen Zie opnieuw kunnen alleen de items die ze toegang zijn toegewezen aan in de **alle Resources** tabblad of rechtstreeks in het dashboard.

Er is een belangrijk aspect voor RBAC zowel op resource groepsbereik of resource-bereik voor de gebruikers om ervoor te zorgen voor aanmelden bij de juiste map.





![Directory-aanmelding in Azure-portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>RBAC-rollen voor een Azure Active Directory-groep toewijzen
Alle scenario's die gebruikmaken van RBAC op de drie verschillende bereiken in Azure bieden de bevoegdheid beheren, implementeren en beheren van verschillende bronnen als een toegewezen gebruiker zonder de noodzaak om een persoonlijke abonnement te beheren. Ongeacht is de RBAC-rol toegewezen voor een abonnement, resourcegroep of resource-bereik, alle zijn onder de één Azure-abonnement waar gebruikers toegang tot hebben de resources verder gemaakt door de toegewezen gebruikers kosten in rekening gebracht. Op deze manier de gebruikers die beschikken over beheerdersrechten voor die volledige Azure-abonnement facturering heeft een volledig overzicht over het verbruik ongeacht wie de resources wordt beheerd.

Voor grotere organisaties, kunnen de RBAC-rollen worden toegepast op dezelfde manier voor het perspectief dat de gebruiker met beheerdersrechten toegang wil verlenen de gedetailleerde voor teams of volledige afdelingen, niet afzonderlijk voor elke gebruiker, overweegt het dus als een zeer tijd- en efficiënte optie overweegt Azure Active Directory-groepen. Ter illustratie van dit voorbeeld wordt de **Inzender** rol is toegevoegd aan een van de groepen in de tenant op het abonnementsniveau.





![RBAC-rol voor AAD-groepen toevoegen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

Deze groepen zijn beveiligingsgroepen die zijn ingericht en beheerd alleen binnen Azure Active Directory.

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-powershell"></a>Maakt een aangepaste RBAC-rol om te openen met behulp van PowerShell ondersteuningsaanvragen
De ingebouwde RBAC-rollen die beschikbaar in Azure zijn ervoor zorgen dat bepaalde machtigingsniveaus op basis van de beschikbare bronnen in de omgeving. Als geen van deze rollen behoeften van de gebruiker admin, is er echter de optie voor het beperken van toegang nog meer door aangepaste RBAC-rollen te maken.

Maken van aangepaste RBAC-rollen moet een ingebouwde functie, bewerken en vervolgens importeren in de omgeving. Het downloaden en het uploaden van de rol worden beheerd met PowerShell of CLI.

Het is belangrijk om te begrijpen van de vereisten voor het maken van een aangepaste rol die u kunnen gedetailleerde toegang verlenen op het abonnementsniveau en kan de uitgenodigde gebruiker de flexibiliteit ondersteuningsaanvragen te openen.

In dit voorbeeld de ingebouwde functie **lezer** waarmee gebruikers toegang om alle scopes van de resource weer te geven, maar niet te bewerken of maakt u nieuwe is aangepast om de gebruiker de optie ondersteuningsaanvragen te openen.

De eerste actie van het exporteren van de **lezer** rol moet worden uitgevoerd in PowerShell met verhoogde machtigingen worden uitgevoerd als administrator.

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Schermopname van PowerShell voor de rol Lezer RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

Vervolgens moet u het JSON-sjabloon van de rol extraheren.





![JSON-sjabloon voor aangepaste lezer RBAC-rol](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

Een typische RBAC-rol bestaat uit drie gedeelten **acties**, **NotActions** en **AssignableScopes**.

In de **actie** sectie worden de toegestane bewerkingen voor deze rol wordt vermeld. Het is belangrijk te weten dat elke actie van een resourceprovider is toegewezen. In dit geval voor het maken van ondersteuningstickets voor van de **Microsoft.Support** resourceprovider moet worden weergegeven.

Om te kunnen zien alle resourceproviders beschikbaar is en geregistreerd in uw abonnement, kunt u PowerShell gebruiken.
```
Get-AzureRMResourceProvider

```
Bovendien kunt u de alle de beschikbare PowerShell-cmdlets voor het beheren van de resourceproviders controleren.
    ![Schermopname van PowerShell voor provider bronbeheer](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)

Als u wilt beperken welke acties voor een bepaalde RBAC-rol, resourceproviders worden vermeld in de sectie **NotActions**.
Laatste, het verplicht is dat de RBAC-rol bevat de expliciete abonnement-id's waar het wordt gebruikt. De abonnement-id's worden vermeld in de **AssignableScopes**, anders u niet mag worden importeren van de rol in uw abonnement.

Na het maken en aanpassen van de RBAC-rol, moet deze worden geïmporteerd back de omgeving.

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

In dit voorbeeld is de aangepaste naam voor deze rol RBAC 'Lezer ondersteuning tickets toegangsniveau' zodat de gebruiker kan alles in het abonnement weergeven en openen van aanvragen voor ondersteuning.

> [!NOTE]
> De slechts twee ingebouwde RBAC-rollen waardoor de actie van het openen van ondersteuningsaanvragen zijn **eigenaar** en **Inzender**. Voor een gebruiker om te kunnen openen ondersteuningsaanvragen moet hij worden toegewezen een RBAC-rol alleen op het abonnementsbereik, omdat alle ondersteuningsaanvragen zijn gemaakt op basis van een Azure-abonnement.

Deze nieuwe aangepaste rol is toegewezen aan een gebruiker uit dezelfde map.





![schermopname van aangepaste RBAC-rol geïmporteerd in de Azure portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![schermopname van het aangepaste geïmporteerde RBAC-rol toewijzen aan gebruiker in dezelfde map](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![schermopname van machtigingen voor aangepaste geïmporteerde RBAC-rol](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

In het voorbeeld zijn meer gedetailleerd overzicht te benadrukken de grenzen van deze aangepaste RBAC-rol als volgt:
* Nieuwe ondersteuningsaanvragen kunt maken
* Nieuwe scopes van de resource kan niet worden gemaakt (bijvoorbeeld: virtuele machine)
* Kan de nieuwe resourcegroepen niet maken.





![schermopname van aangepaste RBAC-rol ondersteuningsaanvragen maken](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![schermopname van aangepaste RBAC-rol kan niet worden gemaakt van virtuele machines](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![schermopname van aangepaste RBAC-rol kan niet worden gemaakt van nieuwe RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-azure-cli"></a>Maakt een aangepaste RBAC-rol om te openen met behulp van Azure CLI ondersteuningsaanvragen
Azure CLI is uitgevoerd op een Mac en zonder dat toegang tot PowerShell, is de manier om te gaan.

De stappen voor het maken van een aangepaste beveiligingsrol zijn hetzelfde, met de enige uitzondering die met behulp van de rol CLI in een JSON-sjabloon kan niet worden gedownload, maar deze kan worden weergegeven in de CLI.

Ik heb de ingebouwde rol van de gekozen voor dit voorbeeld **back-up lezer**.

```

azure role show "backup reader" --json

```





![Schermafbeelding van de rol van back-lezer CLI weergeven](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

De rol in Visual Studio bewerken na het kopiëren van de eigenschappen in een JSON-sjabloon, het **Microsoft.Support** resourceprovider is toegevoegd aan de **acties** secties zodat deze gebruiker ondersteuningsaanvragen openen kunt terwijl u moet een lezer voor de back-upkluizen. Opnieuw is het nodig zijn om toe te voegen van de abonnements-ID waarop u deze rol wordt gebruikt in de **AssignableScopes** sectie.

```

azure role create --inputfile <path>

```





![CLI-schermopname van het importeren van aangepaste RBAC-rol](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

De nieuwe functie is nu beschikbaar in de Azure-portal en de toewijzing-proces is hetzelfde als in de eerdere voorbeelden.





![Azure portal schermafbeelding van aangepaste RBAC-rol gemaakt met behulp van de CLI 1.0](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

De Azure-Cloud-Shell is vanaf de meest recente Build-2017 algemeen beschikbaar. Azure Cloud-Shell vormt een aanvulling op de IDE- en de Azure-portal. Met deze service krijgt u een browser gebaseerde shell die is geverifieerd en wordt gehost in Azure en u kunt deze gebruiken in plaats van de CLI op uw computer geïnstalleerd.





![Azure Cloud Shell](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
