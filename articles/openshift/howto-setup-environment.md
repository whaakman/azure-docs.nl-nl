---
title: Uw Azure Red Hat OpenShift-ontwikkelomgeving instellen | Microsoft Docs
description: Hier volgen de vereisten voor het werken met Microsoft Azure Red Hat OpenShift.
services: openshift
keywords: Red hat openshift installatie instellen
author: jimzim
ms.author: jzim
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: container-service
manager: jeconnoc
ms.openlocfilehash: a31655e8c8805505bdcc5e90bf25191590d35c18
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672524"
---
# <a name="set-up-your-azure-red-hat-openshift-dev-environment"></a>Een Azure Red Hat OpenShift-ontwikkelaarsomgeving instellen

Als u wilt bouwen en uitvoeren van toepassingen van Microsoft Azure Red Hat OpenShift, moet u het:

* Kopen van gereserveerde virtuele Azure-machine-instanties.
* Versie 2.0.65 installeren (of hoger) van de Azure CLI (of gebruik de Azure Cloud Shell).
* Meld u aan voor de `AROGA` functie en de gekoppelde resourceproviders.
* Een Azure Active Directory (Azure AD)-tenant maken.
* Maak een Azure AD-toepassing-object.
* Maak een Azure AD-gebruiker.

De volgende instructies begeleidt u bij al deze vereisten.

## <a name="purchase-azure-red-hat-openshift-application-nodes-reserved-instances"></a>Koop gereserveerde instanties van Azure Red Hat OpenShift toepassing knooppunten

Voordat u Azure Red Hat OpenShift gebruiken kunt, moet u een minimum van 4 Azure Red Hat OpenShift gereserveerde knooppunten van toepassingen, waarna u wordt mogelijk clusters inrichten kopen.

Als u een Azure-klant bent, [Azure Red Hat OpenShift gereserveerde instanties kopen](https://aka.ms/openshift/buy) via Azure portal. Na de aankoop, wordt uw abonnement binnen 24 uur zijn geactiveerd.

Als u niet een Azure-klant, [Neem contact op met verkoop](https://aka.ms/openshift/contact-sales) en vul het formulier de omzet in aan de onderkant van de pagina om het proces te starten.

Raadpleeg de [Azure Red Hat OpenShift pagina met prijzen](https://aka.ms/openshift/pricing) voor meer informatie.

## <a name="install-the-azure-cli"></a>Azure-CLI installeren

Azure Red Hat OpenShift vereist versie 2.0.65 of hoger van Azure CLI. Als u de Azure CLI al hebt geïnstalleerd, kunt u controleren welke versie u door te voeren hebt:

```bash
az --version
```

De eerste regel van de uitvoer heeft de CLI-versie, bijvoorbeeld `azure-cli (2.0.65)`.

Hier vindt u instructies voor [installeren van de Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) als u een nieuwe installatie of een upgrade nodig hebt.

U kunt ook kunt u de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). Wanneer u de Azure Cloud Shell gebruikt, moet u selecteren de **Bash** omgeving als u van plan bent om te volgen we de [maken en beheren van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) reeks.

## <a name="register-providers-and-features"></a>Registreren van providers en -functies

De `Microsoft.ContainerService AROGA` functie `Microsoft.Solutions`, `Microsoft.Compute`, `Microsoft.Storage`, `Microsoft.KeyVault` en `Microsoft.Network` providers moeten worden geregistreerd bij uw abonnement handmatig voordat u uw eerste Azure Red Hat OpenShift-cluster implementeert.

Gebruik voor het handmatig registreren van deze voorzieningen en functies, de volgende instructies zijn van een Bash-shell als u de CLI hebt geïnstalleerd, of van de sessie van de Azure Cloud Shell (Bash) in uw Azure-portal:

1. Als u meerdere Azure-abonnementen hebt, geeft u de relevante abonnements-ID:

    ```bash
    az account set --subscription <SUBSCRIPTION ID>
    ```

1. De functie Microsoft.ContainerService AROGA registreren:

    ```bash
    az feature register --namespace Microsoft.ContainerService -n AROGA
    ```

1. Registreer de provider Microsoft.Storage:

    ```bash
    az provider register -n Microsoft.Storage --wait
    ```
    
1. Registreren van de Microsoft.Compute-provider:

    ```bash
    az provider register -n Microsoft.Compute --wait
    ```

1. Registreer de provider Microsoft.Solutions:

    ```bash
    az provider register -n Microsoft.Solutions --wait
    ```

1. Registreer de Microsoft.COMPUTE-provider:

    ```bash
    az provider register -n Microsoft.Network --wait
    ```

1. De Microsoft.KeyVault-provider hebt geregistreerd:

    ```bash
    az provider register -n Microsoft.KeyVault --wait
    ```

1. De registratie van de resourceprovider Microsoft.ContainerService vernieuwen:

    ```bash
    az provider register -n Microsoft.ContainerService --wait
    ```

## <a name="create-an-azure-active-directory-azure-ad-tenant"></a>Een Azure Active Directory (Azure AD)-tenant maken

De service Azure Red Hat OpenShift vereist een gekoppelde Azure Active Directory (Azure AD)-tenant die uw organisatie en de relatie naar Microsoft voorstelt. Uw Azure AD-tenant kunt u registreren, maken, en apps beheren, evenals andere Azure-services gebruiken.

Als u een Azure AD hebt gebruiken als de tenant voor uw Azure Red Hat OpenShift-cluster of u een tenant maken wilt voor het testen, volg de instructies in [maken van een Azure AD-tenant voor uw cluster Azure Red Hat OpenShift](howto-create-tenant.md) voordat Met deze handleiding wordt voortgezet.

## <a name="create-an-azure-ad-user-security-group-and-application-object"></a>Maak een Azure AD-gebruiker, beveiligingsgroep en toepassing object

Azure Red Hat OpenShift vereist machtigingen voor het uitvoeren van taken in uw cluster, zoals het configureren van opslag. Deze machtigingen worden vertegenwoordigd door een [service-principal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object). Moet u een nieuwe Active Directory-gebruiker maken voor het testen van apps die worden uitgevoerd op uw cluster Azure Red Hat OpenShift.

Volg de instructies in [maken van een object voor Azure AD-app en gebruiker](howto-aad-app-configuration.md) voor het maken van een service-principal genereren van een client secret en verificatie URL voor terugbellen voor uw app en maak een nieuwe Azure AD-beveiligingsgroep en de gebruiker toegang tot de cluster.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar voor gebruik van Azure Red Hat OpenShift!

Raadpleeg de zelfstudie:
> [!div class="nextstepaction"]
> [Een Azure Red Hat OpenShift-cluster maken](tutorial-create-cluster.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
