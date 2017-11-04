---
title: De Azure CLI gebruiken voor het maken van een beleidstoewijzing om te identificeren van niet-compatibele bronnen in uw Azure-omgeving | Microsoft Docs
description: PowerShell gebruiken voor het maken van een Azure-beleidstoewijzing om te identificeren van niet-compatibele bronnen.
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 764554a6afcc7912c53fc5000a6af44abb2adc99
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Een beleidstoewijzing om te identificeren van niet-compatibele bronnen in uw Azure-omgeving met de Azure CLI maken

De eerste stap bij de naleving van inzicht in Azure is weten waar u met uw eigen huidige resources staan. Deze snelstartgids begeleidt u door het proces van het maken van een beleidstoewijzing om te identificeren van virtuele machines die geen gebruik van beheerde schijven maakt.

Aan het einde van dit proces voor u wordt hebt is geïdentificeerd welke virtuele machines niet beheerde schijven gebruikt en zijn daarom *niet-compatibele*.
.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="opt-in-to-azure-policy"></a>U meldt zich aan Azure-beleid

Azure-beleid is nu beschikbaar in Public Preview en u wilt registreren bij aanvragen voor toegang.

1. Ga naar de Azure-beleid op https://aka.ms/getpolicy en selecteer **aanmelden** in het linkerdeelvenster.

   ![Zoeken naar beleid](media/assign-policy-definition/sign-up.png)

2. U meldt zich aan Azure-beleid door het selecteren van de abonnementen in de **abonnement** u werken wilt met lijst. Selecteer vervolgens **registreren**.

   ![Aanmelden voor Azure-beleid gebruiken](media/assign-policy-definition/preview-opt-in.png)

   Uw aanvraag is automatisch goedgekeurd voor de Preview. Wacht tot 30 minuten voor het systeem voor het verwerken van uw registratie.

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze snelstartgids we een beleidstoewijzing maken en toewijzen Audit virtuele Machines zonder schijven beheerd definitie. Deze definitie voor identificeert de bronnen die niet aan de voorwaarden in de beleidsdefinitie van het voldoen.

Volg deze stappen voor het maken van een nieuwe beleidstoewijzing.

Weergeven van alle beleidsdefinities en vindt u de definitie van de beleid 'Audit virtuele Machines zonder beheerd-schijven genoemd:

```azurecli
az policy definition list
```

Beleid voor Azure wordt geleverd met al ingebouwd in beleidsdefinities die u kunt gebruiken. Ziet u de beleidsdefinities van de ingebouwde, zoals:

- Afdwingen van code en de bijbehorende waarde
- Label en de waarde ervan toepassen
- SQL Server-versie 12.0 vereisen

Vervolgens de volgende informatie en voer de volgende opdracht om toe te wijzen de definitie voor:

- Weergave **naam** voor de beleidstoewijzing. In dit geval gaan we gebruiken *Audit virtuele Machines zonder schijven beheerd*.
- **Beleid** – dit is de beleidsdefinitie, op basis van uit dat u de toewijzing te maken. In dit geval is de beleidsdefinitie – *Audit virtuele Machines zonder schijven beheerd*
- Een **bereik** : een bereik bepaalt welke resources of groeperen van resources de toewijzing van beleid wordt afgedwongen op. Dit kan variëren van een abonnement aan resourcegroepen.

  Het abonnement (of resourcegroep) u eerder hebt geregistreerd wanneer u mee aan het Azure-beleid gebruiken in dit voorbeeld gebruiken we deze abonnement-ID - **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** en de naam van de resource - **FabrikamOMS**. Zorg ervoor dat deze naar de ID van het abonnement en de naam van de resourcegroep die u met werkt wijzigen.

Dit is wat de opdracht moet eruitzien als:

```azurecli
az policy assignment create --name Audit Virtual Machines without Managed Disks Assignment --policy Audit Virtual Machines without Managed Disks --scope /subscriptions/
bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

Een beleidstoewijzing is een beleid dat is toegewezen aan plaatsvinden binnen een bepaald bereik. Dit bereik kan ook variëren van een beheergroep aan een resourcegroep.

## <a name="identify-non-compliant-resources"></a>Niet-compatibele bronnen identificeren

De resources die niet compatibel onder deze nieuwe toewijzing weergeven:

1. Ga terug naar de pagina Azure-beleid.
2. Selecteer **naleving** in het linkerdeelvenster en zoek naar de **beleidstoewijzing** u hebt gemaakt.

   ![Naleving van beleid](media/assign-policy-definition/policy-compliance.png)

   Als er bestaande resources die niet compatibel met deze nieuwe toewijzing, ze worden weergegeven onder de **niet-compatibel resources** tabblad, zoals hierboven.

## <a name="clean-up-resources"></a>Resources opschonen

Andere handleidingen in deze verzameling bouwen voort op deze snelstartgids. Als u van plan bent om door te gaan werken met de volgende zelfstudies, geen clean up maakt van de resources in deze snelstartgids hebt gemaakt. Als u niet van plan bent om door te gaan, verwijdert u de toewijzing die u hebt gemaakt met deze opdracht uit te voeren:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids toegewezen u de beleidsdefinitie van een voor het identificeren van niet-compatibele bronnen in uw Azure-omgeving.

Voor meer informatie over het toewijzen van beleid om ervoor te zorgen dat resources u maakt de **toekomstige** compatibel zijn, blijven de zelfstudie voor:

> [!div class="nextstepaction"]
> [Maken en beheren van beleid](./create-manage-policy.md)
