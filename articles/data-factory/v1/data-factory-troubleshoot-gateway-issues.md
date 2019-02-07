---
title: Problemen met Data Management Gateway | Microsoft Docs
description: Tips voor het oplossen van problemen met betrekking tot Data Management Gateway biedt.
services: data-factory
author: nabhishek
manager: craigg
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/01/2017
ms.author: abnarain
robots: noindex
ms.openlocfilehash: dcbf011d6e5f035a1934b69f94cf95b2318491f0
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813837"
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a>Problemen oplossen met behulp van Data Management Gateway
In dit artikel bevat informatie over het oplossen van problemen bij het gebruik van Data Management Gateway.

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [zelf-hostende integratieruntime in Gegevensfactory](../create-self-hosted-integration-runtime.md).

Zie de [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor meer informatie over de gateway. Zie de [gegevens verplaatsen tussen on-premises en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor een overzicht van de gegevens uit een on-premises SQL Server-database verplaatsen naar Microsoft Azure Blob-opslag met behulp van de gateway.

## <a name="failed-to-install-or-register-gateway"></a>Installatie mislukt of gateway registreren
### <a name="1-problem"></a>1. Probleem
Ziet u dit foutbericht ontvangt bij het installeren en registreren van een gateway, met name tijdens het downloaden van het installatiebestand van de gateway.

`Unable to connect to the remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a>Oorzaak
De machine waarop u probeert te installeren van de gateway is mislukt voor het downloaden van het recentste installatiebestand van de gateway van het download center door een netwerkprobleem.

#### <a name="resolution"></a>Oplossing
Controleer de instellingen van uw firewall-proxy om te zien of de instellingen voor de netwerkverbinding van de computer naar blokkeren de [Downloadcentrum](https://download.microsoft.com/), en de instellingen dienovereenkomstig.

U kunt ook het installatiebestand voor de meest recente gateway downloaden de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=39717) op andere computers die toegang hebben tot het Downloadcentrum. U kunt vervolgens het installer-bestand kopiëren naar de computer van de gateway-host en voert deze handmatig te installeren en bijwerken van de gateway.

### <a name="2-problem"></a>2. Probleem
U ziet deze fout wanneer u probeert een gateway te installeren door te klikken op **rechtstreeks op deze computer installeren** in Azure portal.

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a>Oorzaak
Een gateway is al geïnstalleerd op de machine.

#### <a name="resolution"></a>Oplossing
Verwijder de bestaande gateway op de machine en klikt u op de **rechtstreeks op deze computer installeren** opnieuw koppelen.

### <a name="3-problem"></a>3. Probleem
U ziet deze fout bij het registreren van een nieuwe gateway.

`Error: The gateway has encountered an error during registration.`

#### <a name="cause"></a>Oorzaak
U ziet dit bericht voor een van de volgende redenen:

* De indeling van de gateway sleutel is ongeldig.
* De gateway sleutel is ongeldig gemaakt.
* De gateway sleutel is opnieuw gegenereerd vanaf de portal.  

#### <a name="resolution"></a>Oplossing
Controleer of u de juiste gateway sleutel vanuit de portal gebruiken. Indien nodig, een sleutel opnieuw genereren en de sleutel gebruiken om de gateway te registreren.

### <a name="4-problem"></a>4. Probleem
U ziet het volgende foutbericht weergegeven wanneer u bezig bent met het registreren van een gateway.

`Error: The content or format of the gateway key "{gatewayKey}" is invalid, please go to azure portal to create one new gateway or regenerate the gateway key.`



![Inhoud of indeling van de sleutel is ongeldig](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a>Oorzaak
De inhoud of indeling van de invoer gateway sleutel is onjuist. Een van de redenen kan zijn dat u hebt alleen een gedeelte van de sleutel gekopieerd vanuit de portal of u een sleutel is ongeldig.

#### <a name="resolution"></a>Oplossing
Een gateway sleutel genereren in de portal en gebruik de knop kopiëren om te kopiëren van de gehele sleutel. Plak deze in dit venster om de gateway te registreren.

### <a name="5-problem"></a>5. Probleem
U ziet het volgende foutbericht weergegeven wanneer u bezig bent met het registreren van een gateway.

`Error: The gateway key is invalid or empty. Specify a valid gateway key from the portal.`

![Gateway sleutel is ongeldig of leeg zijn](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a>Oorzaak
De gateway sleutel is opnieuw gegenereerd of de gateway is verwijderd in Azure portal. Het kan ook gebeuren als de installatie van Data Management Gateway is niet de laatste.

#### <a name="resolution"></a>Oplossing
Controleer of de Data Management Gateway-installatie de meest recente versie is, kunt u de meest recente versie kunt vinden op de Microsoft [Downloadcentrum](https://go.microsoft.com/fwlink/p/?LinkId=271260).

Als setup is de huidige / nieuwste en gateway nog op de Portal bestaat, opnieuw genereren van de gateway sleutel in de Azure-portal en gebruik de knop kopiëren om te kopiëren van de gehele sleutel en plak deze in dit venster om de gateway te registreren. Anders maakt u de gateway opnieuw en begin opnieuw.

### <a name="6-problem"></a>6. Probleem
U ziet het volgende foutbericht weergegeven wanneer u bezig bent met het registreren van een gateway.

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with the status “Gateway key is invalid”`

![Gateway sleutel is ongeldig of leeg zijn](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a>Oorzaak
Deze fout kan optreden omdat de gateway is verwijderd of de gekoppelde gateway sleutel is opnieuw gegenereerd.

#### <a name="resolution"></a>Oplossing
Als de gateway is verwijderd, opnieuw de gateway vanuit de portal maken, klikt u op **registreren**, Kopieer de sleutel van de portal, plakt u deze en probeert om de gateway te registreren.

Als de gateway nog wel bestaat, maar de sleutel is opnieuw gegenereerd, moet u de nieuwe sleutel gebruiken om de gateway te registreren. Als u de sleutel niet hebt, moet u de sleutel opnieuw vanuit de portal opnieuw gegenereerd.

### <a name="7-problem"></a>7. Probleem
Wanneer u een gateway hebt geregistreerd, moet u mogelijk pad en wachtwoord invoeren voor een certificaat.

![Certificaat opgeven](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a>Oorzaak
De gateway is geregistreerd op andere computers voordat. Tijdens de registratie van een gateway is een versleutelingscertificaat dat gekoppeld aan de gateway. Het certificaat kan zelf die worden gegenereerd door de gateway of opgegeven door de gebruiker.  Dit certificaat wordt gebruikt voor het versleutelen van de referenties van het gegevensarchief (gekoppelde service).  

![Certificaat exporteren](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

Bij het herstellen van de gateway op een andere hostmachine, wordt u gevraagd de registratiewizard voor dit certificaat voor het ontsleutelen van referenties die eerder werden gecodeerd met dit certificaat.  De referenties kunnen niet worden ontsleuteld door de nieuwe gateway en latere kopie activiteit uitvoeringen die zijn gekoppeld aan deze nieuwe gateway mislukt zonder dat dit certificaat.  

#### <a name="resolution"></a>Oplossing
Als u het referentiecertificaat van het oorspronkelijke gateway-apparaat hebt geëxporteerd met behulp van de **exporteren** knop op de **instellingen** tabblad in Data Management Gateway Configuration Manager, gebruikt u het certificaat Hier volgen enkele.

U kunt deze fase niet overslaan als het herstellen van een gateway. Als het certificaat ontbreekt, moet u de gateway verwijderen uit de portal en maak opnieuw een nieuwe gateway.  Bovendien bijwerken alle gekoppelde services die zijn gerelateerd aan de gateway door hun referenties opnieuw invoeren.

### <a name="8-problem"></a>8. Probleem
U ziet het volgende foutbericht weergegeven.

`Error: The remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a>Oorzaak
Deze fout treedt op wanneer uw gateway zich in een omgeving waarvoor een HTTP-proxy voor toegang tot resources op Internet, of van uw proxyserver Verificatiewachtwoord is gewijzigd, maar deze wordt niet bijgewerkt in uw gateway.

#### <a name="resolution"></a>Oplossing
Volg de instructies in de sectie Proxy server overwegingen van dit artikel en proxy-instellingen configureren met Data Management Gateway Configuration Manager.

## <a name="gateway-is-online-with-limited-functionality"></a>Gateway is online met beperkte functionaliteit
### <a name="1-problem"></a>1. Probleem
U ziet de status van de gateway online met beperkte functionaliteit.

#### <a name="cause"></a>Oorzaak
U ziet de status van de gateway online met beperkte functionaliteit voor een van de volgende redenen:

* Gateway kan geen verbinding maken met de cloudservice via Azure Service Bus.
* Cloudservice kan geen verbinding maken met gateway via Service Bus.

Wanneer de gateway online met beperkte functionaliteit is, kunt u mogelijk niet de Kopieerwizard van Data Factory gebruiken om gegevenspijplijnen voor het kopiëren van gegevens naar of van on-premises gegevensopslagexemplaren te maken. Als tijdelijke oplossing, kunt u Data Factory-Editor in de portal, Visual Studio of Azure PowerShell.

#### <a name="resolution"></a>Oplossing
Oplossing voor dit probleem (online met beperkte functionaliteit) is gebaseerd op of de gateway kan geen verbinding met de cloudservice, of andersom. De volgende secties vindt u deze oplossingen.

### <a name="2-problem"></a>2. Probleem
Ziet u de volgende fout.

`Error: Gateway cannot connect to cloud service through service bus`

![Gateway kan geen verbinding maken met cloudservice](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a>Oorzaak
Gateway kan geen verbinding maken met de cloudservice via Service Bus.

#### <a name="resolution"></a>Oplossing
Volg deze stappen voor het ophalen van de gateway weer online te zetten:

1. IP-adres van regels voor uitgaand verkeer op de gatewaycomputer en de firewall van het bedrijf toestaan. U kunt IP-adressen vinden van het Windows-gebeurtenislogboek (ID == 401): Er is geprobeerd toegang tot een socket op een manier die is niet toegestaan volgens de toegangsmachtigingen XX. XX. XX. XX:9350.
* Proxy-instellingen configureren op de gateway. Zie de sectie Proxy server overwegingen voor meer informatie.
* Schakel uitgaande poort 5671 en 9350-9354 op zowel de Windows Firewall op de gatewaycomputer en de firewall van het bedrijf. Zie de poorten en firewall-gedeelte voor meer informatie. Deze stap is optioneel, maar we raden aan ter overweging voor prestaties.

### <a name="3-problem"></a>3. Probleem
Ziet u de volgende fout.

`Error: Cloud service cannot connect to gateway through service bus.`

#### <a name="cause"></a>Oorzaak
Een tijdelijke fout in verbinding met het netwerk.

#### <a name="resolution"></a>Oplossing
Volg deze stappen voor het ophalen van de gateway weer online te zetten:

1. Wacht enkele minuten, de verbinding wordt automatisch worden hersteld als de fout is verdwenen.
* Als de fout zich blijft voordoen, start u de gatewayservice.

## <a name="failed-to-author-linked-service"></a>Kan geen gekoppelde service maken
### <a name="problem"></a>Probleem
U kunt deze fout tegenkomen wanneer u probeert te Referentiebeheer in de portal gebruiken voor het invoeren van referenties voor een nieuwe gekoppelde service of bijwerken van de referenties voor een bestaande gekoppelde service.

`Error: The data store '<Server>/<Database>' cannot be reached. Check connection settings for the data source.`

Wanneer u deze fout ziet, wordt de instellingenpagina van Data Management Gateway Configuratiebeheer uitzien zoals in de volgende schermafbeelding.

![Database kan niet worden bereikt](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a>Oorzaak
Het SSL-certificaat is mogelijk verloren gaan op de gatewaycomputer. Computer met de gateway kan het certificaat op dat moment die wordt gebruikt voor SSL-versleuteling niet laden. U ziet ook een foutbericht weergegeven in het gebeurtenislogboek die vergelijkbaar is met het volgende bericht weergegeven.

 `Unable to get the gateway settings from cloud service. Check the gateway key and the network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a>Oplossing
Volg deze stappen om het probleem te verhelpen:

1. Start de Data Management Gateway Configuratiebeheer.
2. Schakel over naar het tabblad **Instellingen**.  
3. Klik op de **wijzigen** om te wijzigen van het SSL-certificaat.

   ![Certificaat-knop wijzigen](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. Selecteer een nieuw certificaat als het SSL-certificaat. U kunt een SSL-certificaat dat wordt gegenereerd door u of een organisatie.

   ![Certificaat opgeven](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a>Kopieerbewerking is mislukt
### <a name="problem"></a>Probleem
Mogelijk ziet u de volgende "UserErrorFailedToConnectToSqlserver" Fout na het instellen van een pijplijn in de portal.

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server`

#### <a name="cause"></a>Oorzaak
Dit kan gebeuren om verschillende redenen en risicobeperking varieert dienovereenkomstig.

#### <a name="resolution"></a>Oplossing
Uitgaande TCP-verbindingen toestaan via poort TCP/1433 op de client Data Management Gateway voordat u verbinding maakt met een SQL-database.

Als de doeldatabase een Azure SQL database is, controleert u de SQL Server firewall-instellingen voor Azure ook.

Zie de volgende sectie voor het testen van de verbinding met de on-premises-gegevensarchief.

## <a name="data-store-connection-or-driver-related-errors"></a>Gegevensverbinding store of fouten met betrekking tot stuurprogramma
Als u gegevens opslaat verbinding of fouten met betrekking tot stuurprogramma ziet, kunt u de volgende stappen:

1. Start de Data Management Gateway Configuration Manager op de gatewaycomputer.
2. Schakel over naar de **Diagnostics** tabblad.
3. In **testverbinding**, de waarden van de gateway-groep toevoegen.
4. Klik op **Test** om te zien als u verbinding met de on-premises gegevensbron van het gateway-apparaat maken kunt met behulp van de verbindingsgegevens en referenties. Als de testverbinding ook niet lukt nadat u een stuurprogramma hebt geïnstalleerd, start u de gateway opnieuw op om de meest recente wijziging op te halen.

![Testverbinding in tabblad Diagnostische gegevens](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a>Gateway-Logboeken
### <a name="send-gateway-logs-to-microsoft"></a>Gatewaylogboeken naar Microsoft verzenden
Wanneer u contact opneemt met Microsoft Support voor hulp bij het oplossen van dergelijke problemen, wordt u mogelijk gevraagd voor het delen van uw gatewaylogboeken. Met het uitbrengen van de gateway, kunt u logboeken met twee knop klikken in Data Management Gateway Configuration Manager vereist gateway delen.    

1. Schakel over naar de **Diagnostics** tabblad in Data Management Gateway Configuration Manager.

    ![Data Management Gateway Diagnostics-tabblad](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. Klik op **logboeken verzenden** om te zien van het volgende dialoogvenster.

    ![Verzenden van Data Management Gateway-Logboeken](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. (Optioneel) Klik op **logboeken bekijken** om te controleren in de event viewer registreert.
4. (Optioneel) Klik op **privacy** privacyverklaring van Microsoft web services.
5. Wanneer u tevreden bent met wat u wilt uploaden, klikt u op zijn **logboeken verzenden** daadwerkelijk de logboeken van de afgelopen zeven dagen naar Microsoft verzenden voor het oplossen van problemen. U ziet de status van de bewerking verzend logboeken zoals wordt weergegeven in de volgende schermafbeelding.

    ![Status van Data Management Gateway verzenden-Logboeken](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. Nadat de bewerking voltooid is, ziet u een dialoogvenster zoals wordt weergegeven in de volgende schermafbeelding.

    ![Status van Data Management Gateway verzenden-Logboeken](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. Sla de **rapport-ID** en delen met Microsoft Support. De lijst-ID wordt gebruikt voor het vinden van de gatewaylogboeken die u hebt geüpload voor het oplossen van problemen.  De lijst-ID wordt ook opgeslagen in de event viewer.  U kunt vinden door te kijken naar de gebeurtenis-ID "25" en controleer de datum en tijd.

    ![Verzenden van Data Management Gateway-logboeken rapport-ID](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Archief-gateway-logboeken op de hostcomputer gateway
Er zijn enkele scenario's waarbij u problemen met gateway hebt en u kunt gateway-Logboeken niet rechtstreeks delen:

* U handmatig de gateway installeren en registreren van de gateway.
* U probeert te registreren van de gateway met een opnieuw gegenereerd in Data Management Gateway Configuration Manager-sleutel.
* U probeert om Logboeken te verzenden en de gateway-host-service kan niet worden verbonden.

Voor deze scenario's kunt u gateway-Logboeken als een zip-bestand opslaan en delen wanneer u contact op met Microsoft ondersteuning. Bijvoorbeeld, als er een fout opgetreden tijdens het registreren van de gateway wordt weergegeven in de volgende schermafbeelding.   

![Data Management Gateway-Registratiefout](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

Klik op de **gatewaylogboeken archiveren** koppelen als u wilt archiveren en opslaan van Logboeken en vervolgens het zip-bestand delen met Microsoft ondersteuning.

![Data Management Gateway Archive-Logboeken](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a>Ga naar de gateway-Logboeken
U vindt gedetailleerde gateway-logboekgegevens in de Windows-gebeurtenislogboeken.

1. Start Windows **logboeken**.
2. Ga naar Logboeken in de **logboeken toepassingen en Services** > **Data Management Gateway** map.

 Als u problemen met problemen met de gateway, zoekt u op gebeurtenissen op foutniveau in de event viewer.

![Data Management Gateway-Logboeken in Logboeken](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
