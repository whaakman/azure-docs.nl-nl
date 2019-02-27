---
title: Het oplossen van problemen met Azure Application Gateway-sessie-affiniteit
description: Dit artikel bevat informatie over het oplossen van problemen van de sessie-affiniteit in Azure Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: c98328342eec7fa59a56fbcc70da8cdd7a8fabf1
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56880883"
---
# <a name="troubleshoot-azure-application-gateway-session-affinity-issues"></a>Oplossen van problemen met Azure Application Gateway-sessie-affiniteit

Informatie over het diagnosticeren en oplossen van problemen van de sessie-affiniteit met Azure Application Gateway.

## <a name="overview"></a>Overzicht

De functie Sessieaffiniteit op basis van cookies is handig als u een gebruikerssessie op dezelfde server wilt behouden. Met behulp van de gatewaybeheerde cookies kan de Application Gateway het daarop volgende verkeer van een gebruikerssessie naar dezelfde server leiden voor verwerking. Dit is belangrijk wanneer de sessiestatus lokaal wordt opgeslagen op de server voor een gebruikerssessie.

## <a name="possible-problem-causes"></a>Mogelijk probleem oorzaken

Het probleem in het onderhouden van cookies gebaseerde sessieaffiniteit kan komen doordat de volgende belangrijke redenen:

- De instelling 'cookies gebaseerde affiniteit' is niet ingeschakeld
- Cookies gebaseerde affiniteit kan niet worden verwerkt door uw toepassing
- Toepassing gebruikt cookies gebaseerde affiniteit maar nog steeds venster tussen back-endservers aanvragen

### <a name="check-whether-the-cookie-based-affinity-setting-is-enabled"></a>Controleer of de instelling 'cookies gebaseerde affiniteit' is ingeschakeld

Soms optreden de sessie-affiniteit problemen wanneer u bent vergeten om in te schakelen van de instelling 'Cookies op basis van affiniteit'. Om te bepalen of u de instelling 'Cookies op basis van affiniteit' op het tabblad HTTP-instellingen in de Azure-portal hebt ingeschakeld, volgt u de instructies:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. In de **linkernavigatiebalk** deelvenster, klikt u op **alle resources**. Klik op de naam van de application gateway in de blade alle resources. Als het abonnement dat u geselecteerd, al verschillende resources heeft, kunt u de naam van de application gateway in de **filteren op naam...** voor eenvoudige toegang tot de toepassingsgateway.

3. Selecteer **HTTP-instellingen** tabblad onder **instellingen**.

   ![problemen oplossen-sessie-affiniteit-problemen met-1](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-1.png)

4. Klik op **appGatewayBackendHttpSettings** aan de rechterkant om te controleren of u hebt geselecteerd **ingeschakeld** voor op cookies gebaseerde affiniteit.

   ![troubleshoot-session-affinity-issues-2](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-2.jpg)



U kunt ook controleren met de waarde van de '**CookieBasedAffinity**' is ingesteld op *ingeschakeld*onder '**backendHttpSettingsCollection**"met behulp van een van de volgende methoden:

- Voer [Get-AzureRmApplicationGatewayBackendHttpSettings](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermapplicationgatewaybackendhttpsettings?view=azurermps-4.1.0) in PowerShell
- Zoek in het JSON-bestand met de Azure Resource Manager-sjabloon

```
"cookieBasedAffinity": "Enabled", 
```

### <a name="the-application-cannot-handle-cookie-based-affinity"></a>Cookies gebaseerde affiniteit kan niet worden verwerkt door de toepassing

#### <a name="cause"></a>Oorzaak

De application gateway kan alleen op basis van sessie-affiniteit uitvoeren met behulp van een cookie.

#### <a name="workaround"></a>Tijdelijke oplossing

Als de toepassing niet kan cookies gebaseerde affiniteit verwerken, moet u een externe of interne azure load balancer of een andere oplossing van derden gebruiken.

### <a name="application-is-using-cookie-based-affinity-but-requests-still-bouncing-between-back-end-servers"></a>Toepassing gebruikt cookies gebaseerde affiniteit maar nog steeds venster tussen back-endservers aanvragen

#### <a name="symptom"></a>Symptoom

U kunt de instelling van de cookies gebaseerde affiniteit hebt ingeschakeld, wanneer u toegang de toepassingsgateway tot met behulp van een korte naam-URL in Internet Explorer, bijvoorbeeld: [ http://website ](http://website/) , de aanvraag is nog steeds venster tussen back-endservers.

Volg de instructies voor het identificeren van dit probleem:

1. Een foutopsporingsprogramma web tracering uitvoeren op de 'Client' die verbinding met de toepassing achter de Gateway(We are using Fiddler in this example) toepassing maakt.
    **Tip** als u niet hoe u de Fiddler gebruikt weet, controleert u de optie '**ik wil netwerkverkeer verzamelen en analyseren met behulp van web foutopsporingsprogramma**' aan de onderkant.

2. Controleren en analyseren van de sessielogboeken, om te bepalen of de cookies die is opgegeven door de client de details van de ARRAffinity hebben. Als u de details van ARRAffinity, zoals niet vinden "**ARRAffinity =** *ARRAffinityValue*' binnen de cookie is ingesteld, die aangeeft dat de client is niet met behulp van de cookie SH beantwoorden die wordt geleverd door de Application Gateway.
    Bijvoorbeeld:

    ![problemen oplossen-sessie-affiniteit-problemen-3](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-3.png)

        ![troubleshoot-session-affinity-issues-4](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-4.png)

U blijft de toepassing probeert in te stellen van de cookie bij elke aanvraag, totdat het antwoord.

#### <a name="cause"></a>Oorzaak

Dit probleem treedt op omdat Internet Explorer en andere browsers mogelijk niet opslaan in of de cookie wordt gebruikt met een URL voor een korte naam.

#### <a name="resolution"></a>Oplossing

U kunt dit probleem oplossen, moet u toegang tot de toepassingsgateway met behulp van een FQDN-naam. Gebruik bijvoorbeeld [ http://website.com ](http://website.com/) of [ http://appgw.website.com ](http://appgw.website.com/) .

## <a name="additional-logs-to-troubleshoot"></a>Extra Logboeken om op te lossen

U kunt extra logboeken te verzamelen en analyseren ze voor het oplossen van de problemen verwante cookies gebaseerde sessieaffiniteit

### <a name="analyze-application-gateway-logs"></a>Application Gateway-logboekbestanden analyseren

Volg de instructies voor het verzamelen van de Application Gateway-Logboeken:

Logboekregistratie inschakelen via de Azure-portal

1. In de [Azure-portal](https://portal.azure.com/), vindt uw resource en klik vervolgens op **diagnostische logboeken**.

   Voor Application Gateway zijn de drie logboeken beschikbaar: Toegang tot logboek, prestatielogboek, Firewall-logboek

2. Klik op te starten om gegevens te verzamelen, **diagnostische gegevens inschakelen**.

   ![troubleshoot-session-affinity-issues-5](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-5.png)

3. De **diagnostische instellingen** blade bevat de instellingen voor de diagnostische logboeken. In dit voorbeeld worden de logboeken met Log Analytics opgeslagen. Klik op **configureren** onder **Log Analytics** om in te stellen van uw werkruimte. U kunt ook Event Hubs en een opslagaccount gebruiken om de diagnostische logboeken op te slaan.

   ![problemen oplossen-sessie-affiniteit-problemen-6](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-6.png)

4. Bevestig de instellingen en klik vervolgens op **opslaan**.

   ![troubleshoot-session-affinity-issues-7](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-7.png)

#### <a name="view-and-analyze-the-application-gateway-access-logs"></a>De toegang tot Application Gateway logboeken bekijken en analyseren

1. Selecteer in de Azure-portal onder de weergave van de resource Application Gateway, **diagnoselogboeken** in de **bewaking** sectie.

   ![troubleshoot-session-affinity-issues-8](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-8.png)

2. Selecteer aan de rechterkant '**ApplicationGatewayAccessLog**' in de vervolgkeuzelijst onder **categorieën aanmelden.**  

   ![troubleshoot-session-affinity-issues-9](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-9.png)

3. In de lijst Toegangslogboek Application-Gateway op het logboek dat u wilt analyseren en te exporteren en vervolgens de JSON-bestand exporteren.

4. Het JSON-bestand dat u hebt geëxporteerd in stap 3 naar CSV-bestand te converteren en ze weergeven in Excel, Power BI of een ander hulpmiddel voor gegevensvisualisatie.

5. Controleer de volgende gegevens:

- **Client-IP**: dit is het client-IP-adres van de client die verbinding maakt.
- **Enterpriseclient** -dit is de bronpoort van de client die verbinding maakt voor de aanvraag.
- **RequestQuery** – dit geeft aan dat de doelserver dat de aanvraag is ontvangen.
- **Server-Routed**: Back-end-pool-exemplaar dat de aanvraag is ontvangen.
- **X-AzureApplicationGateway-LOG-ID**: Correlatie-ID die wordt gebruikt voor de aanvraag. Het kan worden gebruikt voor het oplossen van problemen met verkeer op de back-endservers. Bijvoorbeeld: X-AzureApplicationGateway-CACHE-hits = 0 & SERVER doorgestuurd = 10.0.2.4.

  - **SERVER-STATUS**: HTTP-responscode die Application Gateway van de back-end ontvangen.

  ![troubleshoot-session-affinity-issues-11](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-11.png)

Als u twee items afkomstig zijn uit dezelfde client-IP en poort van de Client, en ze worden verzonden naar dezelfde back-end server ziet, betekent dit dat de Application Gateway correct geconfigureerd.

Als u twee items afkomstig zijn uit dezelfde client-IP en poort van de Client, en ze worden verzonden naar de andere back-endservers ziet, betekent dit dat de aanvraag is venster tussen back-endservers, selecteer "**toepassing gebruik maakt van cookies gebaseerde affiniteit maar aanvragen nog steeds venster tussen back-endservers**'onder aan het oplossen van dit probleem.

### <a name="use-web-debugger-to-capture-and-analyze-the-http-or-https-traffics"></a>Web foutopsporingsprogramma kunt vastleggen en analyseren van de HTTP of HTTPS-verkeer

Web-foutopsporingsprogramma's, zoals Fiddler, kunt u fouten opsporen in web-apps door het vastleggen van netwerkverkeer tussen Internet en de test. Deze hulpprogramma's kunnen u inkomende en uitgaande gegevens controleren zoals de browser ontvangt/verzendt deze. Fiddler, heeft in dit voorbeeld de HTTP-replay-optie waarmee u client-side-problemen met web-apps, met name voor verificatie van het type van het probleem op te lossen.

Gebruik het foutopsporingsprogramma web van uw keuze. In dit voorbeeld gebruiken we Fiddler vastleggen en analyseren van http of https-verkeer, volg de instructies:

1. Download het hulpprogramma Fiddler op <https://www.telerik.com/download/fiddler>.

    > [!NOTE]
    > Kies Fiddler4 als de computer vastleggen .NET 4 zijn geïnstalleerd heeft. Kies anders Fiddler2.

2. Klik met de rechtermuisknop op het uitvoerbare bestand van de installatie en uitvoeren als beheerder om te installeren.

            ![troubleshoot-session-affinity-issues-12](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-12.png)

3. Wanneer u Fiddler opent, moet het automatisch starten vastleggen van verkeer (Let op het vastleggen op kleine links-corner). Druk op F12 te starten of stoppen van verkeer vastleggen.

        ![troubleshoot-session-affinity-issues-13](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-13.png)

4. Waarschijnlijk, kunt u geïnteresseerd in het versleutelde HTTPS-verkeer, en kunt u decodering van HTTPS inschakelen door het selecteren van **extra** > **Fiddler opties**, en schakel het selectievakje ' **ontsleutelen HTTPS-verkeer**'.

        ![troubleshoot-session-affinity-issues-14](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-14.png)

5. U kunt vorige niet-gerelateerde sessies voordat u het probleem te reproduceren door te klikken op verwijderen **X** (pictogram) > **Alles verwijderen** als volgt schermafbeelding: 

        ![troubleshoot-session-affinity-issues-15](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-15.png)

6. Nadat u het probleem hebt gereproduceerd, sla van het bestand voor revisie door het selecteren van **bestand** > **opslaan** > **alle sessies...** . 

        ![troubleshoot-session-affinity-issues-16](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-16.png)

7. Controleren en analyseren van de sessielogboeken om te bepalen wat het probleem is.

    Voor voorbeelden:

- **Voorbeeld A:** U vindt een sessielogboek die de aanvraag wordt verzonden vanaf de client, en deze naar het openbare IP-adres van de Application Gateway, klikt u op dit logboek om de details weer te geven.  Aan de rechterkant is gegevens in het vak onder wat de Application Gateway naar de client worden geretourneerd. Selecteer het tabblad 'RAW' en bepalen of de client ontvangt een '**Set-Cookie: ARRAffinity =** *ARRAffinityValue*. " Als er geen cookie, sessie-affiniteit is niet ingesteld of de Application Gateway cookie terug naar de client wordt niet toegepast.

   > [!NOTE]
   > Deze waarde ARRAffinity is de cookie-id, die de Application Gateway wordt ingesteld voor de client worden verzonden naar een specifieke back-end-server.

    ![troubleshoot-session-affinity-issues-17](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-17.png)

- **Voorbeeld B:** De volgende sessielogboek gevolgd door de vorige is de client reageert terug naar de Application Gateway, die de ARRAAFFINITY heeft ingesteld. Als de ARRAffinity cookie-id overeenkomt met, moet het pakket worden verzonden naar dezelfde back-end-server die eerder is gebruikt. Controleer de volgende verschillende regels http-communicatie mogelijk is om te zien of van de client ARRAffinity cookie wordt gewijzigd.

    ![troubleshoot-session-affinity-issues-18](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-18.png)

> [!NOTE]
> Voor de dezelfde sessie van de communicatie moet de cookie niet als u wilt wijzigen. Schakel het selectievakje boven aan de rechterkant, selecteert u "Cookies" tabblad om te zien of de client met behulp van de cookie is en dat deze wordt teruggestuurd naar de Application Gateway. Zo niet, in de browser van de client is niet te houden en het gebruik van de cookie voor conversaties. Soms kan de client zich bevinden.

 

## <a name="next-steps"></a>Volgende stappen

Als de voorgaande stappen het probleem niet verhelpen, opent u een [ondersteuningsticket](https://azure.microsoft.com/support/options/).