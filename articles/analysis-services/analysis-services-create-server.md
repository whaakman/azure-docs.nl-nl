---
title: Een Analysis Services-server maken in Azure | Microsoft Docs
description: Informatie over het maken van een exemplaar van Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c910416524f149c785aae299d576ca5c521abc6d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a>Een Azure Analysis Services-server maken in Azure portal
Dit artikel begeleidt u bij het maken van een resource van Analysis Services-server in uw Azure-abonnement.

## <a name="before-you-begin"></a>Voordat u begint
U hebt het volgende nodig om deze quickstart te voltooien:

* **Azure-abonnement**: ga naar [gratis proefversie van Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) om een account te maken.
* **Azure Active Directory**: uw abonnement moet worden gekoppeld aan een Azure Active Directory-tenant. En u moet zijn aangemeld bij Azure met een account in dat Azure Active Directory. Microsoft-accounts worden niet ondersteund. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md).
* **Resourcegroep**: een resourcegroep die u al hebt gebruikt of [Maak een nieuwe](../azure-resource-manager/resource-group-overview.md).

> [!NOTE]
> Het maken van een server kan zorgen voor een nieuwe factureerbare service. Zie [Prijzen van Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/) voor meer informatie.
> 
> 

## <a name="to-create-a-server-in-the-azure-portal"></a>Een server in de Azure portal maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Klik op **+ nieuw** > **gegevens en analyse** > **Analysis Services**.
3. In de **Analysis Services** blade Vul de vereiste velden in en druk vervolgens op **maken**.
   
    ![Server maken](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * **Servernaam**: Typ een unieke naam die wordt gebruikt om te verwijzen naar de server.
   * **Abonnement**: Selecteer het abonnement dat deze server stuklijsten aan.
   * **Resourcegroep**: deze containers zijn ontworpen om u te helpen bij het beheren van een verzameling Azure-resources. Zie voor meer informatie, [resourcegroepen](../azure-resource-manager/resource-group-overview.md).
   * **Locatie**: locatie van deze Azure-datacenter als host fungeert voor de server. Kies een locatie dichtstbijzijnde uw grootste gebruikersgroep.
   * **Prijscategorie**: een prijscategorie selecteren. Tabellaire modellen tot 400 GB worden ondersteund. Zie voor meer informatie, [Azure Analysis Services-prijzen](https://azure.microsoft.com/pricing/details/analysis-services/).
4. Klik op **Create**.

Maak gewoonlijk duurt minder dan een minuut; vaak slechts enkele seconden. Als u hebt geselecteerd **toevoegen aan de Portal**, navigeer naar uw portal om te zien van de nieuwe server. Of Ga naar **alle services** > **Analysis Services** om te zien als de server klaar is.

 ![Dashboard](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Volgende stappen
Nadat u uw server hebt gemaakt, kunt u [implementeert een model](analysis-services-deploy.md) toe met behulp van SSDT of met SSMS.

Als een model dat u op uw server implementeert verbinding met on-premises gegevensbronnen, moet u voor het installeren van een [On-premises gegevensgateway](analysis-services-gateway.md) op een computer in uw netwerk.

