---
title: Verkeer naar apps met meerdere tenants, zoals App service WebApps met Azure Application Gateway - Portal beheren
description: Dit artikel bevat richtlijnen over het configureren van Azure App service WebApps als leden in de back-endpool op een bestaande of nieuwe application gateway.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 3/11/2019
ms.author: absha
ms.openlocfilehash: 4dae04c14f9132c54dcc0575ccb2841a4742a626
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58176205"
---
# <a name="configure-app-service-with-application-gateway"></a>App Service configureren met Application Gateway

Application gateway kunt u een Azure App service-Web-App of andere services voor meerdere tenants hebben als lid van de back-end-pool. 

In dit artikel leert u het volgende:

> [!div class="checklist"]
>
> - Maak een back-endadresgroep en een App Service aan toevoegen
> - HTTP-instellingen en aangepaste test maken met 'Hostnaam kiezen' switches ingeschakeld

## <a name="prerequisites"></a>Vereisten

- Application gateway: Als u een bestaande toepassingsgateway hebt, raadpleegt u het [een toepassingsgateway maken](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)
- Appservice: Als u een bestaande App-service niet hebt, raadpleegt u [documentatie over App service](https://docs.microsoft.com/azure/app-service/).

## <a name="add-app-service-as-backend-pool"></a>Appservice als de back-endpool toevoegen

1. Open de weergave van de configuratie van u application gateway in de Azure-portal.

2. Onder **back-endpools**, klikt u op **toevoegen** naar een nieuwe back endpool maken.

3. Geef een passende naam voor de back-endpool. 

4. Onder **doelen**, klik op de vervolgkeuzelijst en kies **App Services** als de optie.

5. Een vervolgkeuzelijst direct onder de **doelen** vervolgkeuzelijst wordt weergegeven waarin een lijst van uw App-Services. In deze vervolgkeuzelijst, kiest u de App-Service die u wilt toevoegen als lid van een back-end-groep en klik op toevoegen.

   ![Back-end van App service](./media/configure-web-app-portal/backendpool.png)

## <a name="create-http-settings-for-app-service"></a>HTTP-instellingen voor appservice maken

1. Onder **HTTP-instellingen**, klikt u op **toevoegen** om een nieuwe HTTP-instelling te maken.

2. Geef een naam op voor de HTTP-instelling en u kunt inschakelen of uitschakelen van Cookie-affiniteit op basis van aan de hand van uw behoeften.

3. Kies het protocol HTTP of HTTPS aan de hand van uw situatie. 

4. Schakel het selectievakje voor **gebruiken voor App Service** en deze wordt ingeschakeld de **maakt u een test met de hostnaam kiezen uit de back-endadres** en **Kies hostnaam van de back-endadres** opties. Deze optie wordt ook automatisch maken van een test met de switch die is ingeschakeld en deze te koppelen aan deze HTTP-instelling.

5. Klik op **OK** om de HTTP-instelling te maken.

   ![HTTP-setting1](./media/configure-web-app-portal/http-setting1.png)

   ![HTTP-setting2](./media/configure-web-app-portal/http-setting2.png)

## <a name="create-rule-to-tie-the-listener-backend-pool-and-http-setting"></a>Regel voor het koppelen van de Listener, de back-Endpool en de HTTP-instelling maken

1. Onder **regels**, klikt u op **Basic** om een nieuwe Basic regel te maken.

2. Geef een passende naam en selecteer de listener die van de inkomende aanvragen voor de App-service accepteren.

3. In de **back-endpool** vervolgkeuzelijst, kiest u de back-end pool u eerder hebt gemaakt.

4. In de **HTTP-instelling** vervolgkeuzelijst, kies het HTTP instelling die u hierboven hebt gemaakt.

5. Klik op **OK** deze regel op te slaan.

   ![Regel](./media/configure-web-app-portal/rule.png)

## <a name="restrict-access"></a>Toegang beperken

De web-apps geïmplementeerd in de volgende voorbeelden gebruiken openbare IP-adressen die kunnen worden benaderd rechtstreeks vanuit het Internet. Dit helpt bij het oplossen van problemen wanneer u een nieuwe functie zijn leren en probeer het nieuwe dingen. Maar als u van plan bent voor het implementeren van een functie in productie, moet u meer beperkingen toevoegen.

Een manier die u kunt toegang tot uw web-apps beperken, is met [statische IP-beperkingen van Azure App Service](../app-service/app-service-ip-restrictions.md). U kunt bijvoorbeeld de web-app beperken zodat deze alleen verkeer van de toepassingsgateway ontvangt. Gebruik de functie voor de IP-beperking van app service om de toepassingsgateway VIP als het enige-adres met toegang weer te geven.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de appservice en andere ondersteuning voor meerdere tenants met application gateway, [ondersteuning van de service met meerdere tenants met application gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

Als het antwoord van de App-service wordt omgeleid naar de appservice-URL, Zie hoe u [omleiding naar de appservice-URL probleem oplossen](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url).
