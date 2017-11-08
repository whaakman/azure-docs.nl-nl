---
title: StorSimple Manager servicedashboard | Microsoft Docs
description: Beschrijving van het servicedashboard StorSimple Manager-en wordt uitgelegd hoe u hiermee de status van uw StorSimple-oplossing.
services: storsimple
documentationcenter: 
author: SharS
manager: carmonm
editor: 
ms.assetid: fb0f131d-d60b-45d7-ace2-56d0502e6627
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 62273c0093876f136d97eedf5a509f0b306a1379
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-dashboard"></a>Gebruik het servicedashboard StorSimple Manager
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. De versie van dit artikel voor de nieuwe Azure portal, Ga naar [gebruik het servicedashboard StorSimple Manager-](storsimple-8000-service-dashboard.md). Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Overzicht
De dashboardpagina van de StorSimple Manager-service geeft een overzicht van de apparaten die zijn verbonden met de StorSimple Manager-service, die aandacht vereisen een systeembeheerder is gemarkeerd. Deze zelfstudie maakt u kennis met de dashboardpagina, wordt uitgelegd van het Dashboardinhoud en de functie en beschrijft de taken die u op deze pagina uitvoeren kunt.

![Servicedashboard](./media/storsimple-service-dashboard/HCS_ServiceDashboard.png)

Het servicedashboard StorSimple Manager-bevat de volgende informatie:

* In de **grafiek** gebied kunt u de grafiek relevante metrische gegevens voor uw apparaten bekijken. U kunt de primaire opslag (lokaal vastgemaakt en lagen) weergeven gebruikt via alle apparaten, evenals de cloudopslag verbruikt door apparaten gedurende een periode. De opties in de rechterbovenhoek van de grafiek gebruiken om op te geven van een schaal 1 week, 1 maand, 3 maanden of 1 jaar.
* De **overzicht gebruik** ziet u de primaire opslag die is ingericht en gebruikt door alle apparaten ten opzichte van de totale opslagruimte beschikbaar op alle apparaten. **Ingericht** verwijst naar de hoeveelheid opslagruimte die is voorbereid en toegewezen voor gebruik, terwijl **gebruikt** verwijst naar informatie over het gebruik van volumes, zoals weergegeven door de initiators die zijn verbonden met de apparaten.
* De **waarschuwingen** gebied biedt een momentopname van de actieve waarschuwingen op alle apparaten gegroepeerd op de ernst van waarschuwing. Hiermee opent u de ernst op te klikken op de **waarschuwingen** pagina gericht op deze waarschuwingen weergeven. Op de **waarschuwingen** pagina, klikt u op een afzonderlijke waarschuwing om aanvullende informatie over deze waarschuwing, met inbegrip van alle aanbevolen acties weer te geven. U kunt ook de waarschuwing wissen als het probleem is opgelost.
* De **taken** gebied biedt een momentopname van recente taken op alle apparaten die zijn verbonden met uw service. Er zijn koppelingen die u gebruiken kunt om te kijken naar taken die momenteel worden uitgevoerd, die is mislukt in de afgelopen 24 uur of die zijn gepland in de volgende 24 uur.
* De **snelle weergave** gebied biedt nuttige informatie zoals de status van service aantal apparaten die zijn verbonden met de service, de locatie van de service en de details van het abonnement dat is gekoppeld aan de service. Er is ook een koppeling naar de operations-logboek. Klik op de koppeling om een lijst met alle voltooide StorSimple Manager servicebewerkingen.

U kunt de dashboardpagina van de StorSimple Manager-service starten van de volgende taken:

* Weergeven of de service registratie-sleutel opnieuw genereren.
* De gegevensversleutelingssleutel van service wijzigen.
* Bekijk de bewerkingslogboeken.

## <a name="view-or-regenerate-the-service-registration-key"></a>Weergeven of de service registratie-sleutel opnieuw genereren
De serviceregistratiesleutel wordt gebruikt voor een Microsoft Azure StorSimple-apparaat registreren bij de StorSimple Manager-service, zodat het apparaat wordt weergegeven in de klassieke Azure portal voor verdere acties. De sleutel is gemaakt op het eerste apparaat en gedeeld met de rest van uw apparaten.

Te klikken op **registratiesleutel** (aan de onderkant van de pagina) Hiermee opent u de **Serviceregistratiesleutel** in het dialoogvenster waarin u kunt de huidige serviceregistratiesleutel naar het Klembord kopiëren of opnieuw genereren van de serviceregistratiesleutel.

Opnieuw genereren van de sleutel heeft geen invloed op eerder geregistreerde apparaten: is van invloed op alleen de apparaten die zijn geregistreerd bij de service nadat de sleutel wordt opnieuw gegenereerd.

Ga voor meer informatie over het weergeven en het genereren van de serviceregistratiesleutel naar [de serviceregistratiesleutel ophalen](storsimple-manage-service.md#get-the-service-registration-key).

## <a name="change-the-service-data-encryption-key"></a>De gegevensversleutelingssleutel van service wijzigen
Versleutelingssleutels voor service-gegevens worden gebruikt voor het versleutelen van de vertrouwelijke klantgegevens, zoals opslagaccountreferenties, die van uw StorSimple Manager-service worden verzonden naar het StorSimple-apparaat. U moet deze sleutels regelmatig te wijzigen als uw IT-organisatie een beleid voor belangrijke draaien op de opslagapparaten heeft. Het proces voor sleutel kan zijn enigszins verschillen afhankelijk van of er is een apparaat met één of meerdere apparaten, beheerd door de StorSimple Manager-service.

Het wijzigen van de gegevensversleutelingssleutel van service is een proces 3-stappen:

1. Met de klassieke Azure portal, de autorisatie voor een apparaat wijzigen van de gegevensversleutelingssleutel van service.
2. Met Windows PowerShell voor StorSimple, start de service gegevens versleuteling belangrijke wijziging.
3. Als u meer dan een StorSimple-apparaat hebt, werkt u de gegevensversleutelingssleutel van service op de andere apparaten.

De volgende stappen beschrijven de overschakeling van de procedure voor de gegevensversleutelingssleutel van service.

[!INCLUDE [storsimple-change-data-encryption-key](../../includes/storsimple-change-data-encryption-key.md)]

## <a name="view-the-operations-logs"></a>Bekijk de logboeken met bewerkingen
U kunt de bewerkingslogboeken weergeven door te klikken op de koppeling van bewerking logboeken beschikbaar in de **snelle weergave** deelvenster van het dashboard. Hiermee gaat u naar de servicepagina, kunt u filteren en raadpleegt u de specifieke logboeken met uw StorSimple Manager-service.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [oplossen van een StorSimple-apparaat](storsimple-troubleshoot-operational-device.md).
* Meer informatie over het [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

