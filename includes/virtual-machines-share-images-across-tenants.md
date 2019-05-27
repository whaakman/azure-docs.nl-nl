---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 499aeccdf00980eeb66ac6ee06e45267fd515143
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145891"
---
Gedeelde Afbeeldingsgalerieën kunt u installatiekopieën van de share met RBAC. U kunt RBAC gebruiken voor het delen van afbeeldingen in uw tenant, en zelfs personen buiten uw tenant. Maar als u wilt delen van installatiekopieën buiten uw Azure-tenant, op schaal, moet u de registratie van een app delen in het kader maken.  Met behulp van een app-registratie kunt complexere scenario's voor delen, zoals: 

* Gedeelde afbeeldingen beheren wanneer een bedrijf een andere verkrijgt, en de Azure-infrastructuur is verdeeld over afzonderlijke tenants. 
* Azure-Partners beheren Azure-infrastructuur namens hun klanten. Aanpassing van afbeeldingen wordt gedaan binnen de tenant partners, maar de implementaties van de infrastructuur gebeurt in de tenant van de klant. 


## <a name="create-the-app-registration"></a>Maken van de app-registratie

Maak de registratie van een toepassing die door beide tenants worden gebruikt om de installatiekopie van galerie-resources te delen.
1. Open de [App-registraties (preview) in Azure portal](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType//sourceType/).    
1. Selecteer **registratie van nieuwe** in het menu aan de bovenkant van de pagina.
1. In **naam**, type *myGalleryApp*.
1. In **ondersteund accounttypen**, selecteer **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts**.
1. In **omleidings-URI**, type *https://www.microsoft.com* en selecteer vervolgens **registreren**. Nadat de app-registratie is gemaakt, wordt de overzichtspagina wordt geopend.
1. Kopieer op de overzichtspagina de **(client) toepassings-ID** en later opslaan voor gebruik.   
1. Selecteer **certificaten en geheimen**, en selecteer vervolgens **nieuwe clientgeheim**.
1. In **beschrijving**, type *gedeelde installatiekopie galerie cross-tenant-app-geheim*.
1. In **verloopt**, laat de standaardwaarde **In 1 jaar** en selecteer vervolgens **toevoegen**.
1. Kopieer de waarde van de geheime sleutel en sla deze op een veilige plaats. U kunt deze niet ophalen nadat u deze pagina verlaten.


De app-registratie machtigen om te gebruiken de galerie met installatiekopieën van de gedeelde wilt opgeven.
1. Selecteer in de Azure-portal, de gedeelde galerie met installatiekopieën die u wilt delen met een andere tenant.
1. Selecteer **Selecteer toegangsbeheer (IAM)**, en klikt u onder **roltoewijzing toevoegen** Selecteer *toevoegen*. 
1. Onder **rol**, selecteer **lezer**.
1. Onder **toegang toewijzen aan:**, laat u dit als **Azure AD-gebruiker, groep of service-principal**.
1. Onder **Selecteer**, type *myGalleryApp* en dit selecteren wanneer deze weergegeven in de lijst wordt. Wanneer u klaar bent, selecteert u **opslaan**.


## <a name="give-tenant-2-access"></a>Tenant-2-toegang

2 van de Tenant toegang geven tot de toepassing door aan te vragen een aanmelding met een browser. Vervang *<Tenant2 ID>* met de tenant-ID voor de tenant die u wilt uw galerie met installatiekopieën met delen. Vervang *< (client) toepassings-ID >* met toepassings-ID van de appregistratie van de die u hebt gemaakt. Wanneer u klaar bent voor het maken van de vervangingen, plak de URL in een browser en volg de aanwijzingen aanmelden voor Meld u aan bij de Tenant-2.

```
https://login.microsoftonline.com/<Tenant 2 ID>/oauth2/authorize?client_id=<Application (client) ID>&response_type=code&redirect_uri=https%3A%2F%2Fwww.microsoft.com%2F 
```

In de [Azure-portal](https://portal.azure.com) zich aanmelden als Tenant 2 en geeft toegang tot de registratie van de app naar de resourcegroep waar u de virtuele machine wilt maken.

1. Selecteer de resourcegroep en selecteer vervolgens **toegangsbeheer (IAM)**. Onder **roltoewijzing toevoegen** Selecteer **toevoegen**. 
1. Onder **rol**, type **Inzender**.
1. Onder **toegang toewijzen aan:**, laat u dit als **Azure AD-gebruiker, groep of service-principal**.
1. Onder **Selecteer** type *myGalleryApp* selecteren wanneer deze weergegeven in de lijst wordt. Wanneer u klaar bent, selecteert u **opslaan**.

> [!NOTE]
> U moet wachten tot de versie van de installatiekopie wordt gemaakt en gerepliceerd voordat u dezelfde beheerde installatiekopie kunt maken van een andere versie van de installatiekopie volledig te voltooien.

