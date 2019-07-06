---
title: Een model implementeren voor aangepaste spraak - spraakservices
titlesuffix: Azure Cognitive Services
description: In dit document leert u hoe u kunt maken en implementeren van een eindpunt met behulp van de aangepaste spraak-portal.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: f2b69bf3e5d536e7e813ef4a7c36b2937141fd43
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606629"
---
# <a name="deploy-a-custom-model"></a>Aangepaste model implementeren

Nadat u hebt geüpload en inspectie van gegevens, nauwkeurigheid geëvalueerd en een aangepast model wordt getraind, kunt u een aangepast eindpunt voor gebruik met uw apps, -tools en producten kunt implementeren. In dit document leert u hoe u kunt maken en implementeren van een eindpunt met behulp van de aangepaste spraak-portal.

## <a name="create-a-custom-endpoint"></a>Maken van een aangepast eindpunt

Voor het maken van een nieuw aangepast eindpunt selecteert **implementatie** in het menu voor aangepaste spraak-bovenaan de pagina. Als dit de eerste keer uitvoert, zult u merken dat er geen eindpunten die worden vermeld in de tabel zijn. Nadat u een eindpunt hebt gemaakt, gebruikt u deze pagina om bij te houden van elke geïmplementeerde eindpunt.

Selecteer vervolgens **eindpunt toevoegen** en voer een **naam** en **beschrijving** voor uw aangepast eindpunt. Selecteer vervolgens het aangepaste model dat u wilt koppelen aan dit eindpunt. U kunt ook logboekregistratie inschakelen op deze pagina. Logboekregistratie kunt u voor het bewaken van de eindpunt-verkeer. Als uitgeschakeld, kunt u verkeer wordt niet opgeslagen.

![Een model implementeren](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> Vergeet niet om de voorwaarden van gebruik en de prijsinformatie te accepteren.

Selecteer vervolgens **maken**. Deze actie gaat u terug naar de **implementatie** pagina. De tabel bevat nu een vermelding die overeenkomt met uw aangepast eindpunt. Status van het eindpunt bevat de huidige status. Het kan instantiëren van een nieuw eindpunt met behulp van uw aangepaste modellen tot 30 minuten duren. Wanneer de status van de implementatie is gewijzigd in **voltooid**, het eindpunt kan worden gebruikt.

Nadat het eindpunt is geïmplementeerd, wordt de naam van het eindpunt als een koppeling weergegeven. Klik op de koppeling om specifieke informatie aan uw eindpunt, zoals de eindpuntsleutel, de eindpunt-URL en de voorbeeldcode weer te geven.

## <a name="view-logging-data"></a>Logboekgegevens weergeven

Gegevens voor logboekregistratie is beschikbaar als download onder **eindpunt > Details**.

## <a name="next-steps"></a>Volgende stappen

* Gebruik uw aangepaste eindpunt met de [spraak SDK](speech-sdk.md)

## <a name="additional-resources"></a>Aanvullende resources

* [Voorbereiden en testen van uw gegevens](how-to-custom-speech-test-data.md)
* [Controleer uw gegevens](how-to-custom-speech-inspect-data.md)
* [Evalueren van uw gegevens](how-to-custom-speech-evaluate-data.md)
* [Uw model te trainen](how-to-custom-speech-train-model.md)
* [Uw model implementeren](how-to-custom-speech-deploy-model.md)
