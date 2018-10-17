---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: include
ms.custom: include file
ms.date: 09/27/2018
ms.author: diberry
ms.openlocfilehash: 420757c5b7f6d19bb0abe87232e75497b7b97322
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019082"
---
1. Open Visual Studio 2017 Community Edition.
1. Maak een nieuw **consoletoepassingsproject (.NET Core)** en noem het project `QnaMakerQuickstart`. Accepteer de standaardwaarden voor de overige instellingen.
1. Klik in Solution Explorer met de rechtermuisknop op de projectnaam, **QnaMakerQuickstart** en selecteer dan **NuGet-pakketten beheren...**.
1. Selecteer **Browser** in het NuGet-venster en zoek dan naar **Newtonsoft.JSON**. Installeer het pakket. Dit pakket wordt gebruikt voor het parseren van de JSON die in het QnaMaker-HTTP-antwoord is geretourneerd. 