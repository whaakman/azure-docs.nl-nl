---
title: Azure VMware-oplossing door CloudSimple Quickstart - service maken
description: Meer informatie over het maken van de service CloudSimple, kopen van knooppunten en knooppunten reserveren
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9b3b95db24f4b0f9a0cf8f5102dfeea5dc51e29f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577568"
---
# <a name="quickstart---create-service"></a>Quick Start - service maken

Om te beginnen, maakt u de Azure-VMware-oplossing door CloudSimple in Azure portal.

## <a name="vmware-solution-by-cloudsimple---service-overview"></a>VMware-oplossing door CloudSimple - Service-overzicht

De service CloudSimple kunt u Azure VMware-oplossing door CloudSimple gebruiken.  Het maken van de service, kunt u knooppunten kopen, knooppunten reserveren en persoonlijke clouds te maken.  U toevoegen de service CloudSimple aan elke Azure-regio waar de service CloudSimple beschikbaar is.  De service definieert het edge-netwerk van Azure VMware-oplossing door CloudSimple.  Dit Microsoft edge-netwerk wordt gebruikt voor VPN en ExpressRoute Internet verbinding met uw persoonlijke clouds onder meer services.

Als u wilt toevoegen de CloudSimple-service, moet u een gatewaysubnet maken. Het gatewaysubnet wordt gebruikt bij het maken van het edge-netwerk en vereist een/28 CIDR-blok. De adresruimte van de gateway-subnet moet uniek zijn. Deze mogen niet overlappen met een van uw on-premises netwerk adresruimten of de adresruimte van Azure-netwerk.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="enable-microsoftvmwarecloudsimple-resource-provider"></a>Enable Microsoft.VMwareCloudSimple resource provider

Volg onderstaande stappen voor het inschakelen van de resourceprovider voor CloudSimple-service.

1. Selecteer **Alle services**.
2. Zoek en selecteer **abonnementen**.

    ![Abonnementen selecteren](media/cloudsimple-service-select-subscriptions.png)

3. Selecteer het abonnement waarvoor u wilt ervoor zorgen dat CloudSimple
4. Klik op **resourceproviders** voor het abonnement
5. Gebruik **Microsoft.VMwareCloudSimple** voor het filteren van de resourceprovider
6. Selecteer de **Microsoft.VMwareCloudSimple** resourceprovider en klik op **registreren**

    ![Resourceprovider registreren](media/cloudsimple-service-enable-resource-provider.png)

## <a name="create-the-service"></a>De service maken

>[!NOTE]
> CloudSimple service moet zijn ingeschakeld op uw abonnement. Als uw abonnement niet is ingeschakeld, ontvangt u een fout wanneer u probeert om de service te maken.  Volg de stappen in [CloudSimple inschakelen service](https://docs.azure.cloudsimple.com/enable-cloudsimple-service) artikel om in te schakelen van de service.

1. Selecteer **Alle services**.
2. Zoeken naar **CloudSimple Service**.

    ![Search CloudSimple Service](media/create-cloudsimple-service-search.png)

3. Selecteer **CloudSimple Services**.
4. Klik op **toevoegen** om een nieuwe service te maken.

    ![CloudSimple Service toevoegen](media/create-cloudsimple-service-add.png)

5. Selecteer het abonnement waar u de service CloudSimple maken.
6. Selecteer de resourcegroep voor de service. Als u wilt een nieuwe resourcegroep toevoegen, klikt u op **nieuw**.
7. Voer naam in om de service te identificeren.
8. De CIDR invoert voor de service-gateway. Geef een/28 subnet dat geen overlap heeft met een van uw on-premises subnetten, Azure-subnets of geplande CloudSimple subnetten. U kunt de CIDR niet wijzigen nadat de service is gemaakt.

    ![Het maken van de service CloudSimple](media/create-cloudsimple-service.png)

9. Klik op **OK**.

De service is gemaakt en toegevoegd aan de lijst met services.

## <a name="purchase-nodes"></a>Kopen van knooppunten

Als u betaalt u Ga capaciteit voor een omgeving CloudSimple Private Cloud instelt, eerst knooppunten in te richten in Azure portal.

1. Selecteer **Alle services**.
2. Zoeken naar **CloudSimple knooppunten**.

    ![Search CloudSimple knooppunten](media/create-cloudsimple-node-search.png)

3. Selecteer **CloudSimple knooppunten**.
4. Klik op **toevoegen** knooppunten te maken.

    ![CloudSimple knooppunten toevoegen](media/create-cloudsimple-node-add.png)

5. Selecteer het abonnement waar u om aan te schaffen CloudSimple knooppunten.
6. Selecteer de resourcegroep voor de knooppunten. Als u wilt een nieuwe resourcegroep toevoegen, klikt u op **nieuw**.
7. Geef het voorvoegsel voor het identificeren van de knooppunten.
8. Selecteer de locatie voor de resources van het knooppunt.
9. Selecteer de specifieke locatie voor het hosten van de resources van het knooppunt.
10. Selecteer het knooppunttype. U kunt ervoor kiezen de [optie CS28 of CS36](cloudsimple-node.md). De laatste optie bevat de maximale capaciteit voor Computing en geheugen.
11. Selecteer het aantal knooppunten om in te richten.
12. Selecteer **Controleren + maken**.
13. Controleer de instellingen. Als u wilt de instellingen wijzigen, klikt u op **vorige**.
14. Selecteer **Maken**.

## <a name="next-steps"></a>Volgende stappen

* [Private Cloud maken en configureren van omgeving](quickstart-create-private-cloud.md)
* Meer informatie over [CloudSimple service](https://docs.azure.cloudsimple.com/cloudsimple-service)
