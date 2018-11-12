---
title: Overzicht van Microsoft Azure Data Heavy | Microsoft Docs in gegevens
description: Beschrijft Azure Data Box, een cloudoplossing waarmee u enorme hoeveelheden gegevens naar Azure kunt overdragen.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 0a5b7f93f9ac6cc5b1076881727a42fd5b95ff4b
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235153"
---
# <a name="what-is-azure-data-box-heavy-preview"></a>Wat is Azure Data Box Heavy? (Preview)

Met de hybride oplossing Microsoft Azure Data Box Heavy kunt u honderden terabytes aan on-premises gegevens op een snelle, goedkope en betrouwbare manier naar Azure verzenden. De veilige overdracht van gegevens vindt snel plaats omdat we een opslagapparaat naar u opsturen met een capaciteit van 1 PB. Dit apparaat heeft een stevige behuizing om uw gegevens tijdens het verzenden te beschermen.

Data Box Heavy is in de preview-fase en u kunt een apparaat aanvragen door u aan te melden via Azure Portal. Nadat we het apparaat hebben ontvangen in het datacenter, kunt u het instellen met de lokale webinterface. Kopieer de gegevens van uw servers naar het apparaat en stuur het terug naar Azure. In het Azure-datacenter worden uw gegevens automatisch van de schijven naar Azure geüpload. Het volledige proces wordt gevolgd door de Data Box-service in Azure Portal.


> [!IMPORTANT]
> - Data Box Heavy is in de preview-fase. Lees de [Azure-gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert. 
> - Meld u aan bij de [Preview-portal](https://aka.ms/) om een apparaat aan te vragen.
> - Tijdens de preview-fase wordt Data Box Heavy verzonden naar klanten in de Verenigde Staten en de Europese Unie. Ga naar [Beschikbaarheid in de regio](#region-availability) voor meer informatie.

## <a name="use-cases"></a>Gebruiksvoorbeelden

Data Box Heavy is ideaal voor het overbrengen van gegevens met een totale omvang van 500 TB in scenario’s waarbij netwerkbandbreedte beperkt of afwezig is. De gegevensverplaatsing kan eenmalig of periodiek zijn, of eerst een grote gegevensoverdracht, gevolgd door meerdere periodieke overdrachten. Hier volgen de verschillende scenario’s waarin Data Box Heavy kan worden gebruikt voor gegevensoverdracht.

 - **Eenmalige migratie**: wanneer grote aantallen on-premises gegevens worden verplaatst naar Azure. 
     - Verplaatsen van een mediabibliotheek van offline tapes naar Azure, voor het maken van een online mediabibliotheek.
     - Uw virtuele machines, SQL Server en toepassingen migreren naar Azure
     - Verplaatsen van historische gegevens naar Azure voor gedetailleerde analyse en rapportage met HDInsight.

 - **Initiële bulkoverdracht**: wanneer eerst een grote overdracht plaatsvindt met Data Box Heavy (seed), gevolgd door incrementele overdrachten via het netwerk. 
     - Bijvoorbeeld: voor de eerste grote back-up van historische gegevens naar Azure wordt gebruikgemaakt van een back-upoplossing van Commvault of Data Box Heavy. Zodra dat is voltooid, worden gegevens incrementeel via het netwerk overgebracht naar Azure-opslag.

 - **Periodieke uploads**: wanneer er periodiek grote aantallen gegevens worden gegenereerd en verplaatst moeten worden naar Azure. Bijvoorbeeld in de energie-exploitatie, waar er video's worden gegenereerd op olieplatforms en windmolenparken.      

## <a name="benefits"></a>Voordelen

Data Box Heavy is ontworpen om enorme aantallen gegevens naar Azure te verplaatsen, zonder impact op het netwerk. Deze oplossing biedt de volgende voordelen:

- **Snelheid**: Data Box Heavy maakt gebruik van netwerkinterfaces met een hoge capaciteit van 40 Gpbs.

- **Beveiligd**: Data Box Heavy heeft ingebouwde beveiliging voor het apparaat, de gegevens en de service.
    - Het apparaat heeft een robuuste behuizing die is beveiligd met fraudebestendige schroeven en verzegelde stickers. 
    - De gegevens op het apparaat zijn altijd beveiligd met AES 256-bitsversleuteling.
    - Het apparaat kan alleen worden ontgrendeld met een wachtwoord dat via Azure Portal wordt doorgegeven.
    - De service wordt beschermd door beveiligingsfuncties van Azure.
    - Nadat uw gegevens zijn geüpload naar Azure, worden alle schijven in het apparaat gewist overeenkomstig NIST 800-88r1-standaarden.


<!--## Features and specifications

The Data Box Heavy device has the following features in this release.

| Specifications                                          | Description              |
|---------------------------------------------------------|--------------------------|
| Weight                                                  | < 50 lbs.                |
| Dimensions                                              | Device - Width: 309.0 mm Height: 430.4 mm Depth: 502.0 mm |            
| Rack space                                              | 7 U when placed in the rack on its side (cannot be rack-mounted)|
| Cables required                                         | 1 X power cable (included) <br> 2 RJ45 cables <br> 2 X SFP+ Twinax copper cables|
| Storage capacity                                        | 100 TB <br> 80 TB usable capacity after RAID 5 protection|
| Network interfaces                                      | 2 X 1 GbE interface - MGMT, DATA 3. <br> MGMT - for management, not user configurable, used for initial setup <br> DATA3 - for data, user configurable, and is dynamic by default <br> MGMT and DATA 3 can also work as 10 GbE <br> 2 X 10 GbE interface - DATA 1, DATA 2 <br> Both are for data, can be configured as dynamic (default) or static |
| Data transfer media                                     | RJ45, SFP+ copper 10 GbE Ethernet  |
| Security                                                | Rugged device casing with tamper-proof custom screws <br> Tamper-evident stickers placed at the bottom of the device|
| Data transfer rate                                      | Up to 80 TB in a day over 10 GbE network interface        |
| Management                                              | Local web UI - one-time initial setup and configuration <br> Azure portal - day-to-day device management        |-->

## <a name="components"></a>Onderdelen

Deze Data Box Heavy bevat de volgende onderdelen:

* **Data Box Heavy-apparaat**: Een fysiek apparaat met een stevige behuizing waarop gegevens veilig worden opgeslagen. Het apparaat heeft een bruikbare opslagcapaciteit van 800 TB. 

    
* **Data Box-service**: Een extensie in Azure Portal waarmee u een Data Box Heavy-apparaat kunt beheren via een webinterface waartoe u toegang hebt vanaf verschillende geografische locaties. U kunt de Data Box-service gebruiken voor het dagelijkse beheer van uw Data Box Heavy-apparaat. Met de service kunt u taken uitvoeren zoals orders maken en beheren, waarschuwingen bekijken en beheren en shares beheren.  

* **Lokale webinterface**: een webinterface die wordt gebruikt voor het configureren van het apparaat, zodat u verbinding kunt maken met het lokale netwerk en het apparaat bij de Data Box-service kunt registreren. Gebruik de lokale webinterface om het apparaat uit te schakelen of opnieuw op te starten, logboeken met kopieerbewerkingen te bekijken en contact op te nemen met Microsoft Ondersteuning om een serviceaanvraag in te dienen.


## <a name="the-workflow"></a>De werkstroom

Een typische stroom bestaat uit de volgende stappen:

1. **Bestellen**: plaats een bestelling in Azure Portal en geef de verzendingsgegevens en het Azure-doelopslagaccount voor uw gegevens op. Wanneer het apparaat beschikbaar is, bereidt Azure het voor en verstuurt Azure het met volgnummer.

2. **Ontvangen**: Nadat het apparaat wordt geleverd, sluit u de opgegeven netwerk- en voedingskabels aan. Zet het apparaat aan en sluit het aan. Configureer de netwerkinstellingen voor het apparaat en koppel bestandsshares op de hostcomputer van waar u gegevens wilt kopiëren.

3. **Gegevens kopiëren**: Kopieer gegevens naar de shares van Data Box Heavy.

4. **Retourneren**: Bereid het apparaat voor, zet het uit en stuur het terug naar het Azure-datacenter.

5. **Uploaden**: De gegevens worden automatisch van het apparaat naar Azure gekopieerd. De schijven in het apparaat worden veilig gewist overeenkomstig de richtlijnen van het National Institute of Standards and Technology.

Tijdens dit proces wordt u via e-mail op de hoogte gesteld van alle statuswijzigingen. 

## <a name="region-availability"></a>Beschikbaarheid in regio’s

Data Box Heavy kan bestanden overgedragen op basis van de regio waarin de service is geïmplementeerd, het land waar het apparaat naar wordt verzonden, en het Azure-opslagaccount waarnaar de gegevens worden overgebracht. 

- **Beschikbaarheid van service**: Voor deze release is de Data Box Heavy-resource beschikbaar in de volgende regio’s:
    - Alle openbare cloud-regio's in de Verenigde Staten: US - west-centraal, US - west 2, US - west, US - zuid-centraal, US - centraal, US - noord-centraal, US - oost en US - oost 2.
    - Europese Unie: Europa - west en Europa - noord.
    - Verenigd Koninkrijk: UK - zuid en UK - west.
    - Frankrijk: Frankrijk - centraal en Frankrijk - zuid.

- **Doelopslagaccounts**: De opslagaccounts waarin de gegevens worden opgeslagen, zijn beschikbaar in alle Azure-regio’s waar de service beschikbaar is. 

## <a name="sign-up"></a>Aanmelden

Data Box Heavy is in de preview-fase en u moet zich registreren. Voer de volgende stappen uit om u te registreren voor Data Box Heavy:

1. Meld u aan bij Azure Portal op: https://aka.ms/azuredatabox.
2. Klik op **+** om een nieuwe resource te maken. Zoek naar **Azure Data Box**. Selecteer de **Azure Data Box**-service.

    <!--![The Data Box Heavy sign up 1]()-->

3. Klik op **Create**.

    <!--![The Data Box Heavy sign up 2]()-->

4. Kies het abonnement dat u wilt gebruiken voor de preview-versie van Data Box Heavy. Selecteer de regio waar u de Data Box Heavy-resource wilt implementeren. Klik bij de optie **Data Box Heavy** op **Registreren**.

   <!--![The Data Box Heavy sign up 3]()-->

5. Beantwoord de vragen met betrekking tot in welk land de gegevens zich bevinden, het tijdskader, de Azure-doelservice voor gegevensoverdracht, de netwerkbandbreedte en de frequentie van gegevensoverdracht. Lees de privacyverklaring en voorwaarden en schakel het selectievakje in zodat Microsoft uw e-mailadres kan gebruiken om contact met u op te nemen.

    <!--![The Data Box Heavy sign up 4]()-->

Nadat u zich hebt ingeschreven en aan de preview mag meedoen, kunt u een Data Box Heavy bestellen.




