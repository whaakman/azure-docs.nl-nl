---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/11/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 020e59f029b09f3c7656f67039731e4141e68d31
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61270191"
---
Als Python een fout optreedt tijdens het starten van uw toepassing wordt alleen een eenvoudige foutpagina weergegeven (bijvoorbeeld) geretourneerd Worden "de pagina kan niet weergegeven omdat een interne serverfout opgetreden.").

Om vast te leggen Python-toepassingsfouten:

1. Selecteer in de Azure portal, in uw web-app, **instellingen**.
2. Op de **instellingen** tabblad **toepassingsinstellingen**.
3. Onder **App-instellingen**, voer de volgende sleutel/waarde-paar:
    * Sleutel: WSGI_LOG
    * Waarde: D:\home\site\wwwroot\logs.txt (Geef uw keuze van bestandsnaam)

U ziet nu de fouten in het bestand logs.txt in de wwwroot-map.
