---
title: 'Zelfstudie: Verwerken van de EDIFACT-facturen met behulp van Azure BizTalk Services | Microsoft Docs'
description: Het maken en configureren van de Connector in- of API-app en worden gebruikt in een logische app in Azure App Service
services: biztalk-services
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: ''
ms.assetid: 7e0b93fa-3e2b-4a9c-89ef-abf1d3aa8fa9
ms.service: biztalk-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/31/2016
ms.author: deonhe
ms.openlocfilehash: 7093be36e34785d4153c257d411128efe463dee1
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918957"
---
# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Zelfstudie: EDIFACT-facturen verwerken met Azure BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

U kunt de BizTalk Services-Portal gebruiken om te configureren en implementeren van X12 en EDIFACT-overeenkomsten. In deze zelfstudie wordt behandeld hoe u een EDIFACT-overeenkomst voor het uitwisselen van facturen tussen handelspartners maken. Deze zelfstudie is geschreven om een end-to-end oplossing met betrekking tot twee handelspartners, Northwind en Contoso of EDIFACT-berichten uitwisselen.  

## <a name="sample-based-on-this-tutorial"></a>Voorbeeld op basis van deze zelfstudie
Deze zelfstudie is geschreven om een steekproef **EDIFACT facturen met behulp van BizTalk-Services verzenden**, die kan worden gedownload vanaf de [MSDN-Codegalerie](https://go.microsoft.com/fwlink/?LinkId=401005). U kunt gebruik het voorbeeld en u deze zelfstudie om te begrijpen hoe het voorbeeld is gemaakt. Of u kunt deze zelfstudie om te maken van uw eigen oplossing volledig is ontwikkeld-up. In deze zelfstudie is gericht op de tweede methode zodat u begrijpt hoe deze oplossing is gebouwd. Bovendien zo veel mogelijk de zelfstudie is consistent zijn met het voorbeeld en maakt gebruik van dezelfde namen voor artefacten (bijvoorbeeld schema's, transformaties) als in het voorbeeld.  

> [!NOTE]
> Omdat deze oplossing een bericht verzenden vanuit een brug EAI naar een brug EDI omvat, er wordt gebruikgemaakt van de [BizTalk Services-brug chaining voorbeeld](https://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) voorbeeld.  
> 
> 

## <a name="what-does-the-solution-do"></a>Wat is de oplossing?
In deze oplossing ontvangt Northwind EDIFACT-facturen van Contoso. Deze facturen zijn niet in een standard EDI-indeling. Dus voordat de factuur wordt verzonden naar Northwind, moet deze worden omgezet naar een document EDIFACT-factuur (ook wel INVOIC genoemd). Bij ontvangst, moet Northwind verwerken van de EDIFACT-factuur en een controlebericht (ook wel CONTRL) terug naar Contoso.

![][1]  

Contoso maakt gebruik van de functies van Microsoft Azure BizTalk Services voor het bereiken van deze bedrijfsscenario.

* Contoso maakt gebruik van EAI-bruggen naar de oorspronkelijke factuur EDIFACT INVOIC transformeren.
* De brug EAI verzendt vervolgens het bericht naar een EDI verzenden bridge geïmplementeerd als onderdeel van een overeenkomst die is geconfigureerd in de BizTalk Services-Portal.
* De EDI verzenden brug verwerkt de INVOIC EDIFACT en doorgestuurd naar Northwind.
* Na de ontvangst van de factuur ontvangt Northwind retourneert een CONTRL bericht naar de EDI bridge geïmplementeerd als onderdeel van de overeenkomst.  

> [!NOTE]
> Deze oplossing toont eventueel ook het gebruik van batchverwerking voor het verzenden van facturen in batches, in plaats van elke factuur afzonderlijk verzenden.  
> 
> 

Voor het voltooien van het scenario, we Service Bus-wachtrijen gebruiken om de factuur van Contoso Northwind verzenden of ontvangen van bevestiging van Northwind. Deze wachtrijen kunnen worden gemaakt met een clienttoepassing die is gedownload en is opgenomen in de Voorbeeldpakket dat beschikbaar is als onderdeel van deze zelfstudie.  

## <a name="prerequisites"></a>Vereisten
* U moet een Service Bus-naamruimte hebben. Zie voor instructies over het maken van een naamruimte [How To: Maken of wijzigen van een Service Bus-Service Namespace](/previous-versions/azure/azure-services/hh674478(v=azure.100)). Laat het ons wordt ervan uitgegaan dat u al een Service Bus-naamruimte hebt ingericht, met de naam **edifactbts**.
* U moet een BizTalk Services-abonnement hebben. Voor deze zelfstudie laat het ons wordt ervan uitgegaan dat u hebt een abonnement van BizTalk Services, met de naam **contosowabs**.
* Registreer uw BizTalk Services-abonnement op de BizTalk Services-Portal. Zie voor instructies [registreren van een BizTalk Service-implementatie op de BizTalk Services-Portal](/previous-versions/azure/hh689837(v=azure.100))
* Visual Studio is geïnstalleerd, moet u hebben.
* BizTalk Services SDK is geïnstalleerd, moet u hebben. U kunt de SDK van downloaden [https://go.microsoft.com/fwlink/?LinkId=235057](https://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Stap 1: Maken van de Service Bus-wachtrijen
Deze oplossing maakt gebruik van Service Bus-wachtrijen voor het uitwisselen van berichten tussen handelspartners gerevolutionaliseerd. Contoso en Northwind verzenden berichten naar de wachtrijen van waar de EAI-en/of EDI-bruggen gebruiken. Voor deze oplossing moet u drie Service Bus-wachtrijen:

* **northwindreceive** – Northwind ontvangt de factuur van Contoso via deze wachtrij.
* **contosoreceive** : Contoso ontvangt de bevestiging van Northwind via deze wachtrij.
* **onderbroken** – alle onderbroken berichten worden doorgestuurd naar deze wachtrij. Berichten worden onderbroken wanneer ze tijdens de verwerking mislukken.

U kunt deze Service Bus-wachtrijen maken met behulp van een clienttoepassing die is opgenomen in het Voorbeeldpakket.  

1. Open in de locatie waar u het voorbeeld hebt gedownload, **zelfstudie verzenden van facturen met behulp van BizTalk Services EDI Bridges.sln**.
2. Druk op **F5** om te bouwen en start de **zelfstudie Client** toepassing.
3. Voer in het scherm in de Service Bus ACS-naamruimte, naam en sleutel van verlener.
   
   ![][2]  
4. Een bericht waarin wordt gevraagd dat drie wachtrijen worden gemaakt in uw Service Bus-naamruimte. Klik op **OK**.
5. Laat u de zelfstudie Client dat wordt uitgevoerd. Open het, klikt u op **Service Bus** > ***uw Service Bus-naamruimte*** > **wachtrijen**, en controleer of de drie wachtrijen zijn gemaakt.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Stap 2: Maken en implementeren van handelspartnerovereenkomst
Een handelspartnerovereenkomst tussen Contoso en Northwind maken. Een handelspartnerovereenkomst definieert een contract handel tussen de twee business-partners, zoals welke berichtschema moet worden gebruikt, welke berichtenprotocol, enzovoort. Een handelspartnerovereenkomst bevat twee EDI-bruggen, een berichten te verzenden naar handelspartners (met de naam de **EDI verzenden bridge**) en één voor het ontvangen van berichten van handelspartners (met de naam de **EDI ontvangen bridge**).

In de context van deze oplossing, de EDI verzenden brug komt overeen met de verzendkant van de overeenkomst en wordt gebruikt voor het verzenden van de EDIFACT-facturen van Contoso te Northwind. Op dezelfde manier de EDI ontvangen brug komt overeen met de ontvangstzijde van de overeenkomst en wordt gebruikt voor het ontvangen bevestigingen van Northwind.  

### <a name="create-the-trading-partners"></a>Maken van de handelspartners
Te beginnen met handelspartners voor Contoso en Northwind maken.  

1. In de BizTalk Services-Portal op de **Partners** tabblad **toevoegen**.
2. Voer in de nieuwe pagina van de partner **Contoso** als een Partnernaam en klik op **opslaan**.
3. Herhaal de stap voor het maken van de tweede partner **Northwind**.  

### <a name="create-the-agreement"></a>Maken van de overeenkomst
Handelspartnerovereenkomsten worden gemaakt tussen de bedrijven-profielen van handelspartners. Deze oplossing maakt gebruik van de standaardprofielen voor partners die automatisch worden gemaakt bij het maken van de partners.  

1. Klik in de BizTalk Services-Portal op **overeenkomsten** > **toevoegen**.
2. In de nieuwe overeenkomst **algemene instellingen** pagina en klik vervolgens op de waarden opgeven, zoals wordt weergegeven in de onderstaande afbeelding **doorgaan**.
   
   ![][3]  
   
   Nadat u op **doorgaan**, twee tabbladen **instellingen ontvangen** en **instellingen voor verzenden** beschikbaar.
3. De overeenkomst verzenden tussen Contoso en Northwind maken. Deze overeenkomst bepaalt hoe Contoso de EDIFACT-factuur naar Northwind verzendt.
   
   1. Klik op **verzendinstellingen**.
   2. De standaardwaarden behouden op de **binnenkomende URL**, **transformeren**, en **batchverwerking** tabbladen.
   3. Op de **Protocol** tabblad onder de **schema's** gedeelte in en upload het **EFACT_D93A_INVOIC.xsd** schema. Dit schema is beschikbaar in het Voorbeeldpakket.
      
      ![][4]  
   4. Op de **Transport** tabblad, geef de details op voor de Service Bus-wachtrijen. Voor de overeenkomst verzenden aan clientzijde, gebruiken we de **northwindreceive** wachtrij voor het verzenden van de EDIFACT-factuur naar Northwind, en de **onderbroken** wachtrij voor het routeren van berichten die tijdens de verwerking en zijn onderbroken. U hebt gemaakt met deze wachtrijen in **stap 1: Maken van de Service Bus-wachtrijen** (in dit onderwerp).
      
      ![][5]  
      
      Onder **Transport instellingen > type Transport** en **bericht onderbreking instellingen > type Transport**, selecteert u Azure Service Bus en geef de waarden op zoals weergegeven in de afbeelding.
4. De overeenkomst ontvangen tussen Contoso en Northwind maken. Deze overeenkomst bepaalt hoe Contoso de bevestiging van Northwind ontvangt.
   
   1. Klik op **ontvangstinstellingen**.
   2. De standaardwaarden behouden op de **Transport** en **transformeren** tabbladen.
   3. Op de **Protocol** tabblad onder de **schema's** gedeelte in en upload het **EFACT_4.1_CONTRL.xsd** schema. Dit schema is beschikbaar in het Voorbeeldpakket.
   4. Op de **Route** tabblad, maakt u een filter om ervoor te zorgen dat alleen bevestigingen van Northwind worden gerouteerd naar Contoso. Onder **Route instellingen**, klikt u op **toevoegen** om de routering filter te maken.
      
      ![][6]  
      
      1. Waarden opgeven voor **regelnaam**, **Route regel**, en **Route bestemming** zoals weergegeven in de afbeelding.
      2. Klik op **Opslaan**.
   5. Op de **Route** tabblad opnieuw, geeft u waar onderbroken bevestigingen (bevestigingen die tijdens de verwerking) om te worden gerouteerd. Het transporttype ingesteld op Azure Service Bus, route naar het doeltype **wachtrij**, verificatietype **Shared Access Signature** (SAS), de SAS-verbindingsreeks opgeven voor de Service Bus-naamruimte en voer vervolgens de naam van de wachtrij als **onderbroken**.
5. Klik tot slot, **implementeren** implementeren van de overeenkomst. Houd er rekening mee de eindpunten waarbij het verzenden en ontvangen van overeenkomsten geïmplementeerd.
   
   * Op de **instellingen voor verzenden** tabblad onder **binnenkomende URL**, houd er rekening mee het eindpunt. Een bericht van Contoso om naar te verzenden met behulp van de EDI verzenden bridge Northwind, moet u een bericht verzenden naar dit eindpunt.
   * Op de **instellingen ontvangen** tabblad onder **Transport**, houd er rekening mee het eindpunt. Een bericht verzenden vanaf Northwind naar Contoso bridge, met de EDI ontvangt u een bericht naar dit eindpunt moet verzenden.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Stap 3: De BizTalk Services-webproject maken en implementeren
In de vorige stap hebt geïmplementeerd de EDI-verzenden en ontvangen van overeenkomsten voor het verwerken van de EDIFACT-facturen en bevestigingen. Deze overeenkomsten kunnen alleen verwerken van berichten die aan het standaard schema van de EDIFACT-bericht voldoen. Echter per het scenario voor deze oplossing verzendt Contoso een factuur naar Northwind in een eigen eigen schema. Dus voordat het bericht wordt verzonden naar de brug EDI-verzenden, moet deze worden omgezet in het interne schema naar het standaard schema van de EDIFACT-factuur. De BizTalk Services EAI-project doet.

De BizTalk Services-project, **InvoiceProcessingBridge**, dat transformaties het bericht ook is opgenomen als onderdeel van het voorbeeld dat u hebt gedownload. Het project bevat de volgende artefacten:

* **INHOUSEINVOICE. XSD** -Schema van de interne factuur die wordt verzonden naar Northwind.
* **EFACT_D93A_INVOIC. XSD** -Schema van de standaard EDIFACT-factuur.
* **EFACT_4.1_CONTRL. XSD** -Schema van de EDIFACT-bevestiging Northwind naar Contoso verzendt.
* **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – de transformatie die het schema van de interne factuur wordt toegewezen aan het schema van de standaard EDIFACT factuur.  

### <a name="create-the-biztalk-services-project"></a>De BizTalk Services-project maken
1. Vouw het InvoiceProcessingBridge-project in Visual Studio-oplossing, en open vervolgens de **MessageFlowItinerary.bcs** bestand.
2. Klik ergens op het canvas en stel de **BizTalk Service-URL** in het vak om op te geven van de naam van uw BizTalk Services-abonnement. Bijvoorbeeld `https://contosowabs.biztalk.windows.net`.
   
   ![][7]  
3. Sleep vanuit de werkset een **Xml One-Way Bridge** toe aan het papier. Stel de **entiteitnaam** en **relatief adres** eigenschappen van de brug en **ProcessInvoiceBridge**. Dubbelklik op **ProcessInvoiceBridge** openen van het oppervlak bridge-configuratie.
4. Binnen de **berichttypen** vak, klikt u op het plusteken (**+**) om het schema van het binnenkomende bericht opgeven. Aangezien het binnenkomende bericht voor de brug EAI altijd gelijk is aan de interne factuur, stel dit in op **INHOUSEINVOICE**.
   
   ![][8]  
5. Klik op de **Xml-transformatie** vorm, en in het vak voor de **Maps** eigenschap, klik op het weglatingsteken (**...** ) knop. In de **Maps selectie** in het dialoogvenster, selecteer de **INHOUSEINVOICE_to_D93AINVOIC** bestand transformeren, en klik vervolgens op **OK**.
   
   ![][9]  
6. Ga terug naar **MessageFlowItinerary.bcs**, en sleep vanuit de werkset een **tweerichtings externe Service-eindpunt** aan de rechterkant van de **ProcessInvoiceBridge**. Stel de **entiteitnaam** eigenschap **EDIBridge**.
7. Vouw in Solution Explorer de **MessageFlowItinerary.bcs** en dubbelklikt u op de **EDIBridge.config** bestand. Vervang de inhoud van de **EDIBridge.config** met de volgende stappen.
   
   > [!NOTE]
   > Waarom moet ik het .config-bestand bewerken? De externe service-eindpunt dat wordt toegevoegd aan de brug designer-canvas vertegenwoordigt de EDI-bruggen dat we eerder hebt geïmplementeerd. EDI-bruggen zijn beide richtingen bruggen, met een verzenden en ontvangen aan clientzijde. De EAI-brug die wordt toegevoegd aan de ontwerpfunctie bridge is echter een eenzijdige brug. Ja, voor het afhandelen van de ander bericht exchange patronen in de twee bruggen, gebruiken we een aangepaste bridge-gedrag door de configuratie op te nemen in het .config-bestand. Het aangepaste gedrag verwerkt bovendien ook de verificatie naar de EDI verzenden bridge-eindpunt. Dit gedrag is beschikbaar als een afzonderlijke voorbeeld bij [BizTalk Services Bridge-voorbeeld - EAI naar EDI-koppeling](https://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Deze oplossing wordt gebruikgemaakt van het voorbeeld.  
   > 
   > 
   
   ```
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
     <system.serviceModel>
       <extensions>
         <behaviorExtensions>
           <add name="BridgeAuthentication" 
                 type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
         </behaviorExtensions>
       </extensions>
       <behaviors>
         <endpointBehaviors>
           <behavior name="BridgeAuthenticationConfiguration">
             <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
             <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                   issuername="owner" 
                                   issuersecret="[YOUR ACS SECRET]" />
             <webHttp />
           </behavior>
         </endpointBehaviors>
       </behaviors>
       <bindings>
         <webHttpBinding>
           <binding name="BridgeBindingConfiguration">
             <security mode="Transport" />
           </binding>
         </webHttpBinding>
       </bindings>
       <client>
         <clear />
         <!--
           Go BizTalk Portal > Agreement > Send Settings > Inbound URL
           Copy the Endpoint URL and paste it in the below address field
         -->
         <endpoint name="TwoWayExternalServiceEndpointReference1" 
                   address="[YOUR EDI BRIDGE SEND URI]" 
                   behaviorConfiguration="BridgeAuthenticationConfiguration" 
                   binding="webHttpBinding" 
                   bindingConfiguration="BridgeBindingConfiguration" 
                   contract="System.ServiceModel.Routing.IRequestReplyRouter" />
       </client>
     </system.serviceModel>
   </configuration>
   
   ```
8. Het bestand EDIBridge.config om op te nemen informatie over de configuratie bijwerken
   
   * Onder *<behaviors>*, geeft u de ACS-naamruimte en de sleutel die is gekoppeld aan het abonnement van BizTalk Services.
   * Onder *<client>*, geef het eindpunt waarop de overeenkomst voor het verzenden van EDI wordt geïmplementeerd.
   
   Wijzigingen opslaan en sluiten van het configuratiebestand.
9. Vanuit de werkset, klikt u op de **Connector** en deelnemen aan de **ProcessInvoiceBridge** en **EDIBridge** onderdelen. Selecteer de connector en stel in eigenschappen van **filtervoorwaarde** naar **overeen met alle**. Dit zorgt ervoor dat alle berichten die door de brug EAI verwerkt, gerouteerd naar de EDI-brug.
   
   ![][10]  
10. Sla de wijzigingen aan de oplossing.  

### <a name="deploy-the-project"></a>Het project implementeren
1. Op de computer waarop u het BizTalk Services-project hebt gemaakt, download en installeer het SSL-certificaat voor uw BizTalk Services-abonnement. Uit, klik onder BizTalk Services, op **Dashboard**, en klik vervolgens op **SSL-certificaat downloaden**. Dubbelklik op het certificaat en volg de aanwijzing om de installatie te voltooien. Zorg ervoor dat u het certificaat installeert in **Trusted Root Certification Authorities** certificaatarchief.
2. In Visual Studio Solution Explorer met de rechtermuisknop op de **InvoiceProcessingBridge** project en klik vervolgens op **implementeren**.
3. Geef de waarden op zoals weergegeven in de afbeelding, en klik vervolgens op **implementeren**. U kunt de ACS-referenties ophalen voor BizTalk Services door te klikken op **verbindingsgegevens** vanuit het dashboard van BizTalk Services.
   
   ![][11]  
   
   Kopieer het eindpunt waarop de brug EAI wordt geïmplementeerd, bijvoorbeeld in het deelvenster uitvoer `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. U kunt dit eindpunt-URL wordt later nodig.  

## <a name="step-4-test-the-solution"></a>Stap 4: Testen van de oplossing
In dit onderwerp wordt behandeld hoe u voor het testen van de oplossing met behulp van de **zelfstudie Client** toepassing geleverd als onderdeel van het voorbeeld.  

1. Druk op F5 om te starten in Visual Studio, de **zelfstudie Client**.
2. Het scherm moet de waarden die vooraf zijn ingevuld in de stap waar we de Service Bus-wachtrijen hebt gemaakt. Klik op **volgende**.
3. Geef in het volgende venster ACS-referenties voor BizTalk Services-abonnement en de eindpunten waarbij EAI en EDI (ontvangen) bruggen worden geïmplementeerd.
   
   U hebt het eindpunt van de brug EAI had gekopieerd in de vorige stap. Ontvangen voor EDI bridge-eindpunt in de BizTalk Services-Portal, gaat u naar de overeenkomst > instellingen voor ontvangen > Transport > eindpunt.
   
   ![][12]  
4. Klik in het volgende venster onder Contoso, op de **interne factuur verzenden** knop. In het bestand in het dialoogvenster openen, opent u het bestand INHOUSEINVOICE.txt. Bekijk de inhoud van het bestand en klik vervolgens op **OK** voor het verzenden van de factuur.
   
   ![][13]  
5. De factuur wordt ontvangen in een paar seconden bij Northwind. Klik op de **bericht weergeven** koppeling om te zien van de factuur door Northwind ontvangen. U ziet hoe de ontvangen door Northwind factuur is in het standaard EDIFACT-schema terwijl het certificaat dat is verzonden door Contoso een interne schema is.
   
   ![][14]  
6. Selecteer de factuur en klik vervolgens op **bevestiging verzenden**. In het dialoogvenster dat verschijnt, merk op dat het uitwisselings-ID dezelfde zijn in de ontvangen factuur en de ontvangstbevestiging worden verzonden. Klik op OK in het **bevestiging verzenden** in het dialoogvenster.
   
   ![][15]  
7. Binnen een paar seconden, wordt de bevestiging is ontvangen bij Contoso.
   
   ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Stap 5 (optioneel): EDIFACT-facturen in batches verzenden
BizTalk Services EDI bruggen biedt ook ondersteuning voor batchverwerking van uitgaande berichten. Deze functie is handig voor het ontvangen van partners die liever een berichtenbatch (voldoen aan bepaalde criteria) in plaats van afzonderlijke berichten ontvangen.

De belangrijkste aspecten bij het werken met batches is de daadwerkelijke versie van de batch, ook wel de releasecriteria. De releasecriteria kunnen worden gebaseerd op hoe de ontvangende partner wil om berichten te ontvangen. Indien batchverwerking is ingeschakeld, verzendt de EDI-brug geen uitgaande berichten aan de ontvangende partner totdat de releasecriteria wordt voldaan. Bijvoorbeeld, een batchverwerkingsindeling criteria op basis van bericht grootte verzendt een batch alleen wanneer "n" berichten in batch worden opgenomen. Een batchcriteria kunnen op basis van tijd, ook zijn, zodat een batch op een vaste tijd elke dag wordt verzonden. We proberen de grootte van het bericht op basis van criteria in deze oplossing.

1. Klik op de overeenkomst die u eerder hebt gemaakt in de BizTalk Services-Portal. Klik op instellingen voor het verzenden > batchverwerking > Batch toevoegen.
2. Voer voor de batchnaam van de, **InvoiceBatch**, Geef een beschrijving op en klik vervolgens op **volgende**.
3. Geef de batchcriteria van een, waarmee wordt gedefinieerd welke berichten in batch moeten worden opgenomen. In deze oplossing batch we alle berichten. Dus, selecteert u de geavanceerde optie definities gebruiken en voer **1 = 1**. Dit is een voorwaarde die wordt altijd de waarde true en kan daarom alle berichten worden in batch worden opgenomen. Klik op **volgende**.
   
   ![][17]  
4. Geef een criteria voor batch vrijgeven. Selecteer in de keuzelijst **MessageCountBased**, en voor **aantal**, geef **3**. Dit betekent dat een batch van drie berichten worden verzonden naar Northwind. Klik op **volgende**.
   
   ![][18]  
5. Bekijk de samenvatting en klik vervolgens op **opslaan**. Klik op **implementeren** om de overeenkomst opnieuw te implementeren.
6. Ga terug naar de **zelfstudie Client**, klikt u op **interne factuur verzenden**, en volg de aanwijzingen voor het verzenden van de factuur. U ziet dat er geen factuur is ontvangen door Northwind omdat niet wordt voldaan aan de batchgrootte. Herhaal deze stap nog twee keer, zodat er drie factuur berichten verzonden naar Northwind. Deze voldoet aan de criteria voor batch vrijgeven van 3 berichten en u ziet nu een factuur op Northwind.

<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

