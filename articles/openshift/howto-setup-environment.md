---
title: Uw Azure Red Hat OpenShift-ontwikkelomgeving instellen | Microsoft Docs
description: Hier volgen de vereisten voor het werken met Microsoft Azure Red Hat OpenShift.
services: openshift
keywords: Red hat openshift installatie instellen
author: TylerMSFT
ms.author: twhitney
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: openshift
manager: jeconnoc
ms.openlocfilehash: 3c265d6695af7ba1bc5833db59966a626cb29cb9
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65416078"
---
# <a name="set-up-your-azure-red-hat-openshift-dev-environment"></a>Uw Azure Red Hat OpenShift ontwikkelaarsomgeving instellen

Als u wilt bouwen en uitvoeren van toepassingen van Microsoft Azure Red Hat OpenShift, moet u het:

* Kopen van gereserveerde virtuele Azure-machine-instanties.
* Versie 2.0.64 installeren (of hoger) van de Azure CLI (of gebruik de Azure Cloud Shell).
* Meld u aan voor de `openshiftmanagedcluster` functie en de gekoppelde resourceproviders.
* Een Azure Active Directory (Azure AD)-tenant maken.
* Maak een Azure AD-toepassing-object.
* Maak een Azure AD-gebruiker.

De volgende instructies begeleidt u bij al deze vereisten.

## <a name="purchase-azure-virtual-machine-reserved-instances"></a>Kopen van gereserveerde virtuele Azure-machine-instanties

Voordat u Azure Red Hat OpenShift gebruiken kunt, moet u kopen van gereserveerde virtuele Azure-machine-instanties.

Als u een Azure-klant bent, hier van hoe u [aankoop virtuele machine van Azure gereserveerde instanties](https://aka.ms/openshift/buy). Een reservering vermindert uw uitgaven vooraf betalen voor volledig beheerde Azure-services. Raadpleeg [ *wat zijn Azure-reserveringen* ](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) voor meer informatie over reserveringen en hoe ze u geld besparen.

Als u niet een Azure-klant, [Neem contact op met verkoop](https://aka.ms/openshift/contact-sales) en vul het formulier de omzet in aan de onderkant van de pagina om het proces te starten.

## <a name="install-the-azure-cli"></a>Azure-CLI installeren

Azure Red Hat OpenShift vereist versie 2.0.64 of hoger van Azure CLI. Als u de Azure CLI al hebt geïnstalleerd, kunt u controleren welke versie u door te voeren hebt:

```bash
az --version
```

De eerste regel van de uitvoer heeft de CLI-versie, bijvoorbeeld `azure-cli (2.0.64)`.

Hier vindt u instructies voor [installeren van de Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) als u een nieuwe installatie of een upgrade nodig hebt.

U kunt ook kunt u de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). Wanneer u de Azure Cloud Shell gebruikt, moet u selecteren de **Bash** omgeving als u van plan bent om te volgen we de [maken en beheren van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) reeks.

## <a name="register-providers-and-features"></a>Registreren van providers en -functies

De `Microsoft.ContainerService openshiftmanagedcluster` functie `Microsoft.Solutions`, en `Microsoft.Network` providers moeten worden geregistreerd bij uw abonnement handmatig voordat u uw eerste Azure Red Hat OpenShift-cluster implementeert.

Gebruik voor het handmatig registreren van deze voorzieningen en functies, de volgende instructies zijn van een Bash-shell als u de CLI hebt geïnstalleerd, of van de sessie van de Azure Cloud Shell (Bash) in uw Azure-portal:.
1. Als u meerdere Azure-abonnementen hebt, geeft u de relevante abonnements-ID:

    ```bash
    az account set --subscription <SUBSCRIPTION ID>
    ```

2. De functie van de openshiftmanagedcluster Microsoft.ContainerService registreren:

    ```bash
    az feature register --namespace Microsoft.ContainerService -n openshiftmanagedcluster
    ```

3. Registreer de provider Microsoft.Solutions:

    ```bash
    az provider register -n Microsoft.Solutions --wait
    ```

4. Registreer de Microsoft.COMPUTE-provider:

    ```bash
    az provider register -n Microsoft.Network --wait
    ```

5. De Microsoft.KeyVault-provider hebt geregistreerd:

    ```bash
    az provider register -n Microsoft.KeyVault --wait
    ```

6. De registratie van de resourceprovider Microsoft.ContainerService vernieuwen:

    ```bash
    az provider register -n Microsoft.ContainerService --wait
    ```

## <a name="create-an-azure-active-directory-azure-ad-tenant"></a>Een Azure Active Directory (Azure AD)-tenant maken

De service Azure Red Hat OpenShift vereist een gekoppelde Azure Active Directory (Azure AD)-tenant die uw organisatie en de relatie naar Microsoft voorstelt. Uw Azure AD-tenant kunt u registreren, maken, en apps beheren, evenals andere Azure-services gebruiken.

Als u een Azure AD hebt gebruiken als de tenant voor uw Azure Red Hat OpenShift-cluster of u een tenant maken wilt voor het testen, volg de instructies in [maken van een Azure AD-tenant voor uw cluster Azure Red Hat OpenShift](howto-create-tenant.md) voordat Met deze handleiding wordt voortgezet.

## <a name="create-an-azure-ad-application-object-and-user"></a>Maak een Azure AD-toepassing-object en de gebruiker

Azure Red Hat OpenShift vereist machtigingen voor het uitvoeren van taken in uw cluster, zoals het configureren van opslag. Deze machtigingen worden vertegenwoordigd door een [service-principal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) en worden gemaakt wanneer u een Azure AD-toepassing voor de werkbelasting die u van plan bent voor het hosten op Azure Red Hat OpenShift registreert. Moet u een nieuwe Active Directory-gebruiker maken voor het testen van apps die worden uitgevoerd op uw cluster Azure Red Hat OpenShift.

Volg de instructies in [maken van een object voor Azure AD-app en gebruiker](howto-aad-app-configuration.md) voor meer informatie over het maken van een service-principal, genereren van een client secret en verificatie URL voor terugbellen voor uw app en maak een nieuwe Active Directory-gebruiker voor het testen.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar voor gebruik van Azure Red Hat OpenShift!

Raadpleeg de zelfstudie:
> [!div class="nextstepaction"]
> [Een Azure Red Hat OpenShift-cluster maken](tutorial-create-cluster.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli