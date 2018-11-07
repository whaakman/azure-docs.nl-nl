---
title: Opmerkingen bij de release voor Azure BizTalk Services | Microsoft Docs
description: Geeft een lijst van de bekende problemen voor Azure BizTalk Services
services: biztalk-services
documentationcenter: ''
author: msftman
manager: erikre
editor: ''
ms.assetid: f4906fdc-4cd9-4a57-a007-a88c2e51a18f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: f990aa086997d51e59df4285aabeccd31dcce822
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51253428"
---
# <a name="release-notes-for-azure-biztalk-services"></a>Opmerkingen bij de release voor Azure BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

De opmerkingen bij de release voor Microsoft Azure BizTalk Services bevatten de bekende problemen in deze release.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Wat is er nieuw in de update van November van BizTalk Services
* Versleuteling-at-Rest kan worden ingeschakeld in de BizTalk Services-Portal. Zie [inschakelen van versleuteling-at-Rest in BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Historie van updates
### <a name="october-update"></a>Update van oktober
* Organisatie-accounts worden ondersteund:  
  * **Scenario**: u hebt een BizTalk Service-implementatie met behulp van een Microsoft-account geregistreerd (zoals user@live.com). In dit scenario kunnen alleen gebruikers van Microsoft-Account de BizTalk Service met behulp van de BizTalk Services-portal beheren. Een organisatie-account kan niet worden gebruikt.  
  * **Scenario**: u hebt een BizTalk Service-implementatie met behulp van een organisatie-account in een Azure Active Directory geregistreerd (zoals user@fabrikam.com of user@contoso.com). In dit scenario beheren alleen Azure Active Directory-gebruikers binnen dezelfde organisatie de BizTalk Service met behulp van de BizTalk Services-portal. Een Microsoft-account kan niet worden gebruikt.  
* Wanneer u een BizTalk Service maakt, wordt u automatisch geregistreerd in de BizTalk Services-Portal.
  * **Scenario**: U zich aanmeldt bij Azure, een BizTalk Service maken en selecteer vervolgens **beheren** voor de eerste keer. Wanneer de BizTalk Services-portal wordt geopend, wordt de BizTalk Service automatisch wordt geregistreerd en gereed is voor uw implementaties.  
    Zie [registreren en bijwerken van een BizTalk Service-implementatie op de BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>14 augustus Update
* Overeenkomst en bridge ontkoppeling – Trading partner-overeenkomsten en bruggen worden nu in de BizTalk Services-Portal ontkoppeld. Nu u overeenkomsten en bruggen afzonderlijk en tijdens runtime bruggen worden omgezet naar een overeenkomst op basis van de waarden in de EDI-bericht. Zie [overeenkomsten maken in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689908.aspx), [een EDI-brug met behulp van BizTalk Services-Portal maken](https://msdn.microsoft.com/library/azure/dn793986.aspx), [maken van een AS2-brug met behulp van BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/dn793993.aspx), en [ Hoe kunnen bruggen overeenkomsten tijdens runtime oplossen?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* De optie voor het maken van sjablonen voor overeenkomsten wordt stopgezet.  
* U kunt nu verschillende scheidingsteken ingesteld voor elk schema opgeven voor de overeenkomst verzenden aan clientzijde. Deze configuratie is opgegeven onder protocolinstellingen voor verzenden aan clientzijde overeenkomst. Zie voor meer informatie, [maken een X12 overeenkomst in de Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx) en [een EDIFACT-overeenkomst Maak in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/dn606267.aspx). Twee nieuwe entiteiten worden ook toegevoegd aan de TPM-Operations Manager-API voor hetzelfde doel. Zie [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) en [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standard XSD-constructs, met inbegrip van afgeleide typen, worden nu ondersteund. Zie [Gebruik standaard XSD constructs kaarten](https://msdn.microsoft.com/library/azure/dn793987.aspx) en [gebruik afgeleide typen in de toewijzing van scenario's en voorbeelden](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 biedt ondersteuning voor nieuwe MIC-algoritmen voor het ondertekenen van berichten en nieuwe versleutelingsalgoritmen. Zie [een AS2-overeenkomst Maak in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
  ## <a name="know-issues"></a>Bekende problemen

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Problemen met de netwerkverbinding nadat BizTalk Services-Portal-Update
  Als u de BizTalk Services-Portal openen terwijl BizTalk Services is bijgewerkt om te draaien in wijzigingen aan de service hebt, kunt u problemen met de netwerkverbinding met de BizTalk Services-Portal kan tegenkomen.  
  Als tijdelijke oplossing, mag u de browser opnieuw starten, de browsercache verwijderen of starten van de portal in de privémodus.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE kan het artefact niet vinden als u klikt op een fout of waarschuwing in een project BizTalk Services
Installeer het Visual Studio 2012 Update 3 RC 1 om het probleem te verhelpen.  

### <a name="custom-binding-project-reference"></a>Referentie voor aangepaste binding-project
Houd rekening met de volgende situaties met een BizTalk Services-project in Visual Studio-oplossing:  

* In dezelfde Visual Studio-oplossing, moet u er een project BizTalk Services en een aangepaste binding-project is. De BizTalk Service-project bevat een verwijzing naar dit projectbestand aangepaste binding.
* De BizTalk Service-project bevat een verwijzing naar een aangepaste binding/gedrag dll-bestand.

U maken' ' de oplossing in Visual Studio is. Vervolgens kunt u 'Opnieuw samenstellen' of schoon de oplossing. Na die, wanneer u opnieuw samenstellen of opnieuw, schoon het volgende foutbericht weergegeven:  
  Kan bestand niet kopiëren <Path to DLL> naar 'bin\Debug\FileName.dll'. Het proces heeft geen toegang tot het bestand 'bin\Debug\FileName.dll' omdat deze wordt gebruikt door een ander proces.  

#### <a name="workaround"></a>Tijdelijke oplossing
* Als [Visual Studio 2012 Update 3](https://docs.microsoft.com/visualstudio/releasenotes/vs2012-update3-vs) is geïnstalleerd, hebt u de volgende twee opties:
  
  * Start Visual Studio opnieuw of
  * Start opnieuw op de oplossing. Voer alleen een Build in de oplossing.  
* Als [Visual Studio 2012 Update 3](https://docs.microsoft.com/visualstudio/releasenotes/vs2012-update3-vs) is niet geïnstalleerd, opent taak Manager, klikt u op de processen tabblad, klik op het proces MSBuild.exe en klik vervolgens op de knop proces beëindigen.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Routering naar BasicHttpRelay eindpunten wordt niet ondersteund voor bruggen en BizTalk Services-Portal als niet-afdrukbare tekens worden gepromoveerd tot HTTP-headers
Als u niet-afdrukbare tekens als onderdeel van de gepromoveerde eigenschappen voor berichten gebruiken, kunnen niet deze berichten worden doorgestuurd naar de relay-doelen die gebruikmaken van de binding BasicHttpRelay. De doorgegeven eigenschappen die zijn ook beschikbaar als onderdeel van tracking URL gecodeerd voor blobs en niet gecodeerde voor bestemmingen.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN wordt asynchroon verzonden, zelfs als de optie verzenden asynchrone MDN uitgeschakeld is
Houd rekening met dit scenario: als u de **asynchrone MDN te verzenden** selectievakje in en geef een URL voor het verzenden van de asynchrone MDN, en schakel de **asynchrone MDN te verzenden** selectievakje opnieuw de MDN is nog steeds verzonden naar de URL opgegeven zelfs als de optie voor het verzenden van asynchrone MDN's niet is geselecteerd.  
Als tijdelijke oplossing, moet u de opgegeven URL wissen voordat u het uitschakelen van de **asynchrone MDN te verzenden** selectievakje in en implementeer vervolgens de AS2-overeenkomst.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Uit witruimte bestaat dan een geldige uitwisseling ertoe leiden dat een leeg bericht moet worden verzonden naar de stand-by-eindpunt
Als er alleen uit spaties bestaan naast een segment IEA, wordt de disassembler wordt dit beschouwd als einde van de huidige uitwisseling en kijkt naar de volgende set alleen uit spaties bestaan als een volgende bericht. Omdat dit geen geldige uitwisseling is, merkt u wellicht dat een geslaagde bericht wordt verzonden naar de bestemming van de route en een leeg bericht wordt verzonden de stand-by-eindpunt.  

### <a name="tracking-in-biztalk-services-portal"></a>Bijhouden van BizTalk Services-Portal
Traceringsgebeurtenissen worden vastgelegd maximaal de EDI-berichtverwerking en een correlatie. Als een bericht buiten de fase Protocol is mislukt, wordt tracering als geslaagd weergegeven. In dit geval verwijzen naar de sectie logboek onder de **Details** kolom in **bijhouden** foutdetails.
De X12 ontvangen en verzenden van instellingen ([maken een X12 overeenkomst in de Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx)) bevatten informatie over de Protocol-fase.  

### <a name="update-agreement"></a>Overeenkomst bijwerken
De BizTalk Services-Portal kunt u de kwalificatie van een identiteit wijzigen wanneer een overeenkomst is geconfigureerd. Dit kan resulteren in de eigenschappen van is inconsistent. Er is bijvoorbeeld een overeenkomst met behulp van ZZ:1234567 en ZZ:7654321 de kwalificatie. In de profielinstellingen van BizTalk Services-Portal wijzigt u ZZ:1234567 01:ChangedValue worden. Opent u de overeenkomst en 01:ChangedValue wordt weergegeven in plaats van ZZ:1234567.
De kwalificatie van een identiteit wijzigen, verwijderen van de overeenkomst, bijwerken **identiteiten** in het Partnerprofiel en het vervolgens opnieuw de overeenkomst.  

> AZURE. Waarschuwing: dit probleem heeft gevolgen voor X12 en AS2.  
> 
> 

### <a name="as2-attachments"></a>AS2-bijlagen
Bijlagen voor AS2-berichten worden niet ondersteund in verzenden of ontvangen. Specifiek, bijlagen op de achtergrond worden genegeerd en de hoofdtekst van het bericht wordt verwerkt als een reguliere AS2-bericht.  

### <a name="resources-remembering-path"></a>Resources: Pad onthouden
Bij het toevoegen van **Resources**, het dialoogvenster kan niet meer weet het pad dat eerder is gebruikt voor het toevoegen van een resource. Probeer om te onthouden het pad eerder gebruikt, toe te voegen van de BizTalk Services-Portal-website voor **vertrouwde Sites** in Internet Explorer.  

### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Als u de naam van de entiteit van een brug en sluit u het project zonder wijzigingen worden opgeslagen, resulteert Open de entiteit opnieuw in een fout
U hebt een scenario in de volgende volgorde:  

* Een brug (bijvoorbeeld een XML-One-Way brug) toevoegen aan een BizTalk Service-project  
* Wijzig de naam van de brug door een waarde voor de eigenschap naam van de entiteit op te geven. Hiermee wijzigt u de bijbehorende .bridgeconfig-bestand met de naam die u hebt opgegeven.  
* Sluit het bestand .bcs (op het tabblad in Visual Studio sluiten) zonder de wijzigingen worden opgeslagen.  
* Open het bestand .bcs opnieuw vanuit de Solution Explorer.  
  U ziet dat tijdens het bijbehorende .bridgeconfig-bestand de nieuwe naam die u hebt opgegeven heeft, de naam van de entiteit op het ontwerpoppervlak nog steeds de oude naam. Als u probeert te openen van de brugconfiguratie door te dubbelklikken op het onderdeel bridge, krijgt u de volgende fout:  
  `‘<old name>’ Entity’s associated file ‘<old name>.bridgeconfig’ does not exist` Om te voorkomen die wordt uitgevoerd in dit scenario, zorg ervoor dat u wijzigingen opslaat, nadat u de naam van de entiteiten in een BizTalk Service-project.  
  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk Service-project met succes wordt gemaakt, zelfs als een artefact is uitgesloten van een Visual Studio-project
U hebt een scenario waarin u een artefact (bijvoorbeeld een XSD-bestand) toevoegen aan een BizTalk Service-project, die artefact opnemen in de configuratie van de brug (bijvoorbeeld door te geven deze als een berichttype aanvraag) en deze vervolgens wordt uitgesloten van het Visual Studio-project. In dat geval krijgt het bouwen van het project niet een fout, zolang de verwijderde artefact beschikbaar op de schijf op dezelfde locatie is vanaf waar het is opgenomen in de Visual Studio-project.
  
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>De BizTalk Service-project controleert niet op schema beschikbaarheid tijdens het configureren van de bruggen
In een BizTalk Service-project als een schema dat wordt toegevoegd aan het project een andere schema, importeert wordt de BizTalk Service-project niet gecontroleerd of het geïmporteerde schema wordt toegevoegd aan het project. Als u probeert om een dergelijke project te bouwen, krijgt u niet alle fouten in de build.
  
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Het antwoordbericht voor een XML-aanvraag / antwoord-brug is altijd van tekenset UTF-8
Voor deze release is de tekenset van het antwoordbericht uit een XML-aanvraag / antwoord-brug altijd ingesteld op UTF-8.
  
### <a name="user-defined-datatypes"></a>Gebruiker gedefinieerde gegevenstypen
De BizTalk Adapter Pack-adapters in de BizTalk Adapter Service-functie kunnen gebruikmaken van gebruiker gedefinieerde gegevenstypen voor bewerkingen van de adapter.
Wanneer u de gebruiker gedefinieerde gegevenstypen, de bestanden (.dll) kopiëren naar station: \Program Files\Microsoft BizTalk Adapter Service\BAServiceRuntime\bin\ of aan de Global Assembly Cache (GAC) op de server die als host fungeert voor de BizTalk Adapter Service-service. Anders kan er op de client de volgende fout optreden:  
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
> Het verdient aanbeveling GACUtil.exe gebruiken voor het installeren van een bestand in de Global Assembly Cache. GACUtil.exe wordt beschreven hoe u dit hulpprogramma en de opdrachtregelopties voor Visual Studio.  
> 
> 

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>De website van BizTalk Adapter Service opnieuw starten
Installeren van de **BizTalk Adapter Service Runtime*** maakt de **BizTalk Adapter Service** website in IIS met de **BAService** toepassing. **BAService** relay binding toepassing intern gebruikt om uit te breiden het bereik van on-premises service-eindpunt voor de cloud. Voor een service die wordt gehost on-premises, wordt het eindpunt van de bijbehorende relay wordt geregistreerd in de Service Bus alleen wanneer de on-premises service wordt gestart.  

Als u stoppen en starten van een toepassing, wordt de configuratie voor automatisch starten van een toepassing niet herkend. Wanneer **BAService** is gestopt, moet u altijd opnieuw starten de **BizTalk Adapter Service** website in plaats daarvan. Niet starten of stoppen de **BAService** toepassing.

### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Speciale tekens mag niet worden gebruikt voor het adres en de entiteit namen van LOB-onderdelen
U mag geen speciale tekens gebruiken voor adres en de entiteit namen van LOB-onderdelen. Als u dit doet, krijgt u een fout opgetreden tijdens het implementeren van de BizTalk Service-project. Voor bepaalde tekens zoals '%', de website van de BizTalk Adapter Service bijvoorbeeld worden verzonden in een status gestopt en moet u deze handmatig te starten.

### <a name="test-map-with-get-context-property"></a>Kaart met Get-contexteigenschap testen
Als een transformatie bevat een **contexteigenschap ophalen** kaart bewerking **Test kaart** mislukken. Als tijdelijke oplossing, vervangen de **contexteigenschap ophalen** kaart opnieuw met een samenvoegen kaart bewerking van de tekenreeks met tijdelijke gegevens. Dit wordt het doelschema vullen en kunt dat u andere transformatie-functionaliteit testen.

### <a name="test-map-property-does-not-display"></a>Test kaart de eigenschap wordt niet weergegeven.
De **Test kaart** eigenschappen niet weergeven in Visual Studio. Dit kan gebeuren als de **eigenschappen** venster en de **Solution Explorer** venster niet gelijktijdig worden gekoppeld. U kunt dit oplossen dokken de **eigenschappen** en de **Solution Explorer** windows.  

### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Vervolgkeuzelijst opnieuw opmaken van datum/tijd is niet beschikbaar
Wanneer een bewerking voor datum/tijd opnieuw opmaken van kaart is toegevoegd aan het ontwerpgebied en geconfigureerd, kan de vervolgkeuzelijst indeling worden grijs weergegeven. Dit kan gebeuren als de weergave van de computer is ingesteld **gemiddeld: 125%** of **groter – 150%**. Ingesteld op te lossen, de weergave op **kleiner: 100% (standaard)** met behulp van de volgende stappen uit:  

1. Open de **Configuratiescherm** en klikt u op **vormgeving en persoonlijke instellingen**.
2. Klik op **weergave**.
3. Klik op **kleiner: 100% (standaard)** en klikt u op **toepassen**.

De **indeling** vervolgkeuzelijst nu werkt zoals verwacht.

### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Dubbele overeenkomsten in de BizTalk Services-Portal
Houd rekening met het volgende scenario:

1. Maak een overeenkomst met behulp van de Trading Partner Management OM API.
2. Open de overeenkomst in de Portal van de BizTalk Services in twee verschillende tabbladen.
3. Implementeer de overeenkomst van zowel de tabbladen.
4. Zowel de overeenkomsten geïmplementeerd als gevolg hiervan leidt tot dubbele vermeldingen in de BizTalk Services-Portal

**Tijdelijke oplossing**. Open een van de dubbele overeenkomsten in de BizTalk Services-Portal en worden geïmplementeerd.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Bruggen geen bijgewerkte-certificaat gebruiken, zelfs nadat u een certificaat is bijgewerkt in de store artefact
Houd rekening met de volgende scenario's:  

**Scenario 1: Gebruik van certificaten op basis van de vingerafdruk voor het beveiligen van bericht overdracht van een brug naar een service-eindpunt**  
U hebt een scenario waarin u certificaten op basis van een vingerafdruk in uw BizTalk Service-project gebruiken. U het certificaat in de BizTalk Services-Portal met dezelfde naam maar met een andere vingerafdruk bijwerken, maar de BizTalk Service-project niet dienovereenkomstig bijgewerkt. In een dergelijk scenario blijven de brug mogelijk de berichten worden verwerkt omdat de gegevens van het oudere certificaat mogelijk nog steeds in de cache van het kanaal. Hierna berichtverwerking is mislukt.  

**Tijdelijke oplossing**: het certificaat in de BizTalk Service-project bijwerken en opnieuw implementeren van het project.  

**Scenario 2: Gedrag op basis van naam gebruiken om te identificeren van certificaten voor het beveiligen van bericht overdracht van een brug naar een service-eindpunt**

Houd rekening met een scenario waarbij u gedrag op basis van een naam voor het identificeren van certificaten in uw BizTalk Service-project. Bijwerken van het certificaat in de BizTalk Services-Portal, maar de BizTalk Service-project niet dienovereenkomstig bijgewerkt. In een dergelijk scenario blijven de brug mogelijk de berichten worden verwerkt omdat de gegevens van het oudere certificaat mogelijk nog steeds in de cache van het kanaal. Hierna berichtverwerking is mislukt.  

**Tijdelijke oplossing**: het certificaat in de BizTalk Service-project bijwerken en opnieuw implementeren van het project.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Bruggen blijven voor het verwerken van berichten, zelfs als de SQL-database offline is
De BizTalk Services-bruggen blijven voor het verwerken van berichten voor een tijdje, zelfs als de Microsoft Azure SQL-Database (die de actieve informatie, zoals geïmplementeerde artefacten en pijplijnen opgeslagen), offline is. Dit komt doordat de BizTalk Services maakt gebruik van de artefacten in de cache en de brugconfiguratie.
Als u niet dat de bruggen berichten verwerken wilt als de SQL-Database offline is, kunt u de BizTalk Services PowerShell-cmdlets gebruiken om te stoppen of onderbreken van de BizTalk Service. Zie [Azure BizTalk Service-Beheervoorbeeld](https://go.microsoft.com/fwlink/p/?LinkID=329019) voor de Windows PowerShell-cmdlets om taken te beheren.  

### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Het lezen van de XML-bericht in het onderdeel van een brug van aangepaste code bevat een extra stuklijst-teken
U hebt een scenario waarin u wilt lezen van een XML-bericht in een brug van aangepaste code. Als u de .NET API System.Text.Encoding.UTF8.GetString(bytes) een extra stuklijst teken is opgenomen in de uitvoer aan het begin van het bericht. Dus als u niet dat de uitvoer wilt naar de extra stuklijst teken bevatten, moet u ```System.IO.StreamReader().ReadToEnd()```.

### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Verzenden van berichten naar een brug met behulp van WCF schalen niet
Berichten die worden verzonden naar een brug met behulp van WCF schaalt niet. Als u een schaalbare client wilt, moet u in plaats daarvan HttpWebRequest gebruiken.

### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>UPGRADE: Token providerfout na de upgrade van de Preview-versie van BizTalk Services naar algemene beschikbaarheid (GA)
Er is een EDI- of AS2-overeenkomst met actieve batches. Wanneer de BizTalk Service is bijgewerkt van Preview-versie voor algemene beschikbaarheid, optreden de volgende:

* Fout: De tokenprovider kon niet leveren van een beveiligingstoken. Tokenprovider bericht geretourneerd: de externe naam kan niet worden omgezet.
* Batch-taken worden geannuleerd.

**Tijdelijke oplossing**: nadat de BizTalk-Service is bijgewerkt naar algemene beschikbaarheid (GA), opnieuw implementeren van de overeenkomst.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>UPGRADE: Werkset bevat de oude bridge-pictogrammen na de upgrade van de BizTalk Services SDK
Na de upgrade van een eerdere versie van de BizTalk Services SDK, waarvoor een oude pictogrammen die de bruggen vertegenwoordigt, wordt de werkset blijft de oude pictogrammen voor de bruggen weer te geven. Als u een brug aan de BizTalk Service-project ontwerpoppervlak voor pijplijnen toevoegt, bevat het oppervlak het pictogram Nieuw.  

**Tijdelijke oplossing**. U kunt dit probleem omzeilen door het verwijderen van de bestanden .tbd onder <system drive>: \Users\<gebruiker > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>UPGRADE: BizTalk-Portal-update van Preview-versie voor algemene beschikbaarheid mogelijk weer een foutbericht dat aangeeft dat de EDI-mogelijkheid niet beschikbaar is
Als u bent aangemeld bij de BizTalk Services-Portal terwijl de BizTalk Services is bijgewerkt van Preview-versie voor algemene beschikbaarheid, krijgt u mogelijk de volgende fout in de portal:  

Deze mogelijkheid is niet beschikbaar als onderdeel van deze versie van Microsoft Azure BizTalk Services. Voor het gebruik van overschakelen deze mogelijkheden naar een juiste editie.  

**Resolutie**: afmelden van de portal, sluiten en open de browser en meld u aan bij de portal.  

### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>UPGRADE: Nieuwe traceergegevens niet wordt weergegeven na de upgrade van BizTalk Services algemeen beschikbaar
Wordt ervan uitgegaan dat u een scenario waarin u werkt met een XML-brug geïmplementeerd op de Preview-versie van BizTalk Services-abonnement. U berichten verzenden naar de brug en de bijbehorende gegevens voor het bijhouden is beschikbaar op de BizTalk Services-Portal. Nu, als de BizTalk Services-Portal en BizTalk Services runtime-bits zijn bijgewerkt naar algemene beschikbaarheid, en u een bericht naar het eindpunt van de dezelfde bridge eerder hebt geïmplementeerd verzenden, de traceringsgegevens wordt niet weergegeven voor berichten die worden verzonden na de upgrade.  

### <a name="pipelines-versus-bridges"></a>Pijplijnen en bruggen
In dit document worden de term 'pijplijnen' en 'bruggen' door elkaar gebruikt. Beide betekent in principe hetzelfde te doen, die is, een verwerkingseenheid bericht is geïmplementeerd op BizTalk Services.  

### <a name="concepts"></a>Concepten
[BizTalk Services](https://msdn.microsoft.com/library/azure/hh689864.aspx)   

