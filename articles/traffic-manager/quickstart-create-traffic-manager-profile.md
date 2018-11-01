---
title: 'Snelstart: een Traffic Manager-profiel maken voor toepassingen met hoge beschikbaarheid in de Azure-portal'
description: In dit snelstartartikel wordt beschreven hoe u een Traffic Manager-profiel maakt voor het bouwen van webtoepassingen met hoge beschikbaarheid.
services: traffic-manager
documentationcenter: ''
author: kumudd
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: traffic-manager
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2018
ms.author: kumud
ms.openlocfilehash: bf9e296d7edf5ea8f668299830c64aaf3c5f74e9
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140482"
---
# <a name="quickstart-create-a-traffic-manager-profile-for-a-highly-available-web-application"></a>Snelstart: een Traffic Manager-profiel maken voor webtoepassingen met hoge beschikbaarheid

In deze snelstart wordt beschreven hoe u een Traffic Manager-profiel maakt die hoge beschikbaarheid van uw webtoepassing biedt. 

Het scenario dat in deze snelstart wordt beschreven, beschrijft twee exemplaren van een webtoepassing die in verschillende Azure-regio's wordt uitgevoerd. Er wordt een Traffic Manager-profiel gemaakt op basis van [eindpuntprioriteit](traffic-manager-routing-methods.md#priority) waarmee gebruikersverkeer naar de primaire site wordt gestuurd waar de toepassing wordt uitgevoerd. Traffic Manager bewaakt continu de webtoepassing en biedt automatische failover naar de back-upsite als de primaire site niet beschikbaar is.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure 
Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="prerequisites"></a>Vereisten
Voor deze snelstart dient u twee exemplaren van een webtoepassing te implementeren die in verschillende Azure-regio's wordt uitgevoerd, namelijk *US - oost* en *Europa - west*. De twee webtoepassingen dienen als primair en back-upeindpunt voor Traffic Manager.

1. Selecteer linksboven in het scherm de optie **Een resource maken** > **Web** > **Web-app** > **Maken**.
2. Voer in **Web-app** de volgende gegevens in (of selecteer ze) en voer standaardinstellingen in waar ze niet zijn opgegeven:

     | Instelling         | Waarde     |
     | ---              | ---  |
     | Naam           | Voer een unieke naam voor de web-app in  |
     | Resourcegroep          | Selecteer **Nieuw** en typ *myResourceGroupTM1* |
     | App Service-plan/-locatie         | Selecteer **Nieuw**.  Voer in het App Service-plan *myAppServicePlanEastUS* in en selecteer **OK**. 
     |      Locatie  |   US - oost        |
    |||

3. Selecteer **Maken**.
4. Er wordt een standaardwebsite gemaakt als de web-app is geïmplementeerd.
5. Herhaal stap 1 tot en met 3 om een tweede website te maken in een andere Azure-regio met de volgende instellingen:

     | Instelling         | Waarde     |
     | ---              | ---  |
     | Naam           | Voer een unieke naam voor de web-app in  |
     | Resourcegroep          | Selecteer **Nieuw** en typ *myResourceGroupTM2* |
     | App Service-plan/-locatie         | Selecteer **Nieuw**.  Voer in het App Service-plan *myAppServicePlanWestEurope* in en selecteer **OK**. 
     |      Locatie  |   Europa -west      |
    |||


## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken
Maak een Traffic Manager-profiel waarmee gebruikersverkeer wordt doorgestuurd op basis van eindpuntprioriteit.

1. Selecteer linksboven in het scherm de optie **Een resource maken** > **Netwerken** > **Traffic Manager-profiel** > **Maken**.
2. Voer in  **Traffic Manager-profiel maken** de volgende gegevens in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **Maken**:
    
    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Naam                   | Deze naam moet uniek zijn binnen de zone trafficmanager.net. De naam wordt gebruikt voor de DNS-naam **trafficmanager.net**, die wordt gebruikt voor het openen van uw Traffic Manager-profiel.|
    | Routeringsmethode          | Selecteer de routeringsmethode **Prioriteit**.|
    | Abonnement            | Selecteer uw abonnement.|
    | Resourcegroep          | Selecteer **Bestaande** en selecteer vervolgens *myResourceGroupTM1*.|
    |Locatie |Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op het Traffic Manager-profiel dat wereldwijd wordt geïmplementeerd.|
    |||
    
    
   ![Een Traffic Manager-profiel maken](./media/quickstart-create-traffic-manager-profile/traffic-manager-profile.png)


## <a name="add-traffic-manager-endpoints"></a>Traffic Manager-eindpunten toevoegen

Voeg de website in *US - oost* toe als primair eindpunt om alle gebruikersverkeer te routeren. Voeg de website in *Europa - west* toe als back-upeindpunt. Als het primaire eindpunt niet beschikbaar is, wordt het verkeer automatisch naar het secundaire eindpunt gerouteerd.

1. Zoek in de zoekbalk van de portal de naam van het Traffic Manager-profiel dat u in de vorige sectie hebt gemaakt en selecteer het profiel in de weergegeven resultaten.
2. Klik in **Traffic Manager-profiel**, in de sectie **Instellingen**, op **Eindpunten** en vervolgens op **Toevoegen**.
3. Voer de volgende informatie in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**:

    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Type                    | Azure-eindpunt                                   |
    | Naam           | myPrimaryEndpoint                                        |
    | Doelbrontype           | App Service                          |
    | Doelbron          | **Kies een app-service** om de lijst met web-apps onder hetzelfde abonnement weer te geven. Kies in **Resource** de app-service die u als eerste eindpunt wilt toevoegen. |
    | Prioriteit               | Selecteer **1**. Dit zorgt ervoor dat alle verkeer naar dit eindpunt wordt geleid, indien het eindpunt in orde is.    |
    
4. Herhaal stap 2 en 3 voor het volgende Web Apps-eindpunt. Zorg ervoor dat dit eindpunt wordt toegevoegd met de waarde **2** als bijbehorende **Prioriteit**.
5.  Als beide eindpunten zijn toegevoegd, worden ze weergegeven in **Traffic Manager-profiel**, samen met de controlestatus **Online**.

    ![Traffic Manager-eindpunt toevoegen](./media/quickstart-create-traffic-manager-profile/add-traffic-manager-endpoint2.png)

## <a name="test-traffic-manager-profile"></a>Traffic Manager-profiel testen
In deze sectie bepaalt u eerst de domeinnaam van het Traffic Manager-profiel en bekijkt u vervolgens hoe Traffic Manager een failover uitvoert naar het secundaire eindpunt als het primaire eindpunt niet beschikbaar is.
### <a name="determine-the-dns-name"></a>DNS-naam bepalen
1.  Zoek in de zoekbalk van de portal de naam van het **Traffic Manager-profiel** dat u in de vorige sectie hebt gemaakt. Klik op het Traffic Manager-profiel in de resultaten die worden weergegeven.
2. Klik op **Overzicht**.
3. Het **Traffic Manager-profiel** geeft de DNS-naam weer van het Traffic Manager-profiel dat u zojuist hebt gemaakt.
  
   ![DNS-naam van Traffic Manager](./media/quickstart-create-traffic-manager-profile/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager in werking zien

1. Typ in een webbrowser de DNS-naam van het Traffic Manager-profiel om de standaardwebsite van uw web-app te bekijken. In dit snelstartscenario worden alle aanvragen gerouteerd naar het primaire eindpunt dat is ingesteld op **Priority 1**.

![Traffic Manager-profiel testen](./media/quickstart-create-traffic-manager-profile/traffic-manager-test.png)

2. Als u de failover van Traffic Manager in werking wilt zien, schakelt u de primaire site als volgt uit:
    1. Selecteer op de profielpagina van Traffic Manager **Instellingen**>**Eindpunten**>*MyPrimaryEndpoint*.
    2. Selecteer in *MyPrimaryEndpoint* de optie **Uitgeschakeld**. 
    3. De status van het primaire eindpunt *MyPrimaryEndpoint* wordt nu weergegeven als **Uitgeschakeld**.
3. Kopieer de DNS-naam van het Traffic Manager-profiel uit de vorige stap om de website in een webbrowser te kunnen bekijken. Als het primaire eindpunt is uitgeschakeld, wordt het gebruikersverkeer naar het secundaire eindpunt gerouteerd.

## <a name="clean-up-resources"></a>Resources opschonen
Verwijder de resourcegroepen, de webtoepassingen en alle gerelateerde resources als u ze niet meer nodig hebt. Selecteer hiertoe de resourcegroepen (*myResourceGroupTM1* en *myResourceGroupTM2*) en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen
In deze snelstart maakt u een Traffic Manager-profiel waarmee u gebruikersverkeer voor webtoepassingen van hoge beschikbaarheid kunt doorsturen. Voor meer informatie over het routeren van verkeer gaat u door naar de zelfstudies voor Traffic Manager.

> [!div class="nextstepaction"]
> [Traffic Manager tutorials](tutorial-traffic-manager-improve-website-response.md) (Traffic Manager-zelfstudies)






