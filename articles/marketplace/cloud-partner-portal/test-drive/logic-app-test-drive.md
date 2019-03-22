---
title: Logic App uitproberen | Microsoft Docs
description: Wordt uitgelegd hoe hun testrit die verbinding met een Dynamics AX/CRM-exemplaar of een andere bron dan alleen Azure maakt.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 4fd946b53956509844ad0a9396575f1ee2450414
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58338595"
---
<a name="logic-app-test-drive"></a>Logische App Test Drive
====================

Dit artikel is voor uitgevers die hun aanbieding op AppSource en hun testrit die verbinding met een Dynamics AX/CRM-exemplaar of een andere bron dan alleen Azure maakt bouwen.

<a name="how-to-build-a-logic-app-test-drive"></a>Over het bouwen van een logische App Test Drive
-----------------------------------

Test Drive-documentatie voor logische App Test Drives is momenteel nog steeds op GitHub voor [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) en [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app), gaat u er meer informatie.

<a name="how-to-publish-a-test-drive"></a>Het publiceren van een Test Drive
---------------------------

Nu dat u uw Test Drive gebouwd hebt, wordt dit gedeelte doorloopt samen met elk van de velden die vereist zijn voor u is uw Test Drive publiceren.

![De Test Drive-functie inschakelen](./media/azure-resource-manager-test-drive/howtopub1.png)

Het eerste en belangrijkste veld is om te bepalen of u wilt testen van het formulier in met alle vereiste velden worden weergegeven voor u om in te vullen. Wanneer u selecteert **Nee,** het formulier wordt uitgeschakeld en als u opnieuw met de Test Drive uitgeschakeld publiceren, uw Test Drive wordt verwijderd uit de productie.

*Opmerking*: Als er een Test Drives actief gebruikt door gebruikers, blijft deze Test Drives actief totdat de sessie is verlopen.

### <a name="details"></a>Details

De volgende sectie om in te vullen is dat de informatie over uw Test Drive biedt.

![Stuurprogrammagegevens testen](./media/azure-resource-manager-test-drive/howtopub2.png)

**Beschrijving -** *[vereist veld]* dit is waar u de beschrijving van de belangrijkste schrijven over wat er op uw Test Drive. De klant komt hier om te lezen welke scenario's voor uw Test Drive die betrekking over het product hebben wordt. 

**Gebruikershandleiding -** *[vereist veld]* dit is de uitgebreide overzicht van uw Test Drive-ervaring. De klant dit wordt geopend en precies wat u wilt laten doen tijdens de proefrit kunt doorlopen. Het is belangrijk dat deze inhoud eenvoudig is te begrijpen en te volgen. (Dit moet een PDF-bestand zijn)

**Test Drive demovideo -** \[aanbevolen\] is vergelijkbaar met de handmatige gebruiker, het is raadzaam om op te nemen van een video met zelfstudie van uw Test Drive-ervaring. De klant dit eerdere of tijdens de Test Drive wordt gecontroleerd en precies wat u wilt laten doen tijdens de proefrit kunt doorlopen. Het is belangrijk dat deze inhoud eenvoudig is te begrijpen en te volgen.

- **Naam** -titel van uw Video
- **Koppeling** -moet een ingesloten URL van YouTube of Vimeo. Voorbeeld voor het ophalen van de ingesloten url vindt u hieronder:
- **Miniatuur** -moet een afbeelding van hoge kwaliteit (533 x 324) pixels zijn. Het verdient aanbeveling om hier een schermafbeelding van een deel van uw Test Drive-ervaring te.

Hieronder ziet u hoe deze velden weergegeven voor uw klant tijdens de Test Drive-ervaring.

![Test Drive velden uiterlijk](./media/azure-resource-manager-test-drive/howtopub4.png)

### <a name="technical-configuration"></a>Technische configuratie

De volgende sectie om in te vullen is waar u uw logische App testen station configureren en definiëren hoe specifiek werk uw Test Drive-exemplaren.

![Technische Testconfiguratie station](./media/azure-resource-manager-test-drive/howtopub5_logicapp.png)

- **Regio** - *[vereist veld]* de regio die u selecteert is waar u kiezen waar uw Test Drive logische App-resources worden geïmplementeerd.

    *Opmerking:* Als uw logische App een aangepaste resources die zijn opgeslagen in een regio, moet u dat die regio hier is geselecteerd. De beste manier om dit te doen is om te **volledig uw logische App lokaal op uw Azure-abonnement in de portal implementeren en controleren of deze werkt** voordat deze zich hier worden geschreven.

- **Maximum aantal gelijktijdige Test Drives** - *[vereist veld]* nummer van Test Drive-exemplaren die al zijn geïmplementeerd en wacht op toegang tot per geselecteerde regio. Klanten kunnen onmiddellijk toegang krijgen tot deze Test Drives in plaats van dat moet worden gewacht voor een implementatie.

    *Opmerking:* Als u een webinar/klasse waar u al uw N aantal studenten uitvoert om een Test uitvoeren, is het raadzaam om te publiceren met N-aantal van Hot-exemplaren en vervolgens één keer de klasse is meer dan opnieuw te publiceren naar uw normale aantal Hot-exemplaren.

- **Test Drive duur (uren) -** *[vereist veld]* duur voor hoe lang de Test Drive blijft actief is, in \# uur. De Test Drive wordt automatisch beëindigd na afloop van deze periode.

- **Naam van de Azure-resourcegroep -** *[vereist veld]* schrijven in de naam van de resourcegroep waar uw logische App Test Drives zijn opgeslagen.

- **Toewijzen van logische App-naam -** *[vereist veld]* schrijven in de logische App die wordt gebruikt voor het toewijzen van een gebruiker in de Test Drive voordat de klant deze krijgt de naam van deze logische App schrijven. Zorg ervoor dat dit bestand wordt opgeslagen in de bovenstaande resourcegroep.

- **Naam van de logische App - inrichting** *[vereist veld]* schrijven in de logische App-naam voor het opheffen van inrichting van alle resources die in de Test Drive gemaakt. Zorg ervoor dat dit bestand wordt opgeslagen in de bovenstaande resourcegroep.

- **Toegang tot informatie -** *[vereist veld]* nadat de Test Drive wordt door een klant, de toegang tot informatie weergegeven. Deze instructies zijn bedoeld voor het delen van de parameters die handig zijn in uw Test Drive Resource Manager-sjabloon. Gebruiken om op te nemen output-parameters, dubbele gekrulde haken (bijvoorbeeld **{{outputname}}**), en ze correct zal worden ingevoegd in de locatie. (HTML-indeling van tekenreeks wordt aanbevolen hier moeten worden weergegeven in de front-end).

### <a name="test-drive-deployment-subscription-details"></a>Abonnementsgegevens van Test Drive-implementatie

Het laatste gedeelte om in te vullen is om de Test Drives automatisch implementeren door verbinding te maken van uw Azure-abonnement en Azure Active Directory (AD) te kunnen.

![Abonnementsgegevens station implementatie testen](./media/azure-resource-manager-test-drive/subdetails1.png)

**Azure-abonnements-ID** *[vereist veld]* dit verleent toegang tot Azure-services en de Azure-portal. Het abonnement is waarbij Resourcegebruik wordt gerapporteerd en services worden in rekening gebracht. Als u nog geen een **afzonderlijke** Azure-abonnement voor de Test Drives alleen, gaat u verder en er een maken. U kunt Azure abonnement-id's vinden door te melden bij Azure portal en te navigeren naar de abonnementen in het menu aan de linkerkant.
(Voorbeeld: "a83645ac-1234-5ab6-6789-1h234g764ghty")

![Azure-abonnementen](./media/azure-resource-manager-test-drive/subdetails2.png)

**Azure AD-Tenant-ID** *[vereist veld]* hebt u een Tenant-ID al beschikbaar u deze hieronder in de eigenschappen vindt -\> Directory-ID.

![Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails3.png)

Anders maakt u een nieuwe Tenant in Azure Active Directory.

![Scherm van de eigenschappen van Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails4.png)

! Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails5.png)

![Azure Active Directory-tenants](./media/azure-resource-manager-test-drive/subdetails6.png)

**Azure AD-App-ID** *[vereist veld]* volgende stap is het maken en een nieuwe toepassing registreren. We gebruiken deze toepassing bewerkingen op uw Test Drive-instantie uit te voeren.

1. Ga naar de zojuist gemaakte map of al bestaande directory en Azure Active directory selecteren in het filterdeelvenster.
2. Zoeken naar 'App-registraties' en klik op 'Toevoegen'
3. Geef een toepassingsnaam in.
4. Selecteer het Type als ' Web-app / API '
5. Geef een waarde in de aanmeldings-URL, we hebben gewonnen\'t worden met behulp van dat veld.
6. Klik op maken.
7. Nadat de toepassing is gemaakt, gaat u naar eigenschappen -\> instellen van de toepassing als meerdere tenants en klik op opslaan.

Klik op Opslaan. De laatste stap is om te halen van de toepassings-ID voor deze geregistreerde app en plak deze in het veld van Test Drive hier.

![Azure Active Directory-toepassings-id](./media/azure-resource-manager-test-drive/subdetails7.png)

Krijgen we de toepassing gebruiken om te implementeren voor het abonnement, moet de toepassing toevoegen als Inzender van het abonnement. De instructies voor deze zijn als hieronder:

1. Navigeer naar de blade abonnementen en selecteer het juiste abonnement dat u voor de Test Drive gebruikt.
1. Klik op **toegangsbeheer (IAM)**.
1. Klik op de **roltoewijzingen** tabblad.  ![Azure Active Directory, een nieuwe Access Control-principal toevoegen](./media/azure-resource-manager-test-drive/SetupSub7_1.jpg)
1. Klik op **roltoewijzing toevoegen**.
1. Instellen van de rol als **Inzender**.
1. Typ de naam van de Azure AD-toepassing en selecteer de toepassing aan de rol toe te wijzen.
    ![Azure Active Directory-machtigingen](./media/azure-resource-manager-test-drive/SetupSub7_2.jpg)
1. Klik op **Opslaan**.

**Azure AD App Key -** *[vereist veld]* het laatste veld voor het genereren van een verificatiesleutel nodig is. Onder sleutels, een beschrijving van de sleutel, toevoegen de duur instellen op nooit verlopen, klikt u vervolgens selecteert u opslaan. Het is **belangrijk** om te voorkomen dat een verlopen sleutels, die wordt verbroken uw test drive in productie. Deze waarde kopiëren en plak deze in uw vereist Test Drive-veld.

![Sectie van de Azure Active Directory-sleutels](./media/azure-resource-manager-test-drive/subdetails8.png)

> [!CAUTION]
> U kunt de Preview van Azure App-registratie niet gebruiken omdat een sleutel met base64 gecodeerde momenteel genereert.


<a name="next-steps"></a>Volgende stappen
----------

Nu dat u al uw Test Drive-velden ingevuld hebt, doorlopen en **opnieuw publiceren** uw aanbieding. Wanneer uw Test Drive het certificeringsproces verstreken is, gaat u een uitgebreid te testen de klantervaring in de **preview** van uw aanbieding. Start een Test uit in de gebruikersinterface en controleer of dat uw Test Drives zijn wordt volledig correct is geïmplementeerd.

Het is belangrijk te weten een deel van de Test Drive niet te verwijderen als deze zijn ingericht voor uw klanten, zodat de Test Drive-service automatisch deze brongroepen opschonen wordt nadat een klant klaar is.

Als u vertrouwd met de Preview-aanbieding, het is nu tijd om **live gaan**! Er is een proces uiteindelijke beoordeling van Microsoft zodra de aanbieding gepubliceerd naar dubbele selectievakje de volledige end-to-end-ervaring is. Als voor een of andere reden de aanbieding wordt geweigerd, wordt er een melding verzonden naar de technische contactpersoon voor uw aanbod waarin wordt uitgelegd wat moet worden opgelost.

Als u meer vragen hebt, zoekt advies voor probleemoplossing of wilt u uw Test Drive meer successen te behalen, gaat u naar [Veelgestelde vragen, probleemoplossing en aanbevolen procedures](./marketing-and-best-practices.md).
