---
title: 'Zelfstudie: Verwerken EDIFACT facturen met behulp van Azure BizTalk Services | Microsoft Docs'
description: Het maken en configureren van de Connector in- of API-app en worden gebruikt in een logische app in Azure App Service
services: biztalk-services
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
ms.assetid: 7e0b93fa-3e2b-4a9c-89ef-abf1d3aa8fa9
ms.service: biztalk-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/31/2016
ms.author: deonhe
ms.openlocfilehash: 2ebd6a8cb70f218c3b56bc78c9b853dbf51ab468
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Zelfstudie: Proces EDIFACT facturen met Azure BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

U kunt de BizTalk Services-Portal gebruiken om te configureren en implementeren van X12 en EDIFACT-overeenkomsten. In deze zelfstudie kijken we hoe u een overeenkomst EDIFACT voor het uitwisselen van facturen tussen handelspartners maakt. Deze zelfstudie is geschreven rond een end-to-end oplossing met betrekking tot twee handelspartners, Northwind en Contoso die EDIFACT berichten uitwisselen.  

## <a name="sample-based-on-this-tutorial"></a>Steekproef op basis van deze zelfstudie
Deze zelfstudie is geschreven om een steekproef **EDIFACT facturen met behulp van BizTalk-Services verzenden**, die kan worden gedownload vanaf de [MSDN-Codegalerie](http://go.microsoft.com/fwlink/?LinkId=401005). Het voorbeeld te gebruiken en gaat u voor deze zelfstudie leert u hoe het voorbeeld is opgebouwd. Of u kunt deze zelfstudie om aan te maken van uw eigen oplossing a t/m. Deze zelfstudie is gericht op de tweede oplossing zodat u begrijpen hoe deze oplossing is opgebouwd. Tevens zo veel mogelijk de zelfstudie is consistent met het voorbeeld en dezelfde namen voor artefacten (bijvoorbeeld, schema's, transformaties) gebruikt als die wordt gebruikt in het voorbeeld.  

> [!NOTE]
> Omdat deze oplossing een bericht verzenden vanuit een bridge EAI naar een EDI-bridge omvat, er wordt gebruikgemaakt van de [BizTalk Services Bridge chaining voorbeeld](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) voorbeeld.  
> 
> 

## <a name="what-does-the-solution-do"></a>Wat is de oplossing?
In deze oplossing ontvangt Northwind EDIFACT facturen van Contoso. Deze facturen zijn niet in een standaard EDI-indeling. Dus vóór het verzenden van de factuur aan Northwind, moet deze worden omgezet naar een document van EDIFACT factuur (ook wel INVOIC). Ontvangen, moet Northwind verwerken van de factuur EDIFACT en een besturingselement-bericht (ook wel CONTRL) terug naar Contoso.

![][1]  

Contoso maakt gebruik van de functies van Microsoft Azure BizTalk Services zodat deze bedrijfsscenario.

* Contoso maakt gebruik van EAI-bruggen voor het transformeren van de oorspronkelijke factuur EDIFACT INVOIC.
* De brug EAI verzendt het bericht vervolgens naar een EDI verzenden brug geïmplementeerd als onderdeel van een overeenkomst is geconfigureerd in de BizTalk Services-Portal.
* De brug EDI-verzenden de EDIFACT INVOIC verwerkt en doorgestuurd naar Northwind.
* Na de ontvangst van de factuur ontvangen Northwind retourneert een CONTRL bericht naar de EDI bridge geïmplementeerd als onderdeel van de overeenkomst.  

> [!NOTE]
> Deze oplossing demonstreert eventueel ook het gebruik van batchverwerking te verzenden van de facturen in batches, in plaats van elke factuur afzonderlijk sturen.  
> 
> 

Voor het voltooien van het scenario, gebruiken we Service Bus-wachtrijen te verzenden factuur van Contoso Northwind of bevestiging ontvangen van Northwind. Deze wachtrijen kunnen worden gemaakt met een clienttoepassing die wordt gedownload en is opgenomen in de Voorbeeldpakket dat beschikbaar is als onderdeel van deze zelfstudie.  

## <a name="prerequisites"></a>Vereisten
* U moet een Service Bus-naamruimte hebben. Zie voor instructies over het maken van een naamruimte [How To: maken of wijzigen van een Service Bus-Service Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Laat het ons wordt ervan uitgegaan dat er al een Service Bus-naamruimte die is ingericht, aangeroepen **edifactbts**.
* U moet een BizTalk Services-abonnement hebben. Voor deze zelfstudie laat het ons wordt ervan uitgegaan dat u hebt een abonnement van BizTalk Services, aangeroepen **contosowabs**.
* Registreer uw abonnement BizTalk Services op de BizTalk Services-Portal. Zie voor instructies [een BizTalk Service-implementatie in de Portal van BizTalk Services registreren](https://msdn.microsoft.com/library/hh689837.aspx)
* Visual Studio is geïnstalleerd, moet u hebben.
* BizTalk Services SDK is geïnstalleerd, moet u hebben. U kunt de SDK van downloaden [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Stap 1: De Service Bus-wachtrijen maken
Deze oplossing maakt gebruik van Service Bus-wachtrijen voor het uitwisselen van berichten tussen handelspartners. Contoso en Northwind verzenden berichten naar de wachtrijen van waar de EAI-en/of EDI-bruggen gebruiken. Voor deze oplossing moet u drie Service Bus-wachtrijen:

* **northwindreceive** – Northwind ontvangt de factuur van Contoso via deze wachtrij.
* **contosoreceive** – Contoso ontvangt de bevestiging van Northwind via deze wachtrij.
* **onderbroken** – alle onderbroken berichten worden doorgestuurd naar deze wachtrij. Berichten worden onderbroken als ze tijdens de verwerking mislukken.

U kunt deze Service Bus-wachtrijen maken met behulp van een clienttoepassing die is opgenomen in het Voorbeeldpakket.  

1. Open in de locatie waar u het voorbeeld hebt gedownload, **zelfstudie verzenden van facturen met behulp van BizTalk Services EDI Bridges.sln**.
2. Druk op **F5** te bouwen en start de **zelfstudie Client** toepassing.
3. Voer de naam van de Service Bus ACS-naamruimte, verlener en sleutel van verlener in het scherm.
   
   ![][2]  
4. Een bericht waarin wordt gevraagd dat drie wachtrijen in uw Service Bus-naamruimte wordt gemaakt. Klik op **OK**.
5. Laat de zelfstudie Client die wordt uitgevoerd. Open het, klikt u op **Service Bus** > ***uw Service Bus-naamruimte*** > **wachtrijen**, en controleer of de drie wachtrijen zijn gemaakt.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Stap 2: Maken en implementeren van handelspartnerovereenkomst
Maak een handelspartnerovereenkomst tussen Contoso en Northwind. Een handelspartnerovereenkomst definieert een servicecontract handel tussen de twee zakenpartners, zoals welke berichtschema moet worden gebruikt, welke messaging protocol, enzovoort. Een handelspartnerovereenkomst bevat twee EDI-bruggen, een berichten verzenden naar handelspartners (aangeroepen de **bridge EDI verzenden**) en één om berichten te ontvangen van handelspartners (aangeroepen de **bridge EDI ontvangen**).

In de context van deze oplossing de EDI verzenden brug komt overeen met de verzendkant van de overeenkomst en de factuur EDIFACT van Contoso verzenden naar Northwind wordt gebruikt. Op dezelfde manier de EDI ontvangen brug komt overeen met de receive-zijde van de overeenkomst en wordt gebruikt voor bevestigingen ontvangen van Northwind.  

### <a name="create-the-trading-partners"></a>De handelspartners maken
Beginnen met, handelspartners voor Contoso en Northwind maken.  

1. In de BizTalk Services-Portal op de **Partners** tabblad **toevoegen**.
2. Voer in de nieuwe pagina van de partner **Contoso** als een Partnernaam en klik vervolgens op **opslaan**.
3. Herhaal de stap voor het maken van de tweede partner **Northwind**.  

### <a name="create-the-agreement"></a>Maken van de overeenkomst
Handelspartnerovereenkomsten worden gemaakt tussen bedrijven-profielen van de handelspartners. Deze oplossing maakt gebruik van de partner standaardprofielen die automatisch worden gemaakt wanneer we de partners hebt gemaakt.  

1. Klik in de BizTalk Services-Portal op **overeenkomsten** > **toevoegen**.
2. In de nieuwe overeenkomst **algemene instellingen** pagina en klik vervolgens op de waarden opgeven, zoals wordt weergegeven in de onderstaande afbeelding **doorgaan**.
   
   ![][3]  
   
   Nadat u op **doorgaan**, twee tabbladen **instellingen ontvangen** en **instellingen voor verzenden** beschikbaar.
3. Maken van de overeenkomst verzenden tussen Contoso en Northwind. Deze overeenkomst beheerst hoe Contoso de factuur EDIFACT naar Northwind verzendt.
   
   1. Klik op **verzenden instellingen**.
   2. Behoud de standaardwaarden op de **binnenkomende URL**, **transformeren**, en **Batching** tabbladen.
   3. Op de **Protocol** tabblad onder de **schema's** sectie, uploadt de **EFACT_D93A_INVOIC.xsd** schema. Dit schema is beschikbaar in het Voorbeeldpakket.
      
      ![][4]  
   4. Op de **Transport** tabblad, geeft u de details voor de Service Bus-wachtrijen. Voor de overeenkomst verzenden aan clientzijde gebruiken we de **northwindreceive** wachtrij verzendt de factuur EDIFACT naar Northwind, en de **onderbroken** wachtrij voor het routeren van berichten die tijdens de verwerking is mislukt en zijn onderbroken. U hebt gemaakt deze wachtrijen in **stap 1: de Service Bus-wachtrijen maken** (in dit onderwerp).
      
      ![][5]  
      
      Onder **Transport instellingen > transporttype** en **opschorten berichtinstellingen > transporttype**, selecteer Azure Service Bus en geef de waarden, zoals wordt weergegeven in de afbeelding.
4. Maak de receive-overeenkomst tussen Contoso en Northwind. Deze overeenkomst beheerst hoe Contoso de bevestiging van Northwind ontvangt.
   
   1. Klik op **ontvangen instellingen**.
   2. Behoud de standaardwaarden op de **Transport** en **transformeren** tabbladen.
   3. Op de **Protocol** tabblad onder de **schema's** sectie, uploadt de **EFACT_4.1_CONTRL.xsd** schema. Dit schema is beschikbaar in het Voorbeeldpakket.
   4. Op de **Route** tabblad, maakt u een filter om ervoor te zorgen dat alleen bevestigingen van Northwind worden doorgestuurd naar Contoso. Onder **Route instellingen**, klikt u op **toevoegen** om de routering filter te maken.
      
      ![][6]  
      
      1. Waarden opgeven voor **regelnaam**, **Route regel**, en **Route bestemming** zoals weergegeven in de afbeelding.
      2. Klik op **Opslaan**.
   5. Op de **Route** tabblad opnieuw, Geef waar onderbroken bevestigingen (bevestigingen die niet voldoen aan tijdens de verwerking), worden doorgestuurd naar. Het transporttype ingesteld op Azure Service Bus, doeltype te routeren **wachtrij**, verificatietype moet worden **Shared Access Signature** (SAS), de SAS-verbindingsreeks opgeven voor de Service Bus-naamruimte en voer vervolgens de naam van de wachtrij als **onderbroken**.
5. Tot slot op **implementeren** voor het implementeren van de overeenkomst. Let op de eindpunten waar de verzenden en ontvangen overeenkomsten geïmplementeerd.
   
   * Op de **instellingen voor verzenden** tabblad onder **binnenkomende URL**, Let op het eindpunt. Een bericht van Contoso om naar te verzenden met behulp van de EDI verzenden bridge Northwind, moet u een bericht verzenden naar dit eindpunt.
   * Op de **instellingen ontvangen** tabblad onder **Transport**, Let op het eindpunt. Een bericht verzenden vanaf Northwind naar Contoso met behulp van de EDI ontvangen bridge, moet u een bericht verzenden naar dit eindpunt.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Stap 3: Maken en implementeren van het project BizTalk Services
In de vorige stap hebt geïmplementeerd de EDI-verzenden en ontvangen overeenkomsten verwerken EDIFACT facturen en bevestigingen. Deze overeenkomsten kunnen alleen berichten verwerken die aan het schema standaard EDIFACT bericht voldoen. Echter per het scenario voor deze oplossing verzendt Contoso een factuur naar Northwind in een intern bedrijfseigen schema. Dus voordat het bericht wordt verzonden naar de brug EDI-verzenden, moet deze worden omgezet in het interne schema aan het standaard EDIFACT factuur schema. Het project BizTalk Services EAI biedt die.

Het project BizTalk Services **InvoiceProcessingBridge**, dat transformaties het bericht ook is opgenomen als onderdeel van de steekproef die u hebt gedownload. Het project bevat de volgende artefacten:

* **INHOUSEINVOICE. XSD** – Schema van de interne factuur dat wordt verzonden naar Northwind.
* **EFACT_D93A_INVOIC. XSD** – Schema van de standaard EDIFACT factuur.
* **EFACT_4.1_CONTRL. XSD** – Schema van de EDIFACT bevestiging dat Northwind naar Contoso verzendt.
* **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – de transformatie die het schema intern factuur wordt toegewezen aan het schema standaard EDIFACT factuur.  

### <a name="create-the-biztalk-services-project"></a>Het project BizTalk Services maken
1. Het project InvoiceProcessingBridge uitvouwen in de Visual Studio-oplossing en open vervolgens de **MessageFlowItinerary.bcs** bestand.
2. Klik ergens op het canvas en stel de **BizTalk Service-URL** in het vak de naam van uw BizTalk Services-abonnement op te geven. Bijvoorbeeld `https://contosowabs.biztalk.windows.net`.
   
   ![][7]  
3. Sleep vanuit de werkset een **Xml One-Way Bridge** toe aan het papier. Stel de **entiteitnaam** en **relatief adres** eigenschappen van de brug naar **ProcessInvoiceBridge**. Dubbelklik op **ProcessInvoiceBridge** openen van het oppervlak bridge-configuratie.
4. Binnen de **berichttypen** vak, klikt u op het plusteken (**+**) om het schema van het binnenkomende bericht opgeven. Omdat het inkomende bericht voor de brug EAI altijd de interne factuur, stel dit in op **INHOUSEINVOICE**.
   
   ![][8]  
5. Klik op de **XML-transformatie** vorm, en in het vak van de eigenschap voor de **Maps** eigenschap, klik op het weglatingsteken (**...** ) knop. In de **Maps selectie** selecteert u de **INHOUSEINVOICE_to_D93AINVOIC** transformatiebestand en klik vervolgens op **OK**.
   
   ![][9]  
6. Ga terug naar **MessageFlowItinerary.bcs**, en sleep vanuit de werkset een **tweerichtings externe Service-eindpunt** rechts van de **ProcessInvoiceBridge**. Stel de **entiteitnaam** eigenschap **EDIBridge**.
7. Vouw in Solution Explorer de **MessageFlowItinerary.bcs** en dubbelklikt u op de **EDIBridge.config** bestand. Vervang de inhoud van de **EDIBridge.config** met de volgende stappen.
   
   > [!NOTE]
   > Waarom moet ik het .config-bestand bewerken? De externe service-eindpunt dat we hebben toegevoegd aan het designer-canvas van brug vertegenwoordigt de EDI-bruggen die we eerder hebt geïmplementeerd. EDI-bruggen zijn wederzijdse bruggen, met een verzenden en ontvangen aan clientzijde. De EAI-brug die we hebben toegevoegd aan de ontwerpfunctie bridge is echter een eenzijdige brug. Dus voor het afhandelen van de verschillende bericht exchange patronen van de twee bruggen we gebruiken een aangepaste bridge gedrag door de configuratie in het .config-bestand. Het aangepaste gedrag verwerkt daarnaast ook de verificatie met het eindpunt EDI verzenden over de sitekoppelingsbrug. Dit gedrag is beschikbaar als een afzonderlijke voorbeeld bij [BizTalk Services Bridge voorbeeld - EAI voor EDI-koppeling](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Deze oplossing wordt gebruikgemaakt van het voorbeeld.  
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
8. Het bestand EDIBridge.config configuratiedetails bijwerken
   
   * Onder  *<behaviors>* , geeft u de ACS-naamruimte en de sleutel gekoppeld aan het abonnement BizTalk Services.
   * Onder  *<client>* , geef het eindpunt waarop de EDI verzenden overeenkomst wordt geïmplementeerd.
   
   Wijzigingen opslaan en sluiten van het configuratiebestand.
9. Klik vanuit de werkset op de **Connector** en deelnemen aan de **ProcessInvoiceBridge** en **EDIBridge** onderdelen. Selecteer de connector en ingesteld in eigenschappen van **filtervoorwaarde** naar **overeen met alle**. Dit zorgt ervoor dat alle berichten die door de brug EAI verwerkt worden doorgestuurd naar de EDI-brug.
   
   ![][10]  
10. Sla de wijzigingen aan de oplossing.  

### <a name="deploy-the-project"></a>Het project implementeren
1. Op de computer waarop u het BizTalk Services-project hebt gemaakt, downloadt en installeert u het SSL-certificaat voor uw abonnement BizTalk Services. Uit, klik onder BizTalk Services op **Dashboard**, en klik vervolgens op **SSL-certificaat downloaden**. Dubbelklik op het certificaat en volg de aanwijzing om de installatie te voltooien. Zorg ervoor dat u het certificaat installeert in **Trusted Root Certification Authorities** certificaatarchief.
2. Klik in Visual Studio Solution Explorer met de rechtermuisknop op de **InvoiceProcessingBridge** project en klik vervolgens op **implementeren**.
3. Geef de waarden, zoals wordt weergegeven in de afbeelding en klik vervolgens op **implementeren**. U kunt de ACS-referenties ophalen voor BizTalk Services door te klikken op **verbindingsgegevens** vanuit het dashboard BizTalk Services.
   
   ![][11]  
   
   Kopieer het eindpunt waarop de brug EAI wordt geïmplementeerd, bijvoorbeeld in het deelvenster uitvoer `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. U kunt dit eindpunt-URL wordt later nodig.  

## <a name="step-4-test-the-solution"></a>Stap 4: De oplossing testen
In dit onderwerp kijken we het testen van de oplossing met behulp van de **zelfstudie Client** geleverd als onderdeel van de voorbeeld-toepassing.  

1. Druk op F5 om te starten in Visual Studio de **zelfstudie Client**.
2. Het scherm moet de waarden vooraf ingevuld van de stap waar we de Service Bus-wachtrijen hebt gemaakt. Klik op **Volgende**.
3. Geef in het volgende venster ACS-referenties voor BizTalk Services-abonnement en de eindpunten waar EAI- en EDI (ontvangen) bruggen zijn geïmplementeerd.
   
   U hebt het eindpunt van de brug EAI had gekopieerd in de vorige stap. Ontvangen voor EDI bridge-eindpunt in de BizTalk Services-Portal, gaat u naar de overeenkomst > ontvangen instellingen > Transport > eindpunt.
   
   ![][12]  
4. Klik in het volgende venster onder Contoso, op de **intern factuur verzenden** knop. In het bestand in het dialoogvenster openen, opent u het bestand INHOUSEINVOICE.txt. De inhoud van het bestand en klik vervolgens op **OK** de factuur te verzenden.
   
   ![][13]  
5. Binnen een paar seconden is de factuur ontvangen door Northwind. Klik op de **bericht weergeven** koppeling om te zien van de door Northwind ontvangen factuur. U ziet hoe de factuur ontvangen door Northwind is in de standaard EDIFACT schema wanneer de handtekening die is verzonden door Contoso een interne schema is.
   
   ![][14]  
6. Selecteer de factuur en klik vervolgens op **bevestiging verzenden**. In het dialoogvenster dat wordt weergegeven, ziet dat de DIF-ID dezelfde zijn in de ontvangen factuur en de bevestiging wordt verzonden. Klik op OK in het **bevestiging verzenden** in het dialoogvenster.
   
   ![][15]  
7. In enkele seconden, wordt de bevestiging is ontvangen bij Contoso.
   
   ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Stap 5 (optioneel): verzenden EDIFACT factuur in batches
BizTalk Services EDI-bruggen ondersteunt ook batchverwerking van uitgaande berichten. Deze functie is handig voor het ontvangen van partners zijn die een reeks berichten (voldoen aan bepaalde criteria) in plaats van afzonderlijke berichten ontvangt.

Het belangrijkste onderdeel bij het werken met batches is de werkelijke release van de batch, ook wel de release-criteria. De release-criteria kunnen worden gebaseerd op hoe de ontvangende partner wil om berichten te ontvangen. Als batchverwerking is ingeschakeld, verzendt de EDI-brug geen het uitgaande bericht aan de ontvangende partner totdat de release-criteria wordt voldaan. Bijvoorbeeld, een batchen criteria op basis van bericht grootte verzendingen een batch alleen wanneer "n" berichten in batch worden opgenomen. Een batchcriteria kunnen op basis van tijd ook worden, zodat een batch op een vaste tijd elke dag wordt verzonden. We proberen de grootte van berichten op basis criteria in deze oplossing.

1. Klik op de overeenkomst die u eerder hebt gemaakt in de BizTalk Services-Portal. Klik op verzendinstellingen > batchverwerking > Batch toevoegen.
2. Voer voor de batchnaam van de, **InvoiceBatch**, Geef een beschrijving en klik vervolgens op **volgende**.
3. Geef de batchcriteria van een, waarmee wordt gedefinieerd welke berichten moeten in batch worden opgenomen. We batch alle berichten in deze oplossing. Ja, schakelt u de geavanceerde optie definities gebruiken en voer **1 = 1**. Dit is een voorwaarde die wordt altijd de waarde true en daarmee alle berichten worden in batch worden opgenomen. Klik op **Volgende**.
   
   ![][17]  
4. Geef de criteria van een batch release. Selecteer in de keuzelijst **MessageCountBased**, en voor **aantal**, geef **3**. Dit betekent dat een batch van drie berichten worden verzonden naar Northwind. Klik op **Volgende**.
   
   ![][18]  
5. Bekijk het overzicht en klik vervolgens op **opslaan**. Klik op **implementeren** te implementeren van de overeenkomst.
6. Ga terug naar de **zelfstudie Client**, klikt u op **intern factuur verzenden**, en volg de aanwijzingen om de factuur te verzenden. U ziet dat er geen factuur is ontvangen door Northwind omdat de batchgrootte van de is niet voldaan aan. Herhaal deze stap twee keer, zodat u drie factuur verzonden berichten aan Northwind hebt. Dit voldoet aan de criteria van de batch release van 3 berichten en u ziet nu een factuur op Northwind.

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

