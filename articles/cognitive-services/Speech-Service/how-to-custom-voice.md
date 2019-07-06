---
title: Wat is aangepaste spraak? -Spraakservices
titlesuffix: Azure Cognitive Services
description: Aangepaste spraak is een set online hulpprogramma's waarmee u kunt maken van een herkenbare, één van een unieke stem voor uw merk. Alles die wat u nodig om te beginnen zijn een paar van de audio-bestanden en de bijbehorende transcripties. Volg de koppelingen hieronder om te beginnen met het maken van een aangepaste spraak-naar-tekst-ervaring.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: ed5428da28ebea8b1c7f925696f88e3f3f898942
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606699"
---
# <a name="get-started-with-custom-voice"></a>Aan de slag met Custom Voice

Aangepaste spraak is een set online hulpprogramma's waarmee u kunt maken van een herkenbare, één van een unieke stem voor uw merk. Alles die wat u nodig om te beginnen zijn een paar van de audio-bestanden en de bijbehorende transcripties. Volg de koppelingen hieronder om te beginnen met het maken van een aangepaste Text to Speech-ervaring.

## <a name="whats-in-custom-voice"></a>Wat is er in aangepaste gesproken?

Voordat u begint met aangepaste spraak, moet u een Azure-account en een abonnement Speech Services. Als u een account hebt gemaakt, kunt u uw gegevens voorbereiden, trainen en testen van uw modellen stem kwaliteit evalueren en uiteindelijk het implementatiemodel van uw aangepaste spraak.

In het onderstaande diagram ziet u de stappen voor het maken van een aangepaste spraak-model met behulp van de aangepaste spraak-portal. Gebruik de koppelingen voor meer informatie.

![Architectuurdiagram van aangepaste spraak](media/custom-voice/custom-voice-diagram.png)

1.  [Abonneren en maak een project](#set-up-your-azure-account) : Maak een Azure-account en maakt u een abonnement Speech Services. Deze geïntegreerde abonnement geeft u toegang tot spraak-naar-tekst, Text to Speech, spraakomzetting en de aangepaste spraak-portal. Met behulp van uw abonnement Speech Services, maak vervolgens uw eerste aangepaste spraak-project.

2.  [Gegevens uploaden](how-to-custom-voice-create-voice.md#upload-your-datasets) -gegevens (audio en tekst) uploaden met behulp van de aangepaste spraak-portal of aangepaste spraak-API. U kunt onderzoeken en evalueren uitspraak van scores en signaal ruis ratio's vanuit de portal. Zie voor meer informatie, [over het voorbereiden van gegevens voor aangepaste spraak](how-to-custom-voice-prepare-data.md).

3.  [Uw model te trainen](how-to-custom-voice-create-voice.md#build-your-custom-voice-model) – uw gegevens gebruiken om een aangepaste tekst-naar-spraak-model te maken. U kunt een model in verschillende talen te trainen. Na de training, uw model testen en als u tevreden met het resultaat bent, kunt u het model implementeren.

4.  [Uw model implementeert](how-to-custom-voice-create-voice.md#create-and-use-a-custom-voice-endpoint) : een aangepast eindpunt voor uw stem-model maken en deze gebruiken voor spraaksynthese in uw producten, -hulpprogramma's en toepassingen.

## <a name="set-up-your-azure-account"></a>Instellen van uw Azure-account

Een abonnement Speech Services is vereist voordat u de portal voor aangepaste spraak gebruiken kunt om een aangepast model te maken. Volg deze instructies voor het maken van een abonnement Speech Services in Azure. Als u een Azure-account hebt, kunt u zich aanmelden voor een nieuwe resourcegroep.  

Als u een Azure-account en een Speech Services-abonnement hebt gemaakt, moet u zich aanmelden bij de portal voor aangepaste spraak en verbinding maken met uw abonnement.

1. Uw abonnementssleutel Speech Services ophalen uit de Azure portal.
2. Aanmelden bij de [aangepaste spraak portal](https://aka.ms/custom-voice).
3. Selecteer uw abonnement en een spraak-project maken.
4. Als u wilt overschakelen naar een andere spraak-abonnement, gebruikt u het tandwielpictogram zich in de bovenste navigatiebalk.

> [!NOTE]
> De aangepaste spraak-service biedt geen ondersteuning voor de sleutel 30 dagen gratis proefversie. U moet hebben een F0 of een S0-sleutel in Azure hebt gemaakt voordat u de service kunt gebruiken.

## <a name="how-to-create-a-project"></a>Een project maken

Inhoud, zoals gegevens, modellen, tests en eindpunten zijn ingedeeld in **projecten** in de portal voor aangepaste spraak. Elk project is specifiek voor een land/de taal en het geslacht van de stem die u wilt maken. U kunt bijvoorbeeld een project voor een vrouw stem voor uw callcenter chat bots die gebruik de Engelse versie in de Verenigde Staten (en-US) maken.

Voor het maken van uw eerste project, selecteer de **Text-naar-Speech/aangepaste spraak** tabblad en klik vervolgens op **nieuw Project**. Volg de instructies van de wizard om uw project te maken. Nadat u een project hebt gemaakt, ziet u vier tabbladen: **Gegevens**, **Training**, **testen**, en **implementatie**. Gebruik de koppelingen [Vervolgstappen](#next-steps) voor informatie over het gebruik van elk tabblad.

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste spraak-gegevens voorbereiden](how-to-custom-voice.md)
- [Maken van een aangepaste spraak](how-to-custom-voice-create-voice.md)
- [Handleiding voor het: Registreren van uw stem-voorbeelden](record-custom-voice-samples.md)
