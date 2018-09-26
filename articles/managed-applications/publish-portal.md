---
title: Publiceren van een door Azure beheerde toepassing via de portal | Microsoft Docs
description: Laat zien hoe u de Azure portal om te maken van een Azure-toepassing die is bedoeld voor leden van uw organisatie worden beheerd.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: e52acd8587203c4729ac2bcd6e4bbc09620ead86
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096033"
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Een servicecatalogustoepassing via Azure-portal publiceren

U kunt de Azure-portal gebruiken om te publiceren [beheerde toepassingen](overview.md) die zijn bedoeld voor leden van uw organisatie. Zo kan een IT-afdeling beheerde toepassingen publiceren die zorgen voor naleving van organisatiestandaarden. Deze beheerde toepassingen zijn beschikbaar via de servicecatalogus, niet Azure Marketplace.

## <a name="prerequisites"></a>Vereisten

Wanneer een beheerde toepassing publiceert, geeft u een identiteit voor het beheren van de resources. Het is raadzaam om dat u een Azure Active Directory-gebruikersgroep opgeven. Voor het maken van een Azure Active Directory-gebruikersgroep, Zie [een groep maken en leden toevoegen in Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). 

Het ZIP-bestand met de definitie van beheerde toepassing moet beschikbaar zijn via een URI zijn. Het is raadzaam dat u het ZIP-bestand naar een blob storage uploaden. 

## <a name="create-managed-application-with-portal"></a>Beheerde toepassing maken met portal

1. Selecteer in de linkerbovenhoek **+ nieuw**.

   ![Nieuwe service](./media/publish-portal/new.png)

1. Zoeken naar **Servicecatalogus**.

1. Schuif in de resultaten, totdat u **beheerde toepassingsdefinitie voor de Servicecatalogus**. Selecteer deze.

   ![Zoeken naar definities van beheerde toepassing](./media/publish-portal/select-managed-apps-definition.png)

1. Selecteer **maken** om het proces voor het maken van de definitie van beheerde toepassing te starten.

   ![Definitie van beheerde toepassing maken](./media/publish-portal/create-definition.png)

1. Geef waarden op voor de naam, weergavenaam, beschrijving, locatie, abonnement en resourcegroep. Voor de URI van pakketbestand, geeft u het pad naar het zip-bestand dat u hebt gemaakt.

   ![Geef waarden op](./media/publish-portal/fill-application-values.png)

1. Wanneer u op de verificatie- en vergrendelingsniveau sectie, selecteer **autorisatie toevoegen**.

   ![Autorisatie toevoegen](./media/publish-portal/add-authorization.png)

1. Selecteer een Azure Active Directory-groep voor het beheren van de resources en selecteer **OK**.

   ![Autorisatie-groep toevoegen](./media/publish-portal/add-auth-group.png)

1. Wanneer u alle waarden hebt opgegeven, selecteert u **maken**.

   ![Een beheerde toepassing maken](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van beheerde toepassingen](overview.md) voor een inleiding tot beheerde toepassingen.
* Zie voor voorbeelden van beheerde toepassing [voorbeeldprojecten voor Azure beheerde toepassingen](sample-projects.md).
* Zie [Aan de slag met CreateUiDefinition](create-uidefinition-overview.md) voor meer informatie over het maken van een UI-definitiebestand voor een beheerde toepassing.