---
title: Beheer voor cloud-marketplace leiden | Azure Marketplace en AppSource
description: Een overzicht van de verschillende onderwerpen met betrekking tot het publiceren van aanbiedingen en technische artefacten naar de Azure Marketplace en AppSource
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: yijenj
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: Marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/05/2018
ms.author: yijenj
ms.openlocfilehash: 47333a7b5ad7d76ee8bbac19884ba8246eadc05e
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855779"
---
# <a name="lead-management-for-cloud-marketplace"></a>Beheer voor cloud-marketplace leiden


Zijn het midden van een goede zakelijke klanten. In de transformatie van vandaag product aankopen moeten marketeers zich richten op rechtstreeks verbinding maken met klanten en het bouwen van een relatie. Dit is de reden waarom de verkoopcyclus genereren van leads van hoge kwaliteit een essentieel hulpprogramma is. Nadat de aanbieding van uw aanbieding in de [Cloud Partner-Portal](https://cloudpartner.azure.com/), er zijn hulpprogramma's die zijn ingeschakeld voor de contactgegevens van de klant via een programma ontvangt onmiddellijk nadat een klant uitdrukking van belang of uw product in implementeert u de Marketplace. 



## <a name="what-are-leads-in-the-marketplace"></a>Wat zijn potentiële klanten in de marketplace?

De leads die afkomstig zijn van klanten die geïnteresseerd zijn of implementeren van uw producten uit de Marketplace. Of het product wordt vermeld op Azure Marketplace of AppSource, kunt u zich voor het ontvangen van leads van klanten zodra deze juist is ingesteld in uw CRM naar uw aanbieding(en) in de Cloud Partner-Portal 


## <a name="how-to-connect-your-crm-system-with-the-cloud-partner-portal"></a>Hoe u verbinding met uw CRM-systeem met de cloud partner-portal

Als u wilt beginnen met het ophalen van leads, is leiden Management-connector in de Cloud Partner-Portal zo ontworpen dat deze gemakkelijk kan worden aangesloten aan met uw CRM-gegevens naar een lijst van CRM-systeem. U kunt nu eenvoudig gebruikmaken van de leads die worden gegenereerd door de marketplace zonder een belangrijke technische inspanningen om te integreren met een extern systeem.

Hier vindt u stapsgewijze instructies voor het verbinding maken met elk van de mogelijke lead bestemmingen:

**Dynamics CRM Online** - [Klik hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-dynamics) om op te halen van de instructies voor het configureren van Dynamics CRM Online voor het ophalen van leads.

**Marketo** - [Klik hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-marketo) om op te halen van de instructies voor het instellen van de configuratie van Marketo leiden om op te halen van leads.

**SalesForce** - [Klik hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-salesforce) voor instructies voor het instellen van uw Salesforce-exemplaar om op te halen van leads.

**Azure-tabel** – [Klik hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-azure-table) om op te halen van de instructies voor het instellen van uw Azure storage-account voor het ophalen van leads in een Azure-tabel.

**HTTPS-eindpunt** – [Klik hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-https) om op te halen van de instructies voor het instellen van uw Https-eindpunt om op te halen van leads.

Nadat u de bestemming lead correct is geconfigureerd en publiceren van uw aanbieding hebt bereikt, wordt er valideren van de verbinding en ontvangt u een lead test. Wanneer u de aanbieding bekijken voordat u live gaan, kunt u ook de potentiële klant verbinding testen door bij het ophalen van de aanbieding zelf in de preview-omgeving. Het is belangrijk om ervoor te zorgen dat uw recente lead instellingen blijven zodat u toch nog de leads, dus zorg ervoor dat u deze verbindingen bijwerken wanneer er iets is gewijzigd op uw eindgebruikers.


### <a name="what-are-the-next-steps"></a>Wat zijn de volgende stappen?

Zodra de technische instellen ingesteld is, moet u deze leads opnemen in uw huidige verkoop en marketing strategie en operationele processen. We zijn geïnteresseerd in een beter inzicht in uw totale verkoopproces en willen werken nauw samen met u op het bieden van leads van hoge kwaliteit en voldoende gegevens bijdragen aan uw succes. We stellen uw feedback over hoe we kunt optimaliseren en verbeteren van de leads sturen we u met extra gegevens om u te helpen deze klanten geslaagd. Laat het ons weten als u geïnteresseerd bent in geven van feedback en suggesties om in te schakelen van uw verkoopteam meer successen te behalen met Marketplace leidt.



## <a name="common-lead-configuration-errors-during-publishing-on-cloud-partner-portal"></a>Veelvoorkomende fouten in de lead configuratie tijdens het publiceren op de cloud partner-portal 

**Kan de lead aan Dynamics CRM niet opslaan. Controleer de instellingen van Dynamics CRM-account. LastCRMError: Kan niet aanmelden bij Dynamics CRM, LastCRMException:** 

> Als de O365-verificatie is geselecteerd, controleert u of de gebruikersaccount en het wachtwoord geldig is. Als AAD is geselecteerd, controleert u als de tenant-ID, toepassings-ID en toepassing geheime sleutel overeenkomt met wat is ingesteld op AAD. Volg de instructies [hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-dynamics). Als het account van de gebruikersnaam en wachtwoord geldig is, Controleer of er toegang tot Dynamics 365 en heeft een licentie toegewezen (stappen 11-15 als met Azure Active Directory of beveiligingsinstellingen als een gebruiker van Office). 

 
**Kan de lead aan Dynamics CRM niet opslaan. Gebruikers hebben maakt geen machtigingen voor het kenmerk leadsourcecode in de entiteit lead** 

> De toepassing/gebruiker, security-rollen zijn naar Microsoft Marketplace lead writer ontbreekt. Volg de stappen 11-15 als met Azure Active Directory of beveiligingsinstellingen als een gebruiker van Office [hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-dynamics).

**Kan de lead aan Dynamics CRM met behulp van AAD niet opslaan. Uitzonderingen:: Tenant is niet gevonden. Dit exemplaar kan gebeuren als er geen actieve abonnementen voor de tenant zijn.**  

> De map-Id die is opgegeven in de beheersectie lead is geen geldige map. Zorg voor de map-Id op basis van de instructies in stap 2 (onder Azure Active Directory, uit [hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-dynamics) 

**Kan de lead aan Dynamics CRM niet opslaan. LastCRMError: SecLib::RetrievePrivilegeForUser is mislukt: Er zijn geen rollen zijn toegewezen aan gebruiker.**  

> Oplossing: Beveiligingsrol toewijzen aan de Microsoft Marketplace lead schrijver. Volg de instructies [hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-dynamics) onder beveiligingsinstellingen 

**Kan de lead aan Dynamics CRM met behulp van AAD niet opslaan. Uitzonderingen:: Toepassing met de id is niet gevonden in de map** 

> De toepassings-Id opgegeven in de beheersectie lead is geen geldige map. Zorg voor de map-Id op basis van de instructies in stap 8 (onder Azure Active Directory, uit [hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-dynamics)). 

**Kan de lead aan Dynamics CRM met behulp van AAD niet opslaan. Uitzonderingen:: Aangevraagde tenant-id is niet geldig en is niet geldig voor externe domein-indeling** 

> De map-Id die is opgegeven in de beheersectie lead is geen geldige map. Zorg voor de map-Id op basis van de instructies in stap 2 (onder Azure Active Directory, uit [hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-dynamics)). 

**Kan de lead aan Dynamics CRM met behulp van AAD niet opslaan. Uitzondering:: Valideren van referenties.-fout: ongeldige clientgeheim is opgegeven.** 

> Oplossing: Meld u aan bij Azure Portal, controleert u of de sleutel van de toepassing overeenkomt met wat is er in de Cloud Partner-Portal. Genereer een wachtwoord op basis van de instructie bij stap 10 (onder Azure Active Directory), van [hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-dynamics)). 

**Kan de lead aan Dynamics CRM niet opslaan. LastCRMError: De aanvraagkanaal er is een time-out opgetreden tijdens het wachten op een antwoord op een na 00:02:00 uur. Verhoog de time-outwaarde doorgegeven aan het gesprek op aanvraag of vergroot de waarde voor SendTimeout voor de Binding. De tijd die voor deze bewerking wordt toegewezen mogelijk is een deel van een langere time-out.**  

> Oplossing: Aanmelding voor Cloud Partner-Portal, Controleer de details van de winkel >> Lead bestemming >> URL, controleert u of het is een geldige dynamische CRM-exemplaar

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Wat zijn leads en waarom zijn ze belangrijk voor mij als uitgever op Marketplace?** 

Leads zijn klanten die uw producten uit de Marketplace implementeert. Of het product wordt vermeld op [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us) of [AppSource](http://appsource.microsoft.com/), kunt u zich voor het ontvangen van leads van klanten die geïnteresseerd in uw product zijn als u de bestemming lead op uw aanbieding.  


**Waar vind ik help bij het instellen van mijn lead bestemming?** 

U vindt hier documentatie: https://cloudpartner.azure.com/#documentation/get-customer-leads of verzend een ondersteuningsticket via aka.ms/marketplacepublishersupport selecteert het aanbiedingtype en management leiden. 



**Ben ik vereist voor het configureren van een lead doel om te kunnen publiceren van een aanbieding op Marketplace?**

Ja, als u een Contact Me SaaS-app of Consulting Services publiceert.  
 


**Hoe kan ik Bevestig dat de configuratie van de lead correct is?**

Na het instellen van uw aanbieding en de bestemming van de lead, uw aanbieding te publiceren. Op potentiële klanten validatiestap verzendt Marketplace een lead test naar de bestemming lead is geconfigureerd in uw aanbieding. 


**Hoe vind ik de lead test?**


Zoekopdracht voor 'MSFT_TEST"in uw doel lead, als volgt een voorbeeld-testgegevens lead: 

bedrijf MSFT_TEST_636573304831318844 = 

land/regio = US 

Beschrijving = MSFT_TEST_636573304831318844 

e-mailbericht = MSFT_TEST_636573304831318844@test.com

codering UTF-8 = 

codering UTF-8 = 

Voornaam = MSFT_TEST_636573304831318844 

Achternaam = MSFT_TEST_636573304831318844 

lead_source = MSFT_TEST_636573304831318844 MSFT_TEST_636573304831318844 |<Offer Name> 

OID 00Do0000000ZHog = 

telefoon = 1234567890 

titel MSFT_TEST_636573304831318844 = 

 

**Ik heb een live-aanbieding, maar ik zie niet alle leads?**

Elke lead gegevens die worden doorgegeven in de velden in de geselecteerde lead bestemming hebben, de potentiële klanten komen in deze indeling: **bron-Action | Aanbieding** 

  *Bronnen:*

    “AzureMarketplace”, 
    “AzurePortal”, 
    “TestDrive”,  
    “SPZA” (acronym for AppSource) 

  *Acties:*

    “INS” – Stands for Installation. This is on Azure Marketplace or AppSource whenever a customer hits the button to acquire your product. 
    “PLT” – Stands for Partner Led Trial. This is on AppSource whenever a customer hits the Contact me button. 

    “DNC” – Stands for Do Not Contact. This is on AppSource whenever a Partner who was cross listed on your app page gets requested to be contacted. We are sharing the heads up that this customer was cross listed on your app, but they do not need to be contacted. 

    “Create” – This is inside Azure Portal only and is whenever a customer purchases your offer to their account. 

    “StartTestDrive” – This is for Test Drives only and is whenever a customer starts their test drive. 


  *Biedt het volgende:*

    “checkpoint.check-point-r77-10sg-byol”, 
    “bitnami.openedxcypress”, 
    “docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145a” 

 

  *Hier volgt de voorbeeldgegevens van de gegevens van de klant*

    { 

    "FirstName":"John", 

    "LastName":"Smith", 

    "Email":"jsmith@microsoft.com", 

    "Phone":"1234567890", 

    "Country":"US", 

    "Company":"Microsoft", 

    "Title":"CTO" 

    } 

Meer informatie onder [leiden Info](https://cloudpartner.azure.com/#documentation/get-customer-leads). 


**Kan ik Azure BLOB hebt geconfigureerd als de bestemming van mijn lead, waarom zie ik niet de potentiële klant?** 

De potentiële klant wordt alleen geschreven wanneer u Azure BLOB-opslag als de bestemming lead selecteert. Schakel over naar Azure-tabel voor het ontvangen van de realtime lead 


**Ik heb een e-mailbericht hebt ontvangen van Marketplace, waarom kan ik de lead in mijn CRM niet vinden?**  

Het is mogelijk dat de eindgebruiker de e-maildomein van .edu. Om privacyredenen niet we PII-gegevens doorgeven van .edu domein. Verzend een ondersteuningsticket via aka.ms/marketplacepublishersupport 


 **Kan ik Azure tabel/Azure BLOB als de bestemming van mijn lead hebt geconfigureerd, hoe kan ik de leads weergeven?** 

U kunt toegang krijgen tot de blob of table vanuit Azure Portal of u kunt downloaden en installeren [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) gratis om weer te geven van uw Azure storage-account tabellen/blobs. 


**Kan ik Azure Table als mijn lead doel hebt geconfigureerd, kan ik blijf op de hoogte wanneer er een nieuwe lead is verzonden door de Marketplace?** 

Ja, volg de instructies om in te stellen Azure Table + functie omhoog in de documentatie [hier](https://cloudpartner.azure.com/#documentation/lead-management-instructions-azure-table). 



**Ik Salesforce hebt geconfigureerd als de bestemming van mijn lead, waarom kan ik de leads niet vinden?** 

Controleer of het web tot formulier lead een verplicht veld zijn gebaseerd op een selectielijst is. Zo ja, overschakelen op het veld aan een niet-verplichte tekstveld.  
 

**Er is een probleem met mijn lead bestemming en ik bepaalde leads gemist. Kan ik hebben ze aan mij een e-mail verzonden?** 

Vanwege een beleid met PII (persoonlijke informatie), kan niet we gegevens over leads via onbeveiligde e-mail delen. 



**Kan ik de Azure Storage (BLOB/tabel) als de bestemming van mijn lead, hoeveel kost het hebt geconfigureerd?** 

Potentiële klanten ALG gegevens laag is (< 1 GB voor bijna alle uitgevers). De kosten afhankelijk van het aantal leads die zijn ontvangen, als 1000 potentiële klanten worden ontvangen in een maand, kost ongeveer 50 cent. 
