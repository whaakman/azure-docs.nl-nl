---
title: Overzicht van Microsoft Azure Data Heavy | Microsoft Docs in gegevens
description: Beschrijft Azure Data Box, een cloudoplossing waarmee u enorme hoeveelheden gegevens naar Azure kunt overdragen.
services: databox
documentationcenter: NA
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: overview
ms.date: 05/20/2019
ms.author: alkohli
ms.openlocfilehash: 0f71d9b4400041db50cb3e24940e922acde55edc
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991695"
---
# <a name="what-is-azure-data-box-heavy-preview"></a>Wat is Azure Data Box Heavy? (preview-versie)

Azure Data Box zware kunt u honderden terabytes aan gegevens verzenden naar Azure in een snelle, voordelige, en betrouwbare manier. De gegevens worden overgedragen naar Azure door een Data Box zware apparaat verzenden met een 1 PB opslagcapaciteit, die u met uw gegevens invullen en verzenden naar Microsoft. Het apparaat heeft een robuuste hoofdlettergebruik te beschermen en beveiligen van uw gegevens tijdens de overdracht.

Gegevens in het zware momenteel in preview. Meld u aan om aan te vragen voor een apparaat via de Azure-portal. Zodra het apparaat wordt ontvangen in uw datacenter, instellen met behulp van de lokale webgebruikersinterface. Kopieer de gegevens van uw servers naar het apparaat en stuur het terug naar Azure. In het Azure-datacenter, uw gegevens geüpload naar uw Azure Storage-account (s). Het hele end-to-end-proces in Azure portal, kunt u bijhouden.


> [!IMPORTANT]
> - Data Box Heavy is in de preview-fase. Lees de [Azure-gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert.
> - Meld u aan bij de [Preview-portal](https://aka.ms/azuredatabox) om een apparaat aan te vragen.
> - Tijdens de preview-fase wordt Data Box Heavy verzonden naar klanten in de Verenigde Staten en de Europese Unie. Ga naar [Beschikbaarheid in de regio](#region-availability) voor meer informatie.

## <a name="use-cases"></a>Gebruiksvoorbeelden

Gegevens in het zware is het meest geschikt voor de grootten van de gegevens in de honderden terabytes, waar de netwerkverbinding is onvoldoende voor het uploaden van de gegevens naar Azure. De gegevensverplaatsing kan eenmalig of periodiek zijn, of eerst een grote gegevensoverdracht, gevolgd door meerdere periodieke overdrachten. Hier volgen de verschillende scenario’s waarin Data Box Heavy kan worden gebruikt voor gegevensoverdracht.

 - **Eenmalige migratie**: wanneer grote aantallen on-premises gegevens worden verplaatst naar Azure.
     - Een mediabibliotheek van tapes die offline zijn in Azure om te maken van een online mediabibliotheek verplaatsen.
     - Uw VM-farm, SQL server en toepassingen naar Azure migreren.
     - Historische gegevens te verplaatsen naar Azure voor uitvoerige analyse en rapportage met behulp van HDInsight.

 - **Initiële bulkoverdracht**: wanneer eerst een grote overdracht plaatsvindt met Data Box Heavy (seed), gevolgd door incrementele overdrachten via het netwerk.
     - Gegevens in het zware en een back-upoplossingen partner worden te verplaatsen van de eerste grote historische back-up naar Azure gebruikt. Zodra dat is voltooid, worden gegevens incrementeel via het netwerk overgebracht naar Azure-opslag.

 - **Periodieke uploads**: wanneer er periodiek grote aantallen gegevens worden gegenereerd en verplaatst moeten worden naar Azure. Bijvoorbeeld in de energie-exploitatie, waar er video's worden gegenereerd op olieplatforms en windmolenparken.

## <a name="benefits"></a>Voordelen

Gegevens in het zware is ontworpen om enorme hoeveelheden gegevens naar Azure verplaatsen met weinig of geen invloed op uw netwerk. Deze oplossing biedt de volgende voordelen:

- **Snelheid** -gegevens in het zware maakt gebruik van krachtige 40 Gbps-netwerkinterfaces.

- **Beveiliging** -gegevens in het zware heeft ingebouwde beveiliging beveiligingen voor het apparaat, gegevens en de service.
    - Het apparaat heeft een robuuste behuizing die is beveiligd met fraudebestendige schroeven en verzegelde stickers.
    - De gegevens op het apparaat zijn altijd beveiligd met AES 256-bitsversleuteling.
    - Het apparaat kan alleen worden ontgrendeld met een wachtwoord dat via Azure Portal wordt doorgegeven.
    - De service wordt beschermd door beveiligingsfuncties van Azure.
    - Nadat uw gegevens is geüpload naar Azure, de schijven op het apparaat gewist, in overeenstemming met de National Institute of Standards and Technology (NIST) 800-88r1 standaarden.


## <a name="features-and-specifications"></a>Functies en specificaties

De gegevens in het zware apparaat heeft de volgende functies in deze release.

| Specificaties                                          | Description              |
|---------------------------------------------------------|--------------------------|
| Gewicht                                                  | ~ 500 lbs.                |
| Dimensies                                              | Breedte: 26 inches hoogte: 28 inches lengte: 48 inch |
| Rackruimte                                              | Kan niet worden rek|
| Vereiste kabels                                         | 4 X gegrond 120 V/10 A stroomkabels (NEMA 5 tot 15) opgenomen <br> Apparaat ondersteunt maximaal 240 V power en C-13 power recipiënten heeft <br> Gebruik netwerkkabels compatibel is met [Mellanox MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html)  |
|Energiebeheer                                                    | 4 ingebouwde power supply-eenheden (PSUs) gedeeld tussen beide knooppunten van het apparaat|
| Opslagcapaciteit                                        | ~ 1 PB onbewerkte, 70 schijven van 14 TB <br> 770 TB aan bruikbare capaciteit|
|Aantal knooppunten                                          | 2 onafhankelijke knooppunten per apparaat (500 TB elk) |
| Netwerkinterfaces per knooppunt                             | 4-netwerkinterfaces per knooppunt <br> MGMT, DATA3 <ul><li> 2 x 1 GbE-netwerkinterfaces </li><li> MGMT is voor het beheer, geen gebruiker worden geconfigureerd, die wordt gebruikt voor de eerste installatie </li><li> DATA3 is een interface van de gebruiker configureerbare gegevens, Dynamic Host Configuration Protocol (DHCP) standaard</li><li>1 GbE-netwerkinterfaces kunnen ook worden geconfigureerd als 10 GbE-interfaces</li></ul>Bestand1, DATA2 gegevensinterfaces <ul><li>2 X 40-GbE interfaces </li><li> DHCP door standaard of statische, gebruiker configureerbare gegevensinterfaces</li>|


## <a name="components"></a>Onderdelen

Deze Data Box Heavy bevat de volgende onderdelen:

* **Data Box Heavy-apparaat**: Een fysiek apparaat met een stevige behuizing waarop gegevens veilig worden opgeslagen. Dit apparaat heeft een bruikbaar opslagcapaciteit van 770 TB.
    
* **Data Box-service**: Een extensie in Azure Portal waarmee u een Data Box Heavy-apparaat kunt beheren via een webinterface waartoe u toegang hebt vanaf verschillende geografische locaties. De Data Box-service gebruiken voor het beheren van uw gegevens in het zware apparaat. Met de service kunt u taken uitvoeren zoals orders maken en beheren, waarschuwingen bekijken en beheren en shares beheren.  

* **Lokale webinterface**: een webinterface die wordt gebruikt voor het configureren van het apparaat, zodat u verbinding kunt maken met het lokale netwerk en het apparaat bij de Data Box-service kunt registreren. Gebruik de lokale webinterface om het apparaat uit te schakelen of opnieuw op te starten, logboeken met kopieerbewerkingen te bekijken en contact op te nemen met Microsoft Ondersteuning om een serviceaanvraag in te dienen.


## <a name="the-workflow"></a>De werkstroom

Een typische stroom bestaat uit de volgende stappen:

1. **Bestellen**: plaats een bestelling in Azure Portal en geef de verzendingsgegevens en het Azure-doelopslagaccount voor uw gegevens op. Wanneer het apparaat beschikbaar is, bereidt Azure het voor en verstuurt Azure het met volgnummer.

2. **Ontvangen**: Nadat het apparaat wordt geleverd, sluit u de opgegeven netwerk- en voedingskabels aan. Zet het apparaat aan en sluit het aan. Configureer de netwerkinstellingen voor het apparaat en koppel bestandsshares op de hostcomputer van waar u gegevens wilt kopiëren.

3. **Gegevens kopiëren**: Kopieer gegevens naar de shares van Data Box Heavy.

4. **Retourneren**: Bereid het apparaat voor, zet het uit en stuur het terug naar het Azure-datacenter.

5. **Uploaden**: De gegevens worden automatisch van het apparaat naar Azure gekopieerd. De schijven in het apparaat worden veilig gewist overeenkomstig de richtlijnen van het National Institute of Standards and Technology.

Tijdens dit proces krijgt u een melding via e-mail over alle statuswijzigingen aan de.

## <a name="region-availability"></a>Beschikbaarheid voor de regio

Gegevens in het zware kunt gegevensoverdracht op basis van de regio waarin de service is geïmplementeerd, land/de regio waaraan het apparaat is verzonden en de doel-Azure storage-account waar u de gegevens overbrengt.

- **Beschikbaarheid van service**: Voor deze release is de Data Box Heavy-resource beschikbaar in de volgende regio’s:
    - Alle openbare cloud-regio's in de Verenigde Staten: US - west-centraal, US - west 2, US - west, US - zuid-centraal, US - centraal, US - noord-centraal, US - oost en US - oost 2.
    - Europese Unie: Europa - west en Europa - noord.
    - Verenigd Koninkrijk: UK - zuid en UK - west.
    - Frankrijk: Frankrijk - centraal en Frankrijk - zuid.

- **Doelopslagaccounts**: De opslagaccounts waarin de gegevens worden opgeslagen, zijn beschikbaar in alle Azure-regio’s waar de service beschikbaar is.

Voor de meest actuele informatie over beschikbaarheid in regio's voor gegevens in het zware, gaat u naar [Azure-producten per regio](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all).

## <a name="sign-up"></a>Registreren

Data Box Heavy is in de preview-fase en u moet zich registreren. De volgende stappen om u te registreren voor gegevens in het zware:

1. Meld u aan bij Azure Portal op: https://aka.ms/azuredatabox.
2. Klik op **+ een resource maken** om een nieuwe resource te maken. Zoek naar **Azure Data Box**. Selecteer de **Azure Data Box**-service.

    <!--![The Data Box Heavy sign up 1]()-->

3. Klik op **Create**.

    <!--![The Data Box Heavy sign up 2]()-->

4. Kies het abonnement dat u wilt gebruiken voor de preview-versie van Data Box Heavy. Selecteer de regio waar u de Data Box Heavy-resource wilt implementeren. Klik bij de optie **Data Box Heavy** op **Registreren**.

   <!--![The Data Box Heavy sign up 3]()-->

5. Beantwoord de vragen met betrekking tot de gegevens woonplaats land/regio, tijdsbestek, doel Azure-service voor gegevensoverdracht, netwerkbandbreedte en data transfer frequentie. Lees de privacyverklaring en voorwaarden en schakel het selectievakje in zodat Microsoft uw e-mailadres kan gebruiken om contact met u op te nemen.

    <!--![The Data Box Heavy sign up 4]()-->

Zodra u zich hebt geregistreerd en ingeschakeld voor de Preview-versie, kunt u een Data Box-zwaar kunt bestellen.

    