---
author: jluk
ms.service: cloud-shell
ms.topic: persist-storage
ms.date: 9/7/2018
ms.author: juluk
ms.openlocfilehash: a66f5ca9501d09f2ef89f421191f617c177e10eb
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52886210"
---
# <a name="persist-files-in-azure-cloud-shell"></a>Bestanden in Azure Cloud Shell behouden
Cloudshell gebruikmaakt van Azure File storage om bestanden in verschillende sessies persistent te maken. Op de eerste start vraagt Cloud Shell u om te koppelen van een nieuwe of bestaande bestandsshare om bestanden in verschillende sessies persistent te maken.

> [!NOTE]
> Maken Bash en PowerShell delen dezelfde bestandsshare. Slechts één bestandsshare kan worden gekoppeld aan het automatisch koppelen in Cloud Shell.

## <a name="create-new-storage"></a>Maken van nieuwe opslag

Wanneer u basisinstellingen en selecteert u alleen een abonnement, maakt Cloud Shell drie resources uit uw naam in de ondersteunde regio die het dichtst bij u:
* Resourcegroep: `cloud-shell-storage-<region>`
* Storage-account: `cs<uniqueGuid>`
* Bestandsshare: `cs-<user>-<domain>-com-<uniqueGuid>`

![De instelling voor Cloudabonnement](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

De bestandsshare koppelt als `clouddrive` in uw `$Home` directory. Dit is een eenmalige actie en de bestandsshare koppelt automatisch in de volgende sessies. 

> [!NOTE]
> Voor beveiliging, moet elke gebruiker hun eigen opslagaccount inrichten.  Voor op rollen gebaseerd toegangsbeheer (RBAC), moeten gebruikers Inzender-toegang hebben of boven op de opslag account niveau.

De bestandsshare bevat ook een installatiekopie van 5 GB die automatisch voor u die gemaakt wordt zich blijft voordoen gegevens in uw `$Home` directory. Dit geldt voor zowel Bash als PowerShell.

## <a name="use-existing-resources"></a>Gebruik bestaande bronnen

U kunt bestaande resources koppelen met behulp van de geavanceerde optie. U moet een back-ups opslagaccount CO-locatie in dezelfde regio selecteren bij het selecteren van een Cloud Shell-regio. Als uw toegewezen regio VS-West dan moet u bijvoorbeeld een bestandsshare die zich in VS-West ook bevinden koppelen.

Wanneer de opslag-setup-prompt wordt weergegeven, selecteert u **geavanceerde instellingen weergeven** om extra opties weer te geven. Het filter van de opties ingevuld opslag voor lokaal redundante opslag (LRS), geografisch redundante opslag (GRS) en accounts van de zone-redundante opslag (ZRS). 

> [!NOTE]
> Met GRS of ZRS worden storage-accounts aanbevolen voor extra tolerantie voor de bestandsshare van de back-ups. Welk type redundantie, is afhankelijk van uw doelstellingen en prijs voorkeur. [Meer informatie over replicatieopties voor Azure Storage-accounts](https://docs.microsoft.com/azure/storage/common/storage-redundancy#choosing-a-replication-option).

![De instelling van de Resource](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

### <a name="supported-storage-regions"></a>Ondersteunde storage-regio 's
Gekoppelde Azure storage-accounts moeten bevinden zich in dezelfde regio als de Cloud Shell-machine die u hebt te koppelen. Om te vinden van uw huidige regio die u kunt uitvoeren `env` in Bash en zoekt u de variabele `ACC_LOCATION`. Bestandsshares ontvangen een 5 GB-installatiekopie gemaakt voor u om vast te leggen uw `$Home` directory.

Cloud Shell machines bevinden zich in de volgende regio's:
|Onderwerp|Regio|
|---|---|
|Noord- en Zuid-Amerika|VS-Oost, VS Zuid-centraal, VS-West|
|Europa|Europa - noord, Europa - west|
|Azië en Stille Oceaan|India-centraal, Zuidoost-Azië|

## <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Beperken van het maken van resources aan een Azure-resource-beleid
Storage-accounts die u in de Cloud Shell maakt worden gemarkeerd met `ms-resource-usage:azure-cloud-shell`. Als u wilt weigeren van gebruikers van het storage-accounts maken in Cloud Shell, maakt u een [Azure resourcebeleid voor tags](../articles/azure-policy/json-samples.md) die worden geactiveerd door deze specifieke tag.
