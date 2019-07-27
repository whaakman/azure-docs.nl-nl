---
title: Een model voor Custom Speech-Speech-Service implementeren
titleSuffix: Azure Cognitive Services
description: In dit document leert u hoe u een eind punt maakt en implementeert met behulp van de Custom Speech Portal.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 15e2ce437a746d15622dc4e093a63c87448f100d
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559675"
---
# <a name="deploy-a-custom-model"></a>Aangepaste model implementeren

Nadat u gegevens hebt geüpload en geinspectet, de nauw keurigheid hebt geëvalueerd en een aangepast model hebt getraind, kunt u een aangepast eind punt implementeren voor gebruik met uw apps, hulpprogram ma's en producten. In dit document leert u hoe u een eind punt maakt en implementeert met behulp van de Custom Speech Portal.

## <a name="create-a-custom-endpoint"></a>Een aangepast eind punt maken

Als u een nieuw aangepast eind punt wilt maken, selecteert u **implementatie** in het menu Custom speech boven aan de pagina. Als dit de eerste keer is dat u uitvoert, ziet u dat er geen eind punten in de tabel staan. Nadat u een eind punt hebt gemaakt, gebruikt u deze pagina om elk geïmplementeerd eind punt bij te houden.

Selecteer vervolgens **eind punt toevoegen** en voer een **naam** en **Beschrijving** in voor het aangepaste eind punt. Selecteer vervolgens het aangepaste model dat u aan dit eind punt wilt koppelen. Op deze pagina kunt u ook logboek registratie inschakelen. Met logboek registratie kunt u eindpunt verkeer bewaken. Als deze is uitgeschakeld, wordt verkeer niet opgeslagen.

![Een model implementeren](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> Vergeet niet om akkoord te gaan met de gebruiks voorwaarden en prijs gegevens.

Selecteer vervolgens **maken**. Met deze actie keert u terug naar de **implementatie** pagina. De tabel bevat nu een vermelding die overeenkomt met uw aangepaste eind punt. De status van het eind punt toont de huidige status. Het kan tot 30 minuten duren om een nieuw eind punt te instantiëren met uw aangepaste modellen. Wanneer de status van de implementatie verandert in **voltooid**, is het eind punt klaar voor gebruik.

Nadat het eind punt is geïmplementeerd, wordt de naam van het eind punt weer gegeven als een koppeling. Klik op de koppeling om informatie weer te geven die specifiek is voor uw eind punt, zoals de eindpunt sleutel, eind punt-URL en voorbeeld code.

## <a name="view-logging-data"></a>Logboek gegevens weer geven

Logboek gegevens kunnen worden gedownload onder **eindpunt > Details**.

## <a name="next-steps"></a>Volgende stappen

* Uw aangepaste eind punt gebruiken met de [Speech SDK](speech-sdk.md)

## <a name="additional-resources"></a>Aanvullende resources

* [Uw gegevens voorbereiden en testen](how-to-custom-speech-test-data.md)
* [Uw gegevens controleren](how-to-custom-speech-inspect-data.md)
* [Uw gegevens evalueren](how-to-custom-speech-evaluate-data.md)
* [Uw model trainen](how-to-custom-speech-train-model.md)
* [Uw model implementeren](how-to-custom-speech-deploy-model.md)
