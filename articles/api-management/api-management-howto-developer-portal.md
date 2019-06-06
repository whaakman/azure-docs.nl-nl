---
title: Benaderen en aanpassen van de nieuwe portal voor ontwikkelaars - Azure API Management | Microsoft Docs
description: Informatie over het gebruik van de nieuwe portal voor ontwikkelaars in API Management.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2019
ms.author: apimpm
ms.openlocfilehash: deb3fdf3069aaad4982d71806c209fe516e3c3d6
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743573"
---
# <a name="access-and-customize-the-new-developer-portal-in-azure-api-management"></a>Benaderen en aanpassen van de nieuwe portal voor ontwikkelaars in Azure API Management

In dit artikel wordt beschreven hoe u toegang tot de portal voor ontwikkelaars van nieuwe Azure API Management. Dit helpt u bij de visuele editor-ervaring - toevoegen en bewerken van inhoud, evenals aanpassen van het uiterlijk van de website.

![Nieuwe API Management-ontwikkelaarsportal](media/api-management-howto-developer-portal/cover.png)

## <a name="prerequisites"></a>Vereisten

- Voltooi de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
- Importeren en publiceren van een Azure API Management-exemplaar. Zie voor meer informatie, [importeren en publiceren](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

> [!NOTE]
> De nieuwe portal voor ontwikkelaars is momenteel in preview.

## <a name="managed-and-self-hosted-versions"></a>Beheerde en zelf-hostend versies

U kunt uw developer-portal op twee manieren maken:

- **Beheerde versie** - door te bewerken en aanpassen van de portal ingebouwd in uw exemplaar van API Management en zijn toegankelijk via de URL `<your-api-management-instance-name>.developer.azure-api.net`.
- **Zelf-hostend versie** - door het implementeren en zelf die als host fungeert van de portal buiten een exemplaar van API Management. Deze aanpak kunt u van de portal bewerken codebasis en uitbreiden van de opgegeven Kernfunctionaliteit. Raadpleeg voor meer informatie en instructies, de [GitHub-opslagplaats met de broncode van de portal][1].

## <a name="access-the-managed-version-of-the-portal"></a>Toegang tot de beheerde versie van de portal

Volg de stappen hieronder om de toegang tot de beheerde versie van de portal.

1. Ga naar uw API Management service-exemplaar in de Azure-portal.
1. Klik op de **nieuwe developer-portal (preview)** knop in de bovenste navigatiebalk. Een nieuw browsertabblad met een administratieve versie van de portal wordt geopend. Als u toegang wilt de portal voor het eerst tot krijgen, wordt de Standaardinhoud automatisch ingericht.

## <a name="edit-and-customize-the-managed-version-of-the-portal"></a>Bewerken en de beheerde versie van de portal aanpassen

In de onderstaande video laten we zien hoe u de inhoud van de portal bewerken, uiterlijk van de website aanpassen en de wijzigingen te publiceren.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de nieuwe portal voor ontwikkelaars:

- [GitHub-opslagplaats met de broncode][1]
- [Instructies voor het zelf die als host fungeert van de portal][2]
- [Openbare roadmap van het project][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects