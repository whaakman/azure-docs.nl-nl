---
title: Azure CLI gebruiken om een beleidstoewijzing te maken om niet-compatibele resources te identificeren in uw Azure-omgeving | Microsoft Docs
description: PowerShell gebruiken om een Azure Policy-toewijzing te maken om niet-compatibele resources te identificeren.
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 6ea39618a24249d92b77afdf5cb0ea284b180223
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Een beleidstoewijzing maken om niet-compatibele resources te identificeren in uw Azure-omgeving met Azure CLI

Om naleving in Azure te begrijpen, moet u eerst weten hoe het er voorstaat met uw eigen huidige resources. In deze Quick Start gaat u een beleidstoewijzing maken voor de identificatie van virtuele machines die geen beheerde schijven gebruiken.

Als u het proces helemaal hebt doorlopen, hebt u virtuele machines geïdentificeerd die geen beheerde schijven gebruiken, en daarom *niet compatibel* zijn.
.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze quickstart de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="opt-in-to-azure-policy"></a>Registreren voor Azure Policy

Azure Policy is nu beschikbaar in openbare preview en u moet zich registreren om toegang aan te vragen.

1. Ga naar Azure Policy op https://aka.ms/getpolicy en selecteer **Aanmelden** in het linkerdeelvenster.

   ![Zoeken naar beleid](media/assign-policy-definition/sign-up.png)

2. Registreer u voor Azure Policy door in de lijst **Abonnement** de abonnementen te selecteren waarmee u wilt werken. Selecteer vervolgens **Registreren**.

   ![Registreren voor het gebruik van Azure Policy](media/assign-policy-definition/preview-opt-in.png)

   Uw aanvraag wordt automatisch goedgekeurd voor preview. Het duurt maximaal 30 minuten voordat uw registratie door het systeem is verwerkt.

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze Quick Start maken we een beleidstoewijzing en wijzen we de definitie Virtuele machines zonder beheerde schijven controleren toe. Deze beleidsdefinitie identificeert resources die niet voldoen aan de voorwaarden die zijn vastgelegd in de beleidsdefinitie.

Volg deze stappen als u een nieuwe beleidstoewijzing wilt maken.

Bekijk alle beleidsdefinities en zoek naar de beleidsdefinitie Virtuele machines zonder beheerde schijven controleren:

```azurecli
az policy definition list
```

Azure Policy wordt geleverd met ingebouwde beleidsdefinities die u kunt gebruiken. U ziet ingebouwde beleidsdefinities, zoals:

- Tag en waarde afdwingen
- Tag en waarde toepassen
- SQL Server-versie 12.0 vereisen

Verstrek de volgende informatie en voer de volgende opdracht uit om de beleidsdefinitie toe te wijzen:

- **Weergavenaam** voor de beleidstoewijzing. In dit geval gebruiken we *Virtuele machines zonder beheerde schijven controleren*.
- **Beleid**: dit is de beleidsdefinitie, op basis waarvan u de toewijzing maakt. In dit geval is het de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren*
- Het **bereik**: het bereik bepaalt op welke resources of groep resources de beleidstoewijzing wordt afgedwongen. Dit kan variëren van een abonnement tot resourcegroepen.

  Gebruik het abonnement (of de resourcegroep) die u eerder hebt geregistreerd toen u zich aanmeldde voor Azure Policy. In dit voorbeeld gebruiken we de volgende abonnements-id: **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** en de naam van de resourcegroep: **FabrikamOMS**. Zorg ervoor dat u de id wijzigt in de id van het abonnement en de naam in de naam van de resourcegroep waarmee u werkt.

De opdracht moet er als volgt uitzien:

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

   Als er bestaande resources zijn die niet compatibel zijn met deze nieuwe toewijzing, worden deze weergegeven op het tabblad **Niet-compatibele resources**, zoals u hierboven ziet.

## <a name="clean-up-resources"></a>Resources opschonen

Andere handleidingen in deze verzameling zijn op deze Quick Start gebaseerd. Als u door wilt gaan met andere zelfstudies, verwijdert u de resources die u in deze Quick Start hebt gemaakt, niet. Als u niet van plan bent om door te gaan, verwijdert u de toewijzing die u hebt gemaakt door de volgende opdracht uit te voeren:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een beleidsdefinitie toegewezen om niet-compatibele resources in uw Azure-omgeving te identificeren.

Voor meer informatie over het toewijzen van beleid om ervoor te zorgen dat resources die u in de **toekomst** gaat maken compatibel zijn, gaat u verder met de zelfstudie voor:

> [!div class="nextstepaction"]
> [Beleid maken en beheren](./create-manage-policy.md)
