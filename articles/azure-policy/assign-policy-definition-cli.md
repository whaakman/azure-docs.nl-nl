---
title: Azure CLI gebruiken om een beleidstoewijzing te maken om niet-compatibele resources te identificeren in uw Azure-omgeving | Microsoft Docs
description: PowerShell gebruiken om een Azure Policy-toewijzing te maken om niet-compatibele resources te identificeren.
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/17/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 76725f3ebeaf5af4f2ab8aadb303d862fa111ecb
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Een beleidstoewijzing maken om niet-compatibele resources te identificeren in uw Azure-omgeving met Azure CLI

De eerste stap in het begrijpen van naleving in Azure is het identificeren van de status van uw resources. In deze quickstart gaat u een beleidstoewijzing maken voor het identificeren van virtuele machines die geen beheerde schijven gebruiken.

Als u dit proces helemaal hebt doorlopen, kunt u virtuele machines identificeren die geen beheerde schijven gebruiken. Deze zijn *niet-compatibel* met de beleidstoewijzing.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Voor deze quickstart moet u versie 2.0.4 of hoger van Azure CLI uitvoeren om de CLI lokaal te installeren en te gebruiken. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze Quick Start maken we een beleidstoewijzing en wijzen we de definitie Virtuele machines zonder beheerde schijven controleren toe. Deze beleidsdefinitie identificeert resources die niet voldoen aan de voorwaarden die zijn vastgelegd in de beleidsdefinitie.

Volg deze stappen als u een nieuwe beleidstoewijzing wilt maken:

1. Registreer de Policy Insights-resourceprovider, zodat u zeker weet dat uw abonnement hiervoor geschikt is. Als u een resourceprovider wilt registreren, moet u toestemming hebben om de registreeractie voor de resourceprovider uit te voeren. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar.

    Voer de volgende opdracht uit om de resourceprovider te registreren:

    ```azurecli
    az provider register --namespace Microsoft.PolicyInsights
    ```

    De opdracht retourneert een bericht waarin wordt gemeld dat de registratie gaande is.

    U kunt de registratie van een resourceprovider niet ongedaan maken terwijl er resourcetypen uit de resourceprovider in uw abonnement aanwezig zijn. Zie [Resourceproviders en -typen](../azure-resource-manager/resource-manager-supported-services.md) voor meer informatie over het registreren en weergeven van resourceproviders.

2. Bekijk alle beleidsdefinities en zoek naar de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren*:

    ```azurecli
az policy definition list
```

    Azure Policy wordt geleverd met ingebouwde beleidsdefinities die u kunt gebruiken. U ziet ingebouwde beleidsdefinities, zoals:

    - Tag en waarde afdwingen
    - Tag en waarde toepassen
    - SQL Server-versie 12.0 vereisen

3. Verstrek de volgende informatie en voer de volgende opdracht uit om de beleidsdefinitie toe te wijzen:

    - **Weergavenaam** voor de beleidstoewijzing. In dit geval gebruiken we *Virtuele machines zonder beheerde schijven controleren*.
    - **Beleid**: dit is de beleidsdefinitie, op basis waarvan u de toewijzing maakt. In dit geval is het de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren*
    - Het **bereik**: het bereik bepaalt op welke resources of groep resources de beleidstoewijzing wordt afgedwongen. Dit kan variëren van een abonnement tot resourcegroepen.

    Gebruik het eerder geregistreerde abonnement (of de eerder geregistreerde resourcegroep). In dit voorbeeld gebruiken we de abonnements-id **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** en de resourcegroepnaam **FabrikamOMS**. Zorg ervoor dat u de id wijzigt in de id van het abonnement en de naam in de naam van de resourcegroep waarmee u werkt.

    De opdracht moet er zo uitzien:

    ```azurecli
az policy assignment create --name Audit Virtual Machines without Managed Disks Assignment --policy Audit Virtual Machines without Managed Disks --scope /subscriptions/
bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

Een beleidstoewijzing is een beleid dat is toegewezen om te worden toegepast binnen een bepaald bereik. Dit bereik kan ook variëren van een beheergroep tot een resourcegroep.

## <a name="identify-non-compliant-resources"></a>Niet-compatibele resources identificeren

De resources weergeven die niet compatibel zijn onder deze nieuwe toewijzing:

1. Ga terug naar de pagina van Azure Policy.
2. Selecteer **Naleving** in het linkerdeelvenster en zoek de **beleidstoewijzing** die u hebt gemaakt.

   ![Beleidsnaleving](media/assign-policy-definition/policy-compliance.png)

   Bestaande resources die niet compatibel zijn met deze nieuwe toewijzing, worden weergegeven op het tabblad **Niet-compatibele resources**. In de voorgaande afbeelding ziet u voorbeelden van niet-compatibele resources.

## <a name="clean-up-resources"></a>Resources opschonen

Andere handleidingen in deze verzameling zijn op deze quickstart gebaseerd. Als u door wilt gaan met andere zelfstudies, verwijdert u de resources die u in deze quickstart hebt gemaakt, niet. Als u niet van plan bent om door te gaan, verwijdert u de toewijzing die u hebt gemaakt door de volgende opdracht uit te voeren:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een beleidsdefinitie toegewezen om niet-compatibele resources in uw Azure-omgeving te identificeren.

Voor meer informatie over het toewijzen van beleid om ervoor te zorgen dat resources die u in de **toekomst** gaat maken compatibel zijn, gaat u verder met de zelfstudie voor:

> [!div class="nextstepaction"]
> [Beleid maken en beheren](./create-manage-policy.md)
