---
title: Een bestaande aanbieding bijwerken voor Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd hoe u een bestaande Azure Marketplace-aanbieding met behulp van de Cloud Partner-Portal bijwerkt.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 4bf11c0a084cb0241125a1ed546f48c1c5455de3
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449449"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Een bestaande aanbieding bijwerken voor Azure Marketplace
==============================================

Er zijn verschillende soorten updates die u op uw aanbieding toepassen kunt zodra deze live verdwenen.   De [Cloud Partner-Portal](https://cloudpartner.azure.com/) bevat verschillende functies om u te helpen bij het bewerken van een aanbieding om te controleren of de wijzigingen zijn juist is, met inbegrip van:

-  Nieuwe versie van de virtuele machine (VM)-installatiekopie toe te voegen aan een bestaande SKU
-  Een SKU is beschikbaar in regio's wijzigen
-  Toevoegen van nieuwe SKU 's
-  Bijwerken van marketplace-metagegevens voor aanbiedingen of SKU 's 
-  Bieden van een bijwerken prijs van betalen per gebruik
-  Vergelijking van functies
-  Geschiedenis van functies

Nadat u een aanbieding of SKU hebt gewijzigd, moet worden gepubliceerd voordat de wijzigingen live gaan.
Dit artikel begeleidt u bij de verschillende aspecten van uw VM-aanbieding wordt bijgewerkt.


<a name="unpermitted-changes-to-vm-offersku"></a>Ongeoorloofde wijzigingen in de VM-aanbieding/SKU
-----------------------------------

Er zijn enkele kenmerken van een VM-aanbieding of de SKU die niet worden gewijzigd zodra de aanbieding live in Azure Marketplace is.

-  Aanbiedings-ID en uitgevers-ID van de aanbieding.
-  SKU-ID van de bestaande SKU's.
-  Telling van bestaande SKU's van de gegevensschijf.
-  Licentie/facturering model wijzigingen in bestaande SKU's.


<a name="updating-the-vm-image-version-for-a-sku"></a>Bijwerken van de versie van de VM-installatiekopie voor een SKU
---------------------------------------

De VM-installatiekopie voor een SKU van uw aanbieding is mogelijk bijgewerkt met extra functies, beveiligingspatches, enzovoort. Onder dergelijke scenario's kunt u de VM-installatiekopie die uw SKU verwijst naar bijwerken. Gebruik de volgende stappen uit om bij te werken van een VM-installatiekopie. 

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2.  Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.
3.  Onder de **SKU's** formulier, klik op de SKU waarvan VM image u wilt bijwerken.
4.  Onder **schijf versie**, klikt u op **+ nieuwe versie van de schijf** om toe te voegen een nieuwe VM-installatiekopie.
5.  Geef de nieuwe VM-installatiekopieën **schijf versie**. De versie van de schijf moet volgen de [semantische versie](http://semver.org/) indeling. Versies moeten zijn van het formulier X.Y.Z, waarbij X, Y en Z gehele getallen zijn. Zorg ervoor dat de nieuwe versie die u opgeeft groter dan de vorige versies is, anders het nummer van de nieuwe versie niet in Azure portal weergegeven wordt of Azure marketplace bij opnieuw publiceren.
6.  Voor **OS VHD URL**, voer de shared access signature (SAS) URI voor het besturingssysteem VHD gemaakt. Houd er rekening mee dat aantal gegevensschijven niet tussen verschillende versies van de SKU wisselen. Als vorige versies had gegevensschijven die zijn geconfigureerd, is deze nieuwe versie moet ook de gegevensschijven die zijn geconfigureerd.
7.  Klik op **publiceren** naar een vliegende start de werkstroom publiceren voor uw nieuwe versie van de virtuele machine om live op Azure marketplace en Azure-portal te gaan.


<a name="changing-regions-a-sku-is-available-in"></a>Wijzigen van regio's is een SKU beschikbaar in
--------------------------------------

Na verloop van tijd kunt u uw aanbieding/SKU beschikbaar maken in meer regio's.
U kunt ook stoppen met het ondersteunen van de aanbieding/SKU in een bepaalde regio.
Deze wijzigingen wilt implementeren, de volgende stappen uit te voeren.

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2.  Onder **alle aanbiedingen** vinden van de aanbieding die u wilt bijwerken.
3.  Onder de **SKU's** vormen, klikt u op de SKU die u wilt bijwerken beschikbaarheid in regio's.
4.  Klik op de **landen selecteren** knop onder de **land/regio beschikbaarheid** veld.
5.  In het pop-beschikbaarheid regio toevoegen/verwijderen de regio's die u wilt gebruiken voor deze SKU.
6.  Klik op **publiceren** naar een vliegende start de werkstroom publiceren om bij te werken van de beschikbaarheid van de regio van uw SKU's.

Als u een SKU wordt beschikbaar gesteld in een nieuwe regio, hebt u de mogelijkheid om op te geven van de prijzen voor die bepaalde regio via het **prijzen gegevens exporteren** functionaliteit. Als u een regio weer die is zodra dit beschikbaar is voor toevoegt, wordt het niet mogelijk om bij te werken met de prijzen omdat prijswijzigingen zijn niet toegestaan.


<a name="adding-a-new-sku"></a>Toevoegen van een nieuwe Voorraadeenheid
----------------

Als u een nieuwe SKU voor uw bestaande aanbieding, volg de onderstaande stappen.

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2.  Onder **alle aanbiedingen** vinden van de aanbieding die u wilt bijwerken.
3.  Onder de **SKU's** vormen, klikt u op **toevoegen van nieuwe Voorraadeenheid** en bieden een **SKU-ID** in het pop-upvenster.
4.  Volg de rest van de stappen die worden beschreven [publiceren van een virtuele machine op Azure Marketplace](./cloud-partner-portal-publish-virtual-machine.md).
5.  Klik op **publiceren** naar een vliegende start de werkstroom publiceren om uw nieuwe Voorraadeenheid live gaan.


<a name="updating-offer-marketplace-metadata"></a>Aanbieding marketplace metagegevens worden bijgewerkt.
------------------------------------

Mogelijk hebt u scenario's waarin u bijwerken van de marketplace-metagegevens die zijn gekoppeld aan uw aanbieding wilt zoals het bijwerken van uw bedrijfslogo, enzovoort. Gebruik de volgende stappen uit om bij te werken van de metagegevens van uw aanbieding.

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2.  Onder **alle aanbiedingen** vinden van de aanbieding die u wilt bijwerken.
3.  Ga naar de **Marketplace** vormen en volg de instructies hier te wijzigen.
4.  Klik op **publiceren** naar een vliegende start de werkstroom publiceren om uw wijzigingen live gaan.


<a name="updating-pricing-on-published-offers"></a>Bijwerken van de prijzen van gepubliceerde aanbiedingen
------------------------------------

Zodra uw betalen per gebruik-aanbieding wordt gepubliceerd, kan u niet de prijs van een bestaande SKU verhogen, maar kunt u een nieuwe Voorraadeenheid onder de dezelfde aanbieding maken, verwijdert u de oude SKU en vervolgens uw aanbod opnieuw publiceren. We ook staan toe dat u bij het verminderen van de prijs van een al gepubliceerde aanbiedingen. De aanbiedingsprijs van uw verminderen:

1.  Klik op de SKU die u wilt verkleinen prijzen.
2.  Als u hebt ingesteld de prijzen in de 1 x 1 GUI, kunt u de prijs rechtstreeks in de gebruikersinterface. Als u via importeren/exporteren werkblad prijzen hebt ingesteld, kunt u alleen prijzen via import/export-functie verminderen.
3.  Klik op **Opslaan**.
4.  Uw aanbod opnieuw publiceren en live gaan.

De prijzen zijn zichtbaar voor nieuwe klanten zodra ze volledig actief op de website is en alle nieuwe klanten wordt de nieuwe lagere prijs betalen.

Voor bestaande klanten worden, de afname van de prijs met terugwerkende kracht weergegeven aan het begin van de factureringscyclus gedurende welke de afname van de prijs werd van kracht.
Als ze al zijn gefactureerd voor de cyclus waarbij er een lagere prijs is opgetreden, ontvangt deze een restitutie tijdens de volgende factureringscyclus voor de lagere prijs.


<a name="simplified-currency-pricing"></a>Vereenvoudigde valuta prijzen
---------------------------

Vanaf September 2018, een nieuwe sectie met de naam **vereenvoudigd valuta prijzen** zichtbaar waren voor u. Microsoft is de Azure Marketplace-bedrijven stroomlijnen door in te schakelen van meer voorspelbare prijzen en verzamelingen van uw klanten over de hele wereld. Deze stroomlijnen worden bereikt door het aantal valuta waarin wij factureren van uw klanten te verminderen.

De nieuwe sectie duurt prijzen in deze nieuwe valuta's. Wanneer alle klanten zijn gemigreerd naar deze nieuwe verwerken valuta's, de oorspronkelijke prijzensectie wordt beëindigd en blijft alleen de sectie 'Valuta prijzen vereenvoudigd'.

Hebt u tot 1 November 2018 om in te stellen van een nieuwe prijs voor de regio's waarin de valuta verwerken wordt gewijzigd. Niet mogelijk de prijs voor regio's waarin de valuta verwerken niet is gewijzigd, te verhogen. Hier volgen de regio's waarin de valuta wordt gewijzigd:

| Land/regio                  | ISO-2-Code | Vereenvoudigde factureringsvaluta |
|--------------------------|-----------|-----------------------------|
| **Algeria**              | DZ        | USD                         |
| **Argentinië**            | AR        | USD                         |
| **Bahrein**              | BH        | USD                         |
| **Belarus**              | BY        | USD                         |
| **Brazilië**               | BR        | BRL                         |
| **Bulgarije**             | BG        | EUR                         |
| **Chili**                | CL        | USD                         |
| **Colombia**             | CO        | USD                         |
| **Costa Rica**           | CR        | USD                         |
| **Kroatië**              | HR        | EUR                         |
| **Tsjechische Republiek**       | CZ        | EUR                         |
| **Egypte**                | EG        | USD                         |
| **Guatemala**            | GT        | USD                         |
| **Hongkong SAR**            | HK        | USD                         |
| **Hongarije**              | HU        | EUR                         |
| **IJsland**              | IS        | EUR                         |
| **Indonesië**            | Id        | USD                         |
| **Israël**               | IL        | USD                         |
| **Jordanië**               | JO        | USD                         |
| **Kazachstan**           | KZ        | USD                         |
| **Kenia**                | KE        | USD                         |
| **Koeweit**               | KW        | USD                         |
| **Liechtenstein**        | LI        | EUR                         |
| **Macedonië (FYROM)**     | MK        | USD                         |
| **Maleisië**             | MY        | USD                         |
| **Mexico**               | MX        | USD                         |
| **Montenegro**           | ME        | USD                         |
| **Morocco**              | MA        | USD                         |
| **Nigeria**              | NG        | USD                         |
| **Oman**                 | OM        | USD                         |
| **Pakistan**             | PK        | USD                         |
| **Paraguay**             | PY        | USD                         |
| **Peru**                 | PE        | USD                         |
| **Filipijnen**          | PH        | USD                         |
| **Polen\***             | PL        | EUR                         |
| **Qatar**                | QA        | USD                         |
| **Roemenië**              | RO        | EUR                         |
| **Saudi-Arabië**         | SA        | USD                         |
| **Servië**               | RS        | USD                         |
| **Singapore**            | SG        | USD                         |
| **Zuid-Afrika**         | ZA        | USD                         |
| **Thailand**             | TH        | USD                         |
| **Trinidad enTobago**  | TT        | USD                         |
| **Tunesië**              | TN        | USD                         |
| **Turkije**               | TR        | USD                         |
| **Oekraïne**              | UA        | USD                         |
| **Verenigde Arabische Emiraten** | AE        | USD                         |
| **Uruguay**              | UY        | USD                         |
|  |  |  |

> [!NOTE]
> Als u uw aanbieding publiceren met API's, kan er een nieuwe sectie in de aanbieding met de naam **JSON**. Dit zou worden voorzien van een `virtualMachinePricingV2` of `monthlyPricingV2`, afhankelijk van het type aanbieding.

Als u vragen hebt over deze wijziging hebt, neem dan contact op met ondersteuning voor Azure Marketplace.


<a name="compare-feature"></a>Functie vergelijken
---------------

Als u wijzigingen in een al gepubliceerde aanbieding aanbrengt, kunt u de *vergelijken* functie om te controleren van de wijzigingen die zijn gemaakt. Vergelijk gebruiken:

1.  Op elk gewenst moment tijdens het bewerken, klikt u op de **vergelijken** knop van uw aanbieding.

2.  Side-by-side-versies van marketing activa en metagegevens weergeven.


<a name="history-of-publishing-actions"></a>Geschiedenis van de publicatie van acties
-----------------------------

Als u wilt een historische publicatie-activiteit weergeven, klikt u op de **geschiedenis** tabblad in het navigatievenster aan de linkerkant van de Cloud Partner-Portal. Hier zich kunt u kunt voorzien van een tijdstempel acties die zijn ondernomen tijdens de levensduur van uw Azure Marketplace-aanbiedingen weergeven.
