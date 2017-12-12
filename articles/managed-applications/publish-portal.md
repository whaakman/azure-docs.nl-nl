---
title: Een Azure publiceren voor beheerde toepassing via portal | Microsoft Docs
description: Laat zien hoe u de Azure portal om te maken van een Azure-toepassing die is bedoeld voor leden van uw organisatie worden beheerd.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: 764eb479733a7d4acdb6e6c3eee721cb4a161c88
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Een servicetoepassing catalogus via Azure portal publiceren

U kunt de Azure portal gebruiken voor het publiceren van [beheerde toepassingen](overview.md) die bestemd zijn voor leden van uw organisatie. Een IT-afdeling kan bijvoorbeeld beheerde toepassingen die ervoor zorgen aan de organisatie-standaarden dat publiceren. Deze beheerde toepassingen zijn beschikbaar via de Servicecatalogus, niet de Azure marketplace.

## <a name="prerequisites"></a>Vereisten

Als een beheerde toepassing publiceert, geeft u een identiteit voor het beheren van de resources. Het is raadzaam om dat u een Azure Active Directory gebruikersgroep opgeven. Voor het maken van een Azure Active Directory-gebruikersgroep, Zie [een groep maken en leden toevoegen in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md). 

Het ZIP-bestand met de definitie van de beheerde toepassing moet beschikbaar zijn via een URI. Het is raadzaam om uw ZIP-bestand te uploaden naar een blob storage. 

## <a name="create-managed-application-with-portal"></a>Beheerde toepassing maken met portal

1. Selecteer in de linkerbovenhoek **+ nieuw**.

   ![Nieuwe service](./media/publish-portal/new.png)

1. Zoeken naar **Servicecatalogus**.

1. Schuif in de resultaten, totdat u **toepassing servicedefinitie catalogus-beheerd**. Selecteer deze.

   ![Zoeken naar definities van de beheerde toepassing](./media/publish-portal/select-managed-apps-definition.png)

1. Selecteer **maken** om het proces van het maken van definitie van de beheerde toepassing te starten.

   ![Definitie van de beheerde toepassing maken](./media/publish-portal/create-definition.png)

1. Geef waarden op voor de naam, weergavenaam, beschrijving, locatie, abonnement en resourcegroep. Geef het pad naar het zip-bestand dat u hebt gemaakt voor pakketbestand URI.

   ![Geef waarden op](./media/publish-portal/fill-application-values.png)

1. Als u de sectie verificatie en vergrendelingsniveau, selecteer **toevoegen autorisatie**.

   ![Autorisatie toevoegen](./media/publish-portal/add-authorization.png)

1. Selecteer een Azure Active Directory-groep voor het beheren van de resources en selecteer **OK**.

   ![Autorisatie groep toevoegen](./media/publish-portal/add-auth-group.png)

1. Wanneer u alle waarden hebt opgegeven, selecteert u **maken**.

   ![beheerde toepassing maken](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van beheerde toepassingen](overview.md) voor een inleiding tot beheerde toepassingen.
* Zie voor voorbeelden van beheerde toepassingsservices [voorbeeldprojecten voor Azure beheerde toepassingen](sample-projects.md).
* Zie [Aan de slag met CreateUiDefinition](create-uidefinition-overview.md) voor meer informatie over het maken van een UI-definitiebestand voor een beheerde toepassing.