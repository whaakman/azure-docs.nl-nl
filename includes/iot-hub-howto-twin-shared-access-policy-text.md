---
title: bestand opnemen
description: bestand opnemen
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 07/17/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: f97ab0a0bb91af90daaf60c5c2c85f0ede56a5e7
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68403942"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-twin-getstarted.md files-->

In dit artikel maakt u een back-end-service waarmee gewenste eigenschappen worden toegevoegd aan een apparaat en wordt vervolgens een query uitgevoerd op het id-REGI ster om alle apparaten te zoeken met gerapporteerde eigenschappen die dienovereenkomstig zijn bijgewerkt. Uw service moet beschikken over de machtiging voor het maken van een **service** om de gewenste eigenschappen van een apparaat te wijzigen, en de machtiging lezen van het **REGI ster** is vereist voor het opvragen van het identiteits register. Er is geen standaard beleid voor gedeelde toegang dat alleen deze twee machtigingen bevat, dus u moet er een maken.
