---
title: Leads klant configureren | Microsoft Docs
description: Leads klant in de Cloud Partner-Portal configureren.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 2ebc0c90d62d1299c076f8d91ba9fed1998a20e5
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57450189"
---
<a name="get-customer-leads"></a>Leads klant ophalen
==================

In dit artikel wordt uitgelegd hoe u leads klant met behulp van de Cloud Partner-Portal maken. U kunt deze leads verbinden met uw CRM-systeem en integreren in uw verkooppijplijn.

## <a name="leads"></a>Leads

Leads zijn klanten die geïnteresseerd bent in of implementeert uw producten uit de [Azure Marketplace](https://azuremarketplace.microsoft.com/) of [AppSource](https://appsource.microsoft.com).

### <a name="azure-marketplace"></a>Azure Marketplace

1.  Klant heeft een 'Test Drive' van uw aanbieding. Test Drives zijn versneld u de mogelijkheid voor het delen van uw bedrijf direct met potentiële klanten zonder alle barrières voor post. Alle Test Drives een lead genereren voor een klant die graag bij het product voor meer informatie. Meer informatie over Test Drives op [Azure Marketplace Test Drive](https://azuremarketplace.azureedge.net/documents/azure-marketplace-test-drive-program.pdf).

    ![Voorbeelden van Marketplace test drive](./media/cloud-partner-portal-get-customer-leads/test-drive-offer.png)
 

<!-- -->

2.  Klant instemt met het delen van hun gegevens na het selecteren van de 'Nu ophalen'. Deze lead is een **belang voor de eerste** lead, waar we informatie over de klant die is uitgedrukt belang bij het ophalen van uw product delen. De lead is de bovenkant van de trechter overname.

    ![Deze optie nu ophalen](./media/cloud-partner-portal-get-customer-leads/get-it-now-button.png)

3.  Klant 'Aankoop' selecteert de [Azure Portal](https://portal.azure.com/) om op te halen van het product. Deze lead is een **active** lead, waar we informatie over een klant die is begonnen met het implementeren van uw product delen.

    ![Aankoopoptie](./media/cloud-partner-portal-get-customer-leads/purchase-button.png)


### <a name="appsource"></a>AppSource

1.  Klant heeft een 'Test Drive' voor uw aanbieding. Test Drives zijn versneld u de mogelijkheid voor het delen van uw bedrijf direct met potentiële klanten zonder alle barrières voor post. Alle Test Drives, wordt een potentiële klant van een klant die geïnteresseerd gegenereerd bij het product voor meer informatie. Meer informatie over Test Drives op [AppSource Test Drive](https://appsource.microsoft.com/blogs/want-to-try-an-app-take-a-test-drive).

    ![Voorbeeld van test drive](./media/cloud-partner-portal-get-customer-leads/test-drive-offer-2.png)

2.  Klant instemt met het delen van hun gegevens na het selecteren van de 'Nu ophalen'. Deze lead is een **belang voor de eerste** lead, waar we informatie over klant wie uitdrukking belang bij het ophalen van uw product delen. De lead is de bovenkant van de trechter overname.

      ![Deze optie nu ophalen](./media/cloud-partner-portal-get-customer-leads/get-it-now-button-2.png)


3.  Klant selecteert 'Contact opnemen' op uw aanbieding. Deze lead is een **active** lead, waar we informatie over een klant die vraagt om te worden gevolgd door informatie over uw product delen.

    ![Contact opnemen met een optie](./media/cloud-partner-portal-get-customer-leads/contact-me-image.png)

<a name="lead-data"></a>Gegevens van potentiële klanten
---------

Elke lead u tijdens het inkoopproces voor de klant ontvangt heeft gegevens in specifieke velden. Omdat u leads van meerdere stappen ontvangt, is de beste manier voor het afhandelen van de leads dubbele ongedaan maken en aanpassen van de communityleden reageert. Op deze manier elke klant is het ophalen van een toepasselijk bericht en maakt u een unieke relatie.

### <a name="lead-source"></a>Bron van potentiële klanten

De indeling voor een bron van leads is **bron**-**actie** |  **bieden**

**Bronnen**: "AzureMarketplace", "AzurePortal", "TestDrive" en "AppSource (SPZA)"

**Acties**:
- "Modules'--de installatie. Deze actie is op Azure Marketplace of AppSource wanneer een klant uw product koopt.
- "PLT"--staat voor de Partner geleid proefversie. Deze actie is op AppSource wanneer een klant maakt gebruik van de contactpersoon me optie.
- "DNC"--geen contact opnemen. Deze actie is op AppSource wanneer een Partner die cross vermeld op de pagina van uw app is wordt aangevraagd contact met u worden opgenomen. We delen de hoofden up dat deze klant cross is vermeld in uw app, maar niet hoeven te worden bereikt.
- 'Maken'--deze actie is alleen in de Azure-Portal en wordt gegenereerd wanneer een klant uw aanbieding aan hun account aanschaft.
- "StartTestDrive"--deze actie is voor alleen voor Test-stations en wordt gegenereerd wanneer een klant de test drive wordt gestart.

**Aanbiedingen**

De volgende voorbeelden ziet u de unieke id's die zijn toegewezen aan een publisher en een bepaalde aanbieding: checkpoint.check-punt-r77-10sg-byol bitnami.openedxcypress en docusign.3701c77e-1cfa - 4c 56-91e6-3ed0b622145a.


### <a name="customer-info"></a>Klantgegevens

De velden in het volgende voorbeeld worden de klantgegevens die opgenomen in een lead weergeven.
- Voornaam: John
- Achternaam: Smith
- E-mailadres: jsmith\@microsoft.com
- Telefoonnummer: 1234567890
- Land/regio: VS
- Bedrijf: Microsoft
- Titel: CTO

>[!Note]
>Niet alle gegevens in het vorige voorbeeld is altijd beschikbaar voor elke lead.

Er wordt actief gewerkt aan het verbeteren van de leads, dus als er een veld dat u hier niet wordt weergegeven, maar wilt hebben, neem [Stuur ons uw feedback](mailto:AzureMarketOnboard@microsoft.com).

<a name="how-to-connect-your-crm-system-with-the-cloud-partner-portal"></a>Hoe u verbinding met uw CRM-systeem met de Cloud Partner-Portal
------------------------------------------------------------

Om te beginnen met het ophalen van leads, hebben we onze leiden Management-connector gebouwd op de Cloud Partner-Portal zodat u gemakkelijk in uw CRM-gegevens kunt aansluiten, en we zorgt ervoor dat de verbinding voor u. U kunt nu eenvoudig gebruikmaken van de leads die worden gegenereerd door de marketplace zonder een belangrijke technische inspanningen om te integreren met een extern systeem.

![Potentiële klanten management-connector](./media/cloud-partner-portal-get-customer-leads/lead-management-connector.png)

We kunnen schrijven leads in tal van CRM-systemen of rechtstreeks naar een Azure Storage-tabel waarbij u de leads kunt beheren maar u wilt. Elk van de volgende koppelingen vindt u instructies voor het verbinden met mogelijk lead doelen:

-   [Dynamics CRM Online](./cloud-partner-portal-lead-management-instructions-dynamics.md) om op te halen van de instructies voor het configureren van Dynamics CRM Online voor het ophalen van leads.
-   [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md) om op te halen van de instructies voor het instellen van de configuratie van Marketo leiden om op te halen van leads.
-    [SalesForce](./cloud-partner-portal-lead-management-instructions-salesforce.md) voor instructies voor het instellen van uw Salesforce-exemplaar om op te halen van leads.
-    [Azure-tabel](./cloud-partner-portal-lead-management-instructions-azure-table.md) om op te halen van de instructies voor het instellen van uw Azure storage-account voor het ophalen van leads in een Azure-tabel.
-   [HTTPS-eindpunt](./cloud-partner-portal-lead-management-instructions-https.md) om op te halen van de instructies voor het instellen van uw Https-eindpunt om op te halen van leads.

Nadat u de bestemming lead configureren en publiceren van uw aanbieding, we valideren van de verbinding en sturen u een lead test. Wanneer u de aanbieding bekijken voordat u live gaan, kunt u ook de potentiële klant verbinding testen door bij het ophalen van de aanbieding zelf in de preview-omgeving. Het is belangrijk om ervoor te zorgen dat uw recente lead instellingen blijven zodat u toch nog de leads, dus zorg ervoor dat u deze verbindingen bijwerken wanneer er iets is gewijzigd op uw eindgebruikers.

<a name="what-next"></a>Wat zijn de volgende stappen?
----------

Zodra de technische instellen ingesteld is, moet u deze leads opnemen in uw huidige verkoop en marketing strategie en operationele processen. Er zijn we zeer geïnteresseerd in uw inzicht in uw totale verkoop worden verwerkt en u wilt werken nauw samen met u op het bieden van leads van hoge kwaliteit en voldoende gegevens bijdragen aan uw succes. We stellen uw feedback over hoe we kunt optimaliseren en verbeteren van de leads sturen we u met extra gegevens om u te helpen deze klanten geslaagd. Laat het ons weten als u geïnteresseerd bent in [feedback geven](mailto:AzureMarketOnboard@microsoft.com) en suggesties om in te schakelen van uw verkoopteam meer successen te behalen met Marketplace leidt.
