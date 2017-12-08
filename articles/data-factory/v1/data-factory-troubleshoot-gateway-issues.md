---
title: Problemen met Data Management Gateway | Microsoft Docs
description: Tips voor het oplossen van problemen met Data Management Gateway biedt.
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: abnarain
robots: noindex
ms.openlocfilehash: b3b34921168661089946b5c5dd9e6d489880733b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a>Problemen oplossen met behulp van Data Management Gateway
In dit artikel bevat informatie over het oplossen van problemen met het gebruik van Data Management Gateway.

> [!NOTE]
> In dit artikel is van toepassing op versie 1 van Azure Data Factory (GA) is algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [zelf gehost integratie runtime in Gegevensfactory versie 2](../create-self-hosted-integration-runtime.md).

Zie de [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor meer informatie over de gateway. Zie de [gegevens verplaatsen tussen on-premises en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor een overzicht van de gegevens uit een on-premises SQL Server database verplaatsen naar Microsoft Azure Blob-opslag met behulp van de gateway.

## <a name="failed-to-install-or-register-gateway"></a>Installatie mislukt of gateway registreren
### <a name="1-problem"></a>1. Probleem
U ziet dit foutbericht werd weergegeven bij het installeren en registreren van een gateway in het bijzonder tijdens het downloaden van het installatiebestand van de gateway.

`Unable to connect to the remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a>Oorzaak
De computer waarop u probeert te installeren van de gateway is mislukt voor het downloaden van het meest recente installatiebestand voor de gateway van het Downloadcentrum door een netwerkprobleem.

#### <a name="resolution"></a>Oplossing
Controleer de instellingen van uw firewall proxy om te zien of de instellingen blokkeren met de netwerkverbinding van de computer naar de [Downloadcentrum](https://download.microsoft.com/), en werk de instellingen dienovereenkomstig.

U kunt ook downloaden van het installatiebestand voor de nieuwste gateway via de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=39717) op andere computers die toegang heeft tot het download center. U kunt het installer-bestand kopiëren naar de gateway-hostcomputer en deze handmatig te installeren en bijwerken van de gateway uitvoeren.

### <a name="2-problem"></a>2. Probleem
Deze fout te zien wanneer u probeert een gateway installeren door te klikken op **rechtstreeks op deze computer installeren** in de Azure portal.

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a>Oorzaak
Een gateway is al geïnstalleerd op de machine.

#### <a name="resolution"></a>Oplossing
Verwijder de bestaande gateway op de machine en klik op de **rechtstreeks op deze computer installeren** opnieuw koppelen.

### <a name="3-problem"></a>3. Probleem
U ziet deze fout bij het registreren van een nieuwe gateway.

`Error: The gateway has encountered an error during registration.`

#### <a name="cause"></a>Oorzaak
Dit bericht kan worden weergegeven voor een van de volgende redenen:

* De indeling van de gatewaycode is ongeldig.
* De gatewaycode is ongeldig gemaakt.
* De gatewaycode is opnieuw vanuit de portal is gegenereerd.  

#### <a name="resolution"></a>Oplossing
Controleer of u de juiste gatewaycode via de portal. Indien nodig, een sleutel opnieuw genereren en de sleutel gebruiken om de gateway te registreren.

### <a name="4-problem"></a>4. Probleem
U ziet het volgende foutbericht weergegeven wanneer u bij het registreren van een gateway.

`Error: The content or format of the gateway key "{gatewayKey}" is invalid, please go to azure portal to create one new gateway or regenerate the gateway key.`



![Inhoud of de indeling van de sleutel is ongeldig](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a>Oorzaak
De inhoud of de indeling van de invoer gatewaycode is onjuist. Een van de redenen kan zijn dat u slechts een deel van de sleutel hebt gekopieerd uit de portal of u een ongeldige sleutel.

#### <a name="resolution"></a>Oplossing
Genereren van een gatewaysleutel in de portal en gebruik de knop kopiëren om te kopiëren van de gehele sleutel. Plak deze in dit venster om de gateway te registreren.

### <a name="5-problem"></a>5. Probleem
U ziet het volgende foutbericht weergegeven wanneer u bij het registreren van een gateway.

`Error: The gateway key is invalid or empty. Specify a valid gateway key from the portal.`

![Gatewaycode is ongeldig of leeg](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a>Oorzaak
De gatewaycode is gegenereerd of de gateway is verwijderd in de Azure portal. Het kan ook gebeuren als de installatie van Data Management Gateway geen laatste is.

#### <a name="resolution"></a>Oplossing
Controleer als de installatie van Data Management Gateway de meest recente versie is, kunt u de meest recente versie vinden op de Microsoft [Downloadcentrum](https://go.microsoft.com/fwlink/p/?LinkId=271260).

Als setup is de huidige / nieuwste en gateway nog op de Portal bestaat, de gatewaysleutel in de Azure portal opnieuw genereren en gebruik de knop kopiëren om te kopiëren van de gehele sleutel en plak deze in dit venster om de gateway te registreren. Anders opnieuw maken van de gateway en begin opnieuw.

### <a name="6-problem"></a>6. Probleem
U ziet het volgende foutbericht weergegeven wanneer u bij het registreren van een gateway.

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with the status “Gateway key is invalid”`

![Gatewaycode is ongeldig of leeg](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a>Oorzaak
Deze fout kan optreden omdat de gateway is verwijderd of de verbonden gatewaycode is gegenereerd.

#### <a name="resolution"></a>Oplossing
Als de gateway is verwijderd, opnieuw de gateway vanuit de portal maken, klikt u op **registreren**, Kopieer de sleutel van de portal, plakt u deze en probeert om de gateway te registreren.

Als de gateway nog wel bestaat, maar de sleutel is gegenereerd, gebruikt u de nieuwe sleutel om de gateway te registreren. Als u de sleutel niet hebt, moet u de sleutel opnieuw vanuit de portal opnieuw genereren.

### <a name="7-problem"></a>7. Probleem
Wanneer u een gateway registreren wilt, moet u mogelijk pad en wachtwoord invoeren voor een certificaat.

![Certificaat opgeven](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a>Oorzaak
De gateway is geregistreerd op andere computers voordat. Tijdens de initiële registratie van een gateway is een versleutelingscertificaat gekoppeld aan de gateway. Het certificaat kan automatisch gegenereerd door de gateway of door de gebruiker opgegeven.  Dit certificaat wordt gebruikt om de referenties van het gegevensarchief (gekoppelde service) te versleutelen.  

![Certificaat exporteren](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

Bij het herstellen van de gateway op een andere hostmachine, wordt u gevraagd de registratiewizard voor dit certificaat voor het ontsleutelen van referenties die eerder werden gecodeerd met dit certificaat.  De referenties kunnen niet worden ontsleuteld door de nieuwe gateway en latere kopie activiteit uitvoeringen die zijn gekoppeld aan deze nieuwe gateway niet zonder dit certificaat.  

#### <a name="resolution"></a>Oplossing
Als u de referenties van het certificaat van de oorspronkelijke computer met de gateway hebt geëxporteerd met behulp van de **exporteren** knop op de **instellingen** tabblad in Data Management Gateway Configuration Manager, gebruikt u het certificaat Hier.

U kunt deze fase niet overslaan tijdens het herstellen van een gateway. Als het certificaat ontbreekt, moet u de gateway van de portal te verwijderen en opnieuw maken van een nieuwe gateway.  Daarnaast werkt u alle gekoppelde services die zijn gerelateerd aan de gateway door hun referenties opnieuw invoeren.

### <a name="8-problem"></a>8. Probleem
U ziet het volgende foutbericht weergegeven.

`Error: The remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a>Oorzaak
Deze fout treedt op wanneer uw gateway zich in een omgeving waarin een HTTP-proxy voor toegang tot internetbronnen, of het wachtwoord voor uw proxy-verificatie is gewijzigd, maar deze wordt niet dienovereenkomstig bijgewerkt in uw gateway.

#### <a name="resolution"></a>Oplossing
Volg de instructies in de [overwegingen voor Proxy server](#proxy-server-considerations) sectie van dit artikel en proxy-instellingen configureren met Data Management Gateway Configuration Manager.

## <a name="gateway-is-online-with-limited-functionality"></a>Gateway is online via beperkte functionaliteit
### <a name="1-problem"></a>1. Probleem
U ziet de status van de gateway online met beperkte functionaliteit.

#### <a name="cause"></a>Oorzaak
U ziet de status van de gateway online met beperkte functionaliteit voor een van de volgende redenen:

* Gateway kan geen verbinding maken met cloudservice via Azure Service Bus.
* Cloudservice kan geen verbinding met de gateway via Service Bus.

Wanneer de gateway online met beperkte functionaliteit is, kunt u mogelijk niet de Data Factory-Wizard kopiëren gebruiken om te maken van gegevenspijplijnen voor het kopiëren van gegevens naar of van on-premises gegevensopslagexemplaren. Als tijdelijke oplossing kunt u Data Factory-Editor in de portal, Visual Studio of Azure PowerShell.

#### <a name="resolution"></a>Oplossing
Oplossing voor dit probleem (online met beperkte functionaliteit) is gebaseerd op of de gateway kan geen verbinding met de cloudservice of de andere kant maken. De volgende secties bevatten deze oplossingen.

### <a name="2-problem"></a>2. Probleem
Ziet u de volgende fout.

`Error: Gateway cannot connect to cloud service through service bus`

![Gateway kan geen verbinding maken met cloudservice](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a>Oorzaak
Gateway kan geen verbinding maken met de cloudservice via Service Bus.

#### <a name="resolution"></a>Oplossing
Volg deze stappen om de gateway weer online:

1. IP-adres uitgaande regels op de gatewaycomputer en de firewall van het bedrijf toestaan. U kunt IP-adressen vinden via het Windows-gebeurtenislogboek (ID == 401): Er is een poging gedaan te krijgen tot een socket op een manier die volgens de toegangsmachtigingen XX is niet toegestaan. XX. XX. XX:9350.
* Proxy-instellingen configureren op de gateway. Zie de [overwegingen voor Proxy server](#proxy-server-considerations) sectie voor meer informatie.
* Schakel de uitgaande poort 5671 en 9350-9354 op zowel de Windows Firewall op de gatewaycomputer en de firewall van het bedrijf. Zie de [poorten en firewall](#ports-and-firewall) sectie voor meer informatie. Deze stap is optioneel, maar we raden aan voor prestaties overweging.

### <a name="3-problem"></a>3. Probleem
Ziet u de volgende fout.

`Error: Cloud service cannot connect to gateway through service bus.`

#### <a name="cause"></a>Oorzaak
Een tijdelijke fout in verbinding met het netwerk.

#### <a name="resolution"></a>Oplossing
Volg deze stappen om de gateway weer online:

1. Wacht een paar minuten, de verbindingen worden automatisch hersteld wanneer de fout is verwijderd.
* Als de fout zich blijft voordoen, start u de gateway-service opnieuw.

## <a name="failed-to-author-linked-service"></a>Kan geen gekoppelde service maken
### <a name="problem"></a>Probleem
U kunt deze fout tegenkomen wanneer u probeert te Referentiebeheer in de portal te gebruiken referenties voor een nieuwe gekoppelde service invoeren of bijwerken van referenties voor een bestaande gekoppelde service.

`Error: The data store '<Server>/<Database>' cannot be reached. Check connection settings for the data source.`

Wanneer u deze fout ziet, kan de instellingenpagina van Data Management Gateway Configuration Manager de volgende schermafbeelding eruitzien.

![Database kan niet worden bereikt](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a>Oorzaak
Het SSL-certificaat is mogelijk verloren gegaan op de gatewaycomputer. Computer met de gateway kan het certificaat op dat moment die wordt gebruikt voor SSL-versleuteling niet laden. U ziet ook een foutbericht weergegeven in het gebeurtenislogboek die vergelijkbaar is met het volgende bericht.

 `Unable to get the gateway settings from cloud service. Check the gateway key and the network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a>Oplossing
Volg deze stappen om het probleem te verhelpen:

1. Start de Data Management Gateway Configuration Manager.
2. Overschakelen naar de **instellingen** tabblad.  
3. Klik op de **wijzigen** om te wijzigen van het SSL-certificaat.

   ![De knop certificaat wijzigen](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. Selecteer een nieuw certificaat als het SSL-certificaat. U kunt een SSL-certificaat dat is gegenereerd door u of elke organisatie gebruiken.

   ![Certificaat opgeven](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a>Kopieeractiviteit mislukt
### <a name="problem"></a>Probleem
Ziet u mogelijk de volgende 'UserErrorFailedToConnectToSqlserver' fout na het instellen van een pijplijn in de portal.

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server`

#### <a name="cause"></a>Oorzaak
Dit kan gebeuren om verschillende redenen en risicobeperking varieert dienovereenkomstig.

#### <a name="resolution"></a>Oplossing
Toestaan van uitgaande TCP-verbindingen via poort TCP 1433/aan de clientzijde Data Management Gateway voordat u verbinding maakt met een SQL-database.

Als de doeldatabase een Azure SQL database is, controleert u de SQL Server firewall-instellingen voor Azure ook.

Zie de volgende sectie voor het testen van de verbinding met de on-premises gegevensopslag.

## <a name="data-store-connection-or-driver-related-errors"></a>Gegevens opslaan verbinding of stuurprogramma gerelateerde fouten
Als u gegevens opslaan verbinding of stuurprogramma gerelateerde fouten ziet, kunt u de volgende stappen:

1. Start de Data Management Gateway Configuration Manager op de gatewaycomputer.
2. Overschakelen naar de **Diagnostics** tabblad.
3. In **testverbinding**, voeg de waarden van de groep gateway toe.
4. Klik op **Test** om te zien als u verbinding met de lokale gegevensbron van het gateway-apparaat maken kunt met behulp van de verbindingsgegevens en referenties. Als de testverbinding ook niet lukt nadat u een stuurprogramma hebt geïnstalleerd, start u de gateway opnieuw op om de meest recente wijziging op te halen.

![Testverbinding in tabblad Diagnostische gegevens](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a>Gateway-Logboeken
### <a name="send-gateway-logs-to-microsoft"></a>Gateway-logboeken naar Microsoft verzenden
Wanneer u contact opneemt met Microsoft Support voor hulp bij het oplossen van problemen van de gateway, wordt u mogelijk gevraagd om te delen van uw gateway-Logboeken. Met het uitbrengen van de gateway, kunt u logboeken met twee knop klikken in Data Management Gateway Configuration Manager vereist gateway delen.    

1. Overschakelen naar de **Diagnostics** tabblad in Data Management Gateway Configuration Manager.

    ![Tabblad van Data Management Gateway diagnostische gegevens](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. Klik op **logboeken verzenden** om te zien van het volgende dialoogvenster.

    ![Data Management Gateway verzenden Logboeken](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. (Optioneel) Klik op **logboeken bekijken** te bekijken in de event viewer registreert.
4. (Optioneel) Klik op **privacy** om te controleren van de privacyverklaring van Microsoft web services.
5. Wanneer u tevreden bent met wat u gaat uploaden, klikt u op **logboeken verzenden** daadwerkelijk de logboeken van de afgelopen zeven dagen naar Microsoft te verzenden voor het oplossen van problemen. U ziet de status van de bewerking send logboeken zoals weergegeven in de volgende schermafbeelding.

    ![Data Management Gateway verzenden logboeken status](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. Nadat de bewerking voltooid is, ziet u een dialoogvenster, zoals wordt weergegeven in de volgende schermafbeelding.

    ![Data Management Gateway verzenden logboeken status](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. Sla de **rapport ID** en delen met Microsoft Support. De lijst-ID wordt gebruikt voor het vinden van de gateway-logboeken die u hebt geüpload voor het oplossen van problemen.  De lijst-ID wordt ook opgeslagen in de event viewer.  U kunt vinden door te kijken naar de gebeurtenis-ID '25', en controleer of de datum en tijd.

    ![Data Management Gateway verzenden Logboeken lijst-ID](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Archief gateway-logboeken op de hostmachine gateway
Er zijn enkele scenario's waarbij u problemen met de gateway hebt en u gateway-logboeken rechtstreeks niet delen:

* U handmatig installeren van de gateway en registreer de gateway.
* U probeert te registreren van de gateway met een opnieuw gegenereerde sleutel in Data Management Gateway Configuration Manager.
* U probeert om Logboeken te verzenden en de gateway-hostservice kan niet worden verbonden.

Voor deze scenario's, kunt u de gateway-Logboeken als een zip-bestand opslaan en delen wanneer u contact opneemt met Microsoft ondersteuning. Bijvoorbeeld, als er een fout opgetreden tijdens het registreren van de gateway als weergegeven in de volgende schermafbeelding.   

![Data Management Gateway-Registratiefout](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

Klik op de **gateway-logboeken archiveren** koppeling voor het archiveren en Logboeken opslaan en vervolgens het zip-bestand delen met Microsoft ondersteuning.

![Data Management Gateway archief Logboeken](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a>Naar de gateway-Logboeken
U vindt gedetailleerde gateway logboekgegevens in de Windows-gebeurtenislogboeken.

1. Start Windows **logboeken**.
2. Ga naar Logboeken in de **toepassings- en servicelogboeken** > **Data Management Gateway** map.

 Wanneer u bent het oplossen van problemen met de gateway, zoek naar niveau foutgebeurtenissen in de event viewer.

![Data Management Gateway wordt geregistreerd in Logboeken](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
