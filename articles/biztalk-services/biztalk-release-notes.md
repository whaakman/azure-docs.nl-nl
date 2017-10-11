---
title: Release-opmerkingen voor Azure BizTalk Services | Microsoft Docs
description: Geeft een overzicht van de bekende problemen voor Azure BizTalk Services
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: f4906fdc-4cd9-4a57-a007-a88c2e51a18f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 18ed891a9bba2b4011d3492722a2366d96fb3c01
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="release-notes-for-azure-biztalk-services"></a>Releaseopmerkingen voor Azure BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

De releaseopmerkingen voor Microsoft Azure BizTalk Services bevatten de bekende problemen in deze release.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Wat is er nieuw in de update November van BizTalk Services
* Codering in rust kan worden ingeschakeld in de Portal van BizTalk Services. Zie [Schakel versleuteling in rust in BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Historie van updates
### <a name="october-update"></a>Update van oktober
* Organisatieaccounts worden ondersteund:  
  * **Scenario**: U een BizTalk Service-implementatie met behulp van een Microsoft-account geregistreerd (zoals user@live.com). In dit scenario kunnen alleen gebruikers van Microsoft-Account de BizTalk Service gebruikt de BizTalk Services-portal beheren. Een organisatie-account kan niet worden gebruikt.  
  * **Scenario**: U een BizTalk Service-implementatie met een organisatie-account in een Azure Active Directory geregistreerd (zoals user@fabrikam.com of user@contoso.com). In dit scenario kunnen alleen Azure Active Directory-gebruikers binnen dezelfde organisatie beheren van de BizTalk Service gebruikt de BizTalk Services-portal. Een Microsoft-account kan niet worden gebruikt.  
* Wanneer u een BizTalk Service in de klassieke Azure portal maakt, wordt u automatisch geregistreerd in de Portal van BizTalk Services.
  * **Scenario**: je je aanmelden bij de klassieke Azure portal een BizTalk Service maakt en selecteer vervolgens **beheren** voor de eerste keer. Als de BizTalk Services-portal wordt geopend, wordt de BizTalk Service automatisch wordt geregistreerd en gereed is voor uw implementaties.  
    Zie [registreren en bijwerken van een BizTalk Service-implementatie BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>14 augustus Update
* Overeenkomst en bridge ontkoppeling – Trading partner-overeenkomsten en bruggen worden nu in de Portal van BizTalk Services losgekoppeld. Nu u overeenkomsten en bruggen afzonderlijk en tijdens runtime bruggen worden omgezet in een overeenkomst op basis van de waarden in het EDI-bericht. Zie [overeenkomsten maken in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689908.aspx), [een EDI-brug met behulp van BizTalk Services Portal maken](https://msdn.microsoft.com/library/azure/dn793986.aspx), [een AS2 brug met behulp van BizTalk Services Portal maken](https://msdn.microsoft.com/library/azure/dn793993.aspx), en [ Hoe los bruggen overeenkomsten tijdens runtime](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* De optie voor het maken van sjablonen voor overeenkomsten is stopgezet.  
* U kunt nu verschillende scheidingsteken ingesteld voor elk schema opgeven voor de overeenkomst verzenden aan clientzijde. Deze configuratie is opgegeven bij instellingen van het protocol voor send aan clientzijde overeenkomst. Zie voor meer informatie [maken een X12 overeenkomst in de Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx) en [maken van een overeenkomst EDIFACT in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/dn606267.aspx). Twee nieuwe entiteiten worden ook toegevoegd aan de TPM OM API voor hetzelfde doel. Zie [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) en [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standaard XSD-constructs, met inbegrip van afgeleide typen worden nu ondersteund. Zie [Gebruik standaard XSD-constructs in uw maps](https://msdn.microsoft.com/library/azure/dn793987.aspx) en [gebruik afgeleide typen in de toewijzing van scenario's en voorbeelden](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 biedt ondersteuning voor nieuwe MIC voor het ondertekenen van berichten en nieuwe versleutelingsalgoritmen. Zie [een AS2-overeenkomst Maak in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
  ## <a name="know-issues"></a>Bekende problemen

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Verbindingsproblemen na BizTalk Services-Portal bijwerken
  Als u de BizTalk Services-Portal openen terwijl BizTalk Services is bijgewerkt om te draaien in wijzigingen aan de service hebt, kunt u verbindingsproblemen met de BizTalk Services-Portal geconfronteerd.  
  Als tijdelijke oplossing kan u de browser opnieuw starten, de browser-cache verwijderen of starten van de portal in de privé-modus.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Het artefact kan niet worden gevonden in Visual Studio IDE als u klikt op een fout of waarschuwing in een project BizTalk Services
Installeer Visual Studio 2012 Update 3 RC 1 als het probleem wilt oplossen.  

### <a name="custom-binding-project-reference"></a>Aangepaste binding project een verwijzing
Houd rekening met de volgende situaties met een BizTalk Services-project in Visual Studio-oplossing:  

* In dezelfde Visual Studio-oplossing is een BizTalk Services-project en een aangepaste binding-project. De BizTalk Service-project bevat een verwijzing naar dit projectbestand aangepaste binding.
* De BizTalk Service-project bevat een verwijzing naar een aangepaste/bindingsgedrag dll-bestand.

U Build' ' de oplossing in Visual Studio is. Vervolgens 'Opnieuw samenstellen' of de oplossing opschonen. Hierna is wanneer u opnieuw samenstellen of opnieuw schoon het volgende foutbericht weergegeven:  
  Kan bestand niet kopiëren <Path to DLL> naar 'bin\Debug\FileName.dll'. Het proces heeft geen toegang tot het bestand 'bin\Debug\FileName.dll' omdat deze wordt gebruikt door een ander proces.  

#### <a name="workaround"></a>Tijdelijke oplossing
* Als [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) is geïnstalleerd, hebt u de volgende twee opties:
  
  * Start Visual Studio opnieuw of
  * Start opnieuw op de oplossing. Voer alleen een Build op de oplossing.  
* Als [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) is geen Taakbeheer geïnstalleerd, opent, klikt u op de processen tabblad, klik op het proces MSBuild.exe en klik vervolgens op de knop proces beëindigen.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Routering naar BasicHttpRelay eindpunten wordt niet ondersteund bij bruggen en BizTalk Services-Portal als niet-afdrukbare tekens worden gepromoveerd tot HTTP-headers
Als u niet-afdrukbare tekens als onderdeel van de gepromoveerde eigenschappen voor berichten gebruikt, kunnen niet deze berichten worden doorgestuurd naar de relay-doelen die gebruikmaken van de binding BasicHttpRelay. De doorgegeven eigenschappen die zijn ook beschikbaar als onderdeel van het bijhouden van URL-codering voor blobs en niet gecodeerde voor bestemmingen.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN wordt asynchroon verzonden, zelfs als de verzenden asynchrone MDN-optie uitgeschakeld is
Houd rekening met dit scenario: als u selecteert de **verzenden van asynchrone MDN** selectievakje en geef een URL voor het verzenden van het async MDN aan, en schakelt u vervolgens de **verzenden van asynchrone MDN** selectievakje opnieuw de MDN wordt nog steeds verzonden naar de URL opgegeven zelfs als de optie voor het verzenden van asynchrone MDNs niet is geselecteerd.  
Als tijdelijke oplossing, moet u de opgegeven URL wissen voordat u het uitschakelen van de **verzenden van asynchrone MDN** selectievakje en vervolgens implementeert u de AS2-overeenkomst.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Tekens dan witruimte buiten een geldig interchange ertoe leiden dat een leeg bericht worden verzonden naar het eindpunt onderbreken
Als er spaties buiten een segment IEA, wordt de disassembler wordt dit beschouwd als einde van het huidige knooppunt en wordt gekeken naar de volgende reeks als een volgende bericht uit spaties bestaan. Omdat dit geen geldige interchange is, wellicht u merkt dat een geslaagde bericht is verzonden naar de bestemming route en een leeg bericht het stand-by-eindpunt wordt verzonden.  

### <a name="tracking-in-biztalk-services-portal"></a>BizTalk Services-Portal bijhouden
Bijhouden gebeurtenissen worden vastgelegd maximaal de EDI-berichtverwerking en eventuele correlatie. Als een bericht niet buiten de fase Protocol, kunnen bijhouden wordt weergegeven als geslaagd. In dit geval verwijzen naar de sectie logboek onder de **Details** kolom in **bijhouden** foutdetails.
De X12 ontvangen en verzenden van instellingen ([maken een X12 overeenkomst in de Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx)) bieden informatie over de Protocol-fase.  

### <a name="update-agreement"></a>Bijwerken van de overeenkomst
De BizTalk Services-Portal kunt u de kwalificatie van een identiteit wijzigen wanneer een overeenkomst is geconfigureerd. Dit kan resulteren in de eigenschappen van is inconsistent. Er is bijvoorbeeld een overeenkomst met ZZ:1234567 en ZZ:7654321 de kwalificatie. In de profielinstellingen van BizTalk Services-Portal wijzigt u ZZ:1234567 als 01:ChangedValue. Opent u de overeenkomst en 01:ChangedValue wordt weergegeven in plaats van ZZ:1234567.
De kwalificatie van een identiteit wijzigen, verwijderen van de overeenkomst, bijwerken **identiteiten** in het Partnerprofiel en het vervolgens opnieuw de overeenkomst.  

> AZURE. Dit gedrag waarschuwing invloed X12 en AS2.  
> 
> 

### <a name="as2-attachments"></a>AS2-bijlagen
Bijlagen voor berichten worden niet ondersteund in AS2 verzenden of ontvangen. In het bijzonder bijlagen achtergrond worden genegeerd en de berichttekst wordt verwerkt als een reguliere AS2-bericht.  

### <a name="resources-remembering-path"></a>Resources: Onthoud pad
Bij het toevoegen van **Resources**, het dialoogvenster kan niet meer weet het pad dat eerder is gebruikt voor het toevoegen van een resource. Om te onthouden is het pad eerder gebruikt, probeert de website van BizTalk Services-Portal aan toe te voegen **vertrouwde websites** in Internet Explorer.  

### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Als u de naam van de entiteit van een brug en sluit u het project zonder wijzigingen worden opgeslagen, resulteert opnieuw openen van de entiteit in een fout opgetreden
Houd rekening met een scenario in de volgende volgorde:  

* Een brug (bijvoorbeeld een XML-One-Way brug) toevoegen aan een BizTalk Service-project  
* Wijzig de brug door een waarde opgeeft voor de eigenschap Name van de entiteit. Hiermee wijzigt u de bijbehorende .bridgeconfig-bestand met de naam die u hebt opgegeven.  
* Sluit het bestand .bcs (op het tabblad in Visual Studio sluiten) zonder de wijzigingen worden opgeslagen.  
* Het bestand .bcs opnieuw openen vanuit Solution Explorer.  
  U ziet dat terwijl het bijbehorende .bridgeconfig-bestand de nieuwe naam die u hebt opgegeven heeft, de naam van de entiteit op het ontwerpoppervlak nog steeds de oude naam. Als u probeert te openen van de configuratie van de brug door te dubbelklikken op het onderdeel bridge, krijgt u de volgende fout:  
  `‘<old name>’ Entity’s associated file ‘<old name>.bridgeconfig’ does not exist`Om te voorkomen die wordt uitgevoerd in dit scenario, moet dat u de wijzigingen opslaan nadat u de naam van de entiteiten in een BizTalk Service-project.  
  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk Service-project bouwt is zelfs als een artefact is uitgesloten van een Visual Studio-project
Houd rekening met een scenario waarin u een artefact (bijvoorbeeld een XSD-bestand) toevoegen aan een BizTalk Service-project, die artefact opnemen in de configuratie van de brug (bijvoorbeeld door te geven deze als een aanvraag berichttype) en vervolgens uitsluiten van de Visual Studio-project. In dat geval moet krijgt het project bouwen niet een fout, zolang het verwijderde artefact beschikbaar op de schijf op dezelfde locatie is vanaf waar het is opgenomen in de Visual Studio-project.
  
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>De BizTalk Service-project controleert niet op schema beschikbaarheid tijdens het configureren van de bruggen
In een BizTalk Service-project als een schema dat wordt toegevoegd aan het project een ander schema importeert wordt de BizTalk Service-project niet gecontroleerd of het geïmporteerde schema is toegevoegd aan het project. Als u probeert om dergelijke project te bouwen, krijgt u niet alle fouten in de build.
  
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Het antwoordbericht voor een XML-Request-Reply brug is altijd van charset UTF-8
Voor deze release is de tekenset van het antwoordbericht van een XML-Request-Reply Bridge altijd ingesteld op UTF-8.
  
### <a name="user-defined-datatypes"></a>Gebruiker gedefinieerde gegevenstypen
De BizTalk Adapter Pack-adapters in de BizTalk Adapter Service-functie kunnen gebruikmaken van gebruiker gedefinieerde gegevenstypen voor bewerkingen van de adapter.
Wanneer u de gebruiker gedefinieerde gegevenstypen, de bestanden (.dll) kopiëren naar station: \Program Files\Microsoft BizTalk Adapter Service\BAServiceRuntime\bin\ of naar de globale Assembly-Cache (GAC) op de server die als host fungeert voor de BizTalk Adapter Service-service. Anders worden de volgende fout kan optreden op de client:  
```
<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
<faultcode>s:Client</faultcode>
<faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
<detail>
  <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
  </AFConnectRuntimeFault>
</detail>
</s:Fault>
```  
  
> [!IMPORTANT]
> Het is raadzaam GACUtil.exe gebruiken voor het installeren van een bestand in de Global Assembly Cache. GACUtil.exe beschreven hoe u kunt dit hulpprogramma en de opties voor Visual Studio vanaf de opdrachtregel gebruiken.  
> 
> 

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>De BizTalk Adapter Service-website opnieuw te starten
Installeren van de **BizTalk Adapter Service Runtime*** maakt de **BizTalk Adapter Service** website in IIS met de **BAService** toepassing. **BAService** relay binding toepassing intern gebruikt om uit te breiden het bereik van de lokale service-eindpunt naar de cloud. Voor een service die wordt gehost on-premises, wordt het bijbehorende relay-eindpunt wordt geregistreerd in de Service Bus alleen wanneer de on-premises service wordt gestart.  

Als u stoppen en starten van een toepassing, wordt de configuratie voor het automatisch starten van een toepassing wordt niet gehonoreerd. In dat geval wanneer **BAService** is gestopt, moet u altijd opnieuw opstarten de **BizTalk Adapter Service** website in plaats daarvan. Niet starten of stoppen de **BAService** toepassing.

### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Speciale tekens mag niet worden gebruikt voor de namen van-adres en de entiteit van LOB-onderdelen
U moet speciale tekens niet gebruiken voor adressen en entiteit namen van LOB-onderdelen. Als u doet dit, krijgt u een fout opgetreden tijdens de implementatie van de BizTalk Service-project. Voor bepaalde tekens zoals '%', de BizTalk Adapter Service-website moet wellicht in een status gestopt en moet u deze handmatig te starten.

### <a name="test-map-with-get-context-property"></a>Testen van de Map met de eigenschap Get-Context
Als een transformatie bevat een **contexteigenschap ophalen** kaart bewerking, **Test kaart** zal mislukken. Als tijdelijke oplossing, vervang de **contexteigenschap ophalen** kaart opnieuw met een tekenreeks wilt samenvoegen kaart bewerking met dummy-gegevens. Hiermee wordt het doelschema vullen en kunt dat u andere transformatie-functionaliteit testen.

### <a name="test-map-property-does-not-display"></a>De eigenschap toewijzen test weergegeven niet
De **Test kaart** eigenschappen niet weergeven in Visual Studio. Dit kan gebeuren als de **eigenschappen** venster en de **Solution Explorer** venster niet tegelijkertijd worden gekoppeld. U kunt dit oplossen door het koppelen het **eigenschappen** en de **Solution Explorer** windows.  

### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Datum/tijd opmaken vervolgkeuzelijst is niet beschikbaar
Wanneer een bewerking DateTime opnieuw indelen kaart is toegevoegd aan het ontwerpgebied en geconfigureerd, de vervolgkeuzelijst indeling kan worden lichter gekleurd weergegeven. Dit kan gebeuren als de computer weergeven is ingesteld **gemiddeld: 125%** of **groter – 150%**. Ingesteld op te lossen, de weergave op **kleiner: 100% (standaard)** met behulp van de volgende stappen uit:  

1. Open de **Configuratiescherm** en klik op **vormgeving en persoonlijke instellingen**.
2. Klik op **weergave**.
3. Klik op **kleiner: 100% (standaard)** en klik op **toepassen**.

De **indeling** vervolgkeuzelijst moet nu werkt zoals verwacht.

### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Dubbele overeenkomsten in de Portal van BizTalk Services
Houd rekening met het volgende scenario:

1. Maak een overeenkomst met de Trading Partner Management OM API.
2. Open de overeenkomst in de Portal van BizTalk Services in twee verschillende tabbladen.
3. Implementeer de overeenkomst van zowel de tabbladen.
4. Zowel de overeenkomsten ophalen als gevolg hiervan geïmplementeerd waardoor dubbele vermeldingen in de Portal van BizTalk Services

**Tijdelijke oplossing**. Open een van de dubbele overeenkomsten in de Portal van BizTalk Services en worden geïmplementeerd.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Bruggen gebruik geen bijgewerkte certificaat, zelfs nadat u een certificaat is bijgewerkt in het archief met artefacten
Houd rekening met de volgende scenario's:  

**Scenario 1: Vingerafdruk gebaseerde certificaten gebruiken voor het beveiligen van bericht overdracht van een brug naar een service-eindpunt**  
Neem bijvoorbeeld een scenario waarin het gebruik van certificaten op basis van een vingerafdruk in uw BizTalk Service-project. U het certificaat in de Portal van BizTalk Services met dezelfde naam maar een andere vingerafdruk bijwerken, maar de BizTalk Service-project niet dienovereenkomstig bijgewerkt. In een dergelijk scenario blijven de brug mogelijk de berichten niet verwerken omdat de gegevens van oudere certificaat mogelijk toch in de cache kanaal. Hierna mislukt de berichtverwerking.  

**Tijdelijke oplossing**: bijwerken van het certificaat in de BizTalk Service-project en implementeer het project opnieuw.  

**Scenario 2: Naam gebaseerde gedrag gebruiken voor het identificeren van certificaten voor het beveiligen van bericht overdracht van een brug naar een service-eindpunt**

Overweeg een scenario waarbij u gedrag op basis van een naam voor het identificeren van certificaten in uw BizTalk Service-project. Bijwerken van het certificaat in de BizTalk Services-Portal, maar de BizTalk Service-project niet dienovereenkomstig bijgewerkt. In een dergelijk scenario blijven de brug mogelijk de berichten niet verwerken omdat de gegevens van oudere certificaat mogelijk toch in de cache kanaal. Hierna mislukt de berichtverwerking.  

**Tijdelijke oplossing**: bijwerken van het certificaat in de BizTalk Service-project en implementeer het project opnieuw.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Bruggen blijven voor het verwerken van berichten, zelfs wanneer de SQL-database offline is
De BizTalk Services bruggen blijven voor het verwerken van berichten voor een tijdje, zelfs als de Microsoft Azure SQL Database (die de actieve gegevens, zoals geïmplementeerde artefacten en pijplijnen worden opgeslagen), offline is. Dit is omdat BizTalk Services maakt gebruik van de cache artefacten en bridge configuratie.
Als u niet dat de bruggen alle berichten te verwerken wilt wanneer de SQL-Database offline is, kunt u de BizTalk Services PowerShell-cmdlets gebruiken om te stoppen of onderbreken van de BizTalk Service. Zie [Azure BizTalk Service Management Sample](http://go.microsoft.com/fwlink/p/?LinkID=329019) voor de Windows PowerShell-cmdlets om taken te beheren.  

### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Lezen van het XML-bericht in het onderdeel van een brug aangepaste code bevat een extra BOM-teken
Overweeg een scenario waarin u wilt lezen van een XML-bericht in een brug aangepaste code. Als u de .NET API System.Text.Encoding.UTF8.GetString(bytes) een extra BOM-teken is opgenomen in de uitvoer aan het begin van het bericht. Dus als u niet dat de uitvoer extra BOM String wilt, moet u ```System.IO.StreamReader().ReadToEnd()```.

### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Verzenden van berichten naar een brug met WCF niet kan worden uitgebreid
Berichten die worden verzonden naar een brug met WCF niet kan worden uitgebreid. Als u een schaalbare client wilt, moet u in plaats daarvan HttpWebRequest gebruiken.

### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>UPGRADE: Token providerfout na de upgrade van BizTalk Services Preview voor algemene beschikbaarheid (GA)
Er is een EDI- of AS2-overeenkomst met actieve batches. Wanneer de BizTalk Service is bijgewerkt van Preview naar GA, treedt de volgende:

* Fout: De tokenprovider kan niet worden geleverd een beveiligingstoken. Tokenprovider heeft geretourneerd bericht: de externe naam kan niet worden omgezet.
* Batch-taken geannuleerd.

**Tijdelijke oplossing**: nadat de BizTalk Service voor algemene beschikbaarheid (GA) wordt bijgewerkt, wordt de overeenkomst opnieuw implementeren.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>UPGRADE: Werkset toont de oude bridge pictogrammen na de upgrade van de BizTalk Services SDK
Na de upgrade van een eerdere versie van de BizTalk Services SDK waarin het naamelement oude pictogrammen die de bruggen vertegenwoordigt, wordt de werkset blijft de oude pictogrammen voor de bruggen weer te geven. Als u een brug aan ontwerpoppervlak BizTalk Service-project toevoegt, toont het oppervlak het pictogram Nieuw.  

**Tijdelijke oplossing**. U kunt dit probleem omzeilen door het verwijderen van de bestanden .tbd onder <system drive>: \Users\<gebruiker > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>UPGRADE: Update van de BizTalk-Portal van Preview naar GA mogelijk weer een foutbericht dat aangeeft dat de EDI-mogelijkheid niet beschikbaar is
Als u bent aangemeld bij de Portal van BizTalk Services terwijl de BizTalk Services is bijgewerkt van Preview naar NH, krijgt u mogelijk de volgende fout in de portal:  

Deze mogelijkheid is niet beschikbaar als onderdeel van deze editie van Microsoft Azure BizTalk Services. Voor het gebruik van overschakelen deze mogelijkheden naar een juiste editie.  

**Resolutie**: afmelden van de portal, sluit en open de browser en meld u aan bij de portal.  

### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>UPGRADE: Nieuwe traceergegevens wordt niet weergegeven nadat de BizTalk Services is bijgewerkt naar NH
Stel een scenario waarin u een XML-bridge geïmplementeerd op Preview van BizTalk Services-abonnement hebben. U berichten verzenden naar de brug en de bijbehorende gegevens voor het bijhouden is beschikbaar op de BizTalk Services-Portal. Nu als de BizTalk Services-Portal en BizTalk Services runtime-bits zijn bijgewerkt naar GA en u een bericht naar hetzelfde bridge eindpunt eerder hebt geïmplementeerd verzenden, wordt de traceergegevens niet weergegeven voor berichten die na de upgrade is verzonden.  

### <a name="pipelines-versus-bridges"></a>Pijplijnen versus bruggen
In dit document worden de term 'pijplijnen' en 'bruggen' door elkaar gebruikt. Beide in wezen betekenis dezelfde, die zich bevindt, een bericht processing unit geïmplementeerd op BizTalk Services.  

### <a name="concepts"></a>Concepten
[BizTalk Services](https://msdn.microsoft.com/library/azure/hh689864.aspx)   

