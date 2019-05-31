---
title: Symantec ICDX gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u verbinding maken met gegevens van de Symantec ICDX Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2019
ms.author: rkarlin
ms.openlocfilehash: 3b21371d6321b208b19ca8b2524308736c3ceca9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244347"
---
# <a name="connect-your-symantec-icdx-appliance"></a>Verbinding maken met uw Symantec ICDX-apparaat 

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Symantec ICDX connector kunt u eenvoudig alle uw beveiligingslogboeken voor Symantec verbinding te maken met uw Azure Sentinel, dashboards weergeven, aangepaste waarschuwingen maken en onderzoek te verbeteren. Dit biedt u meer inzicht in het netwerk van uw organisatie en verbetert uw mogelijkheden voor beveiliging bewerking. Integratie tussen Symantec ICDX en Azure Sentinel maakt gebruik van REST-API.


> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

## <a name="configure-and-connect-symantec-icdx"></a>Configureren en verbinden van Symantec ICDX 

Symantec ICDX kunt integreren en logboeken exporteren rechtstreeks naar Azure Sentinel.

1. Open de beheerconsole van ICDX om toe te voegen doorstuurservers Microsoft Azure Sentinel (Log Analytics).
2. Klik op de navigatiebalk ICDx **configuratie**. 
3. Aan de bovenkant van de **configuratie** scherm, klikt u op **doorstuurservers**.
4. Onder **doorstuurservers**, naast de Microsoft Azure Sentinel (Log Analytics), klikt u op **toevoegen**. 
4. In de **Microsoft Azure Sentinel (Log Analytics)** venster, klikt u op **weergeven geavanceerde**. 
5. Aan de bovenkant van het uitgevouwen naar het venster Microsoft Azure Sentinel (Log Analytics) en het volgende doen:
    -   **Naam**: Typ een naam voor de doorstuurserver niet meer dan 30 tekens. Kies een unieke, betekenisvolle naam. Deze naam wordt weergegeven in de lijst met doorstuurservers op de **configuratie** scherm en in de dashboards in de **Dashboard** scherm. Bijvoorbeeld: Microsoft Azure Log Analytics-Oost. Dit veld is vereist.
    -   **Beschrijving**: Typ een beschrijving voor de doorstuurserver. Deze beschrijving wordt ook weergegeven in de lijst met doorstuurservers op de **configuratie** scherm. Meer informatie, zoals het gebeurtenistype wordt doorgestuurd en de groep die nodig heeft om te controleren van de gegevens bevatten.
    -   **Opstarttype**: Selecteer de methode van het opstarten van de configuratie van de doorstuurserver. Uw opties zijn handmatig en automatisch.<br>De standaardwaarde is automatisch. 
6. Onder **gebeurtenissen**, doet u het volgende: 
    - **Bron**: Selecteer een of meer archieven waaruit gebeurtenissen worden doorgestuurd. Kunt u actieve collector archieven (met inbegrip van het algemene archief), zwevende collector archieven (dat wil zeggen, archieven voor de collectors die u hebt verwijderd), ICDx ontvanger archieven of het systeem-archief. <br>De standaardwaarde is algemene archief.
      > [!NOTE]
      > ICDx ontvanger archieven worden afzonderlijk weergegeven met de naam. 
 
    - **Filter**: Toevoegen van een filter op dat Hiermee geeft u de subset met gebeurtenissen om door te sturen. Voer een van de volgende handelingen uit:
        - Als u wilt een filtervoorwaarde selecteren, klikt u op een Type, kenmerk, Operator en waarde. 
        - Controleer in het filterveld in uw filtervoorwaarde. U kunt deze rechtstreeks in het veld te bewerken of verwijderen indien nodig.
        - Klik op en of of toe te voegen aan de filtervoorwaarde voor uw.
        - U kunt ook klikken op query's opgeslagen als een opgeslagen query wilt toepassen.
    - **Opgenomen kenmerken**: Typ de door komma's gescheiden lijst van kenmerken om op te nemen in de doorgestuurde gegevens. De opgenomen kenmerken hebben voorrang op de uitgesloten kenmerken.
    - **Uitgesloten kenmerken**: Typ de door komma's gescheiden lijst van kenmerken moeten worden uitgesloten van de doorgestuurde gegevens.
    - **Batchgrootte**: Selecteer het aantal gebeurtenissen per batch verzenden. Uw opties zijn 10, 50, 100, 500 en 1000.<br>De standaardwaarde is 100. 
    - **Tarieflimiet**: Selecteer de snelheid waarmee gebeurtenissen worden doorgestuurd, uitgedrukt als gebeurtenissen per seconde. Uw opties zijn onbeperkt, 500, 1000 en 5000, 10000. <br> De standaardwaarde is 5000. 
7. Onder **Azure bestemming**, doet u het volgende: 
    - **Werkruimte-ID**: Plak de werkruimte-ID hieronder. Dit veld is vereist.
    - **Primaire sleutel**: Plak de Primary Key hieronder. Dit veld is vereist.
    - **Aangepaste logboeknaam**: Typ de naam van het aangepaste logboek in de portal Log Analytics-werkruimte van Microsoft Azure, waarmee u vooruit gebeurtenissen wilt. De standaardwaarde is SymantecICDx. Dit veld is vereist.
8. Klik op *opslaan* om de configuratie van de doorstuurserver te voltooien. 
9. Starten van de doorstuurserver onder **opties**, klikt u op **meer** en vervolgens **Start**.
10. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Symantec ICDX gebeurtenissen, **SymantecICDX_CL**.


## <a name="validate-connectivity"></a>Verbinding valideren

Het duurt al twintig minuten tot de logboeken in Log Analytics wordt weergegeven. 



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Symantec ICDX Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

