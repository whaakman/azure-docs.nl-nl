---
title: Toegang tot Azure VMware oplossing door CloudSimple - Portal
description: Beschrijft hoe u VMware-oplossing voor toegang door CloudSimple portal van Azure-portal
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8c7bb080b350742d0722cdb4e07b82a6881ba05b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073671"
---
# <a name="accessing-the-vmware-solution-by-cloudsimple-portal-from-azure-portal"></a>Toegang tot de VMware-oplossing door CloudSimple portal van Azure-portal

Eenmalige aanmelding wordt ondersteund voor toegang tot de portal CloudSimple. Nadat u zich bij Azure portal aanmelden, kunt u de toegang tot de portal CloudSimple zonder meldt u zich opnieuw. De eerste keer dat u toegang tot de CloudSimple portal u wordt gevraagd om te machtigen de [CloudSimple Service autorisatie](#consent-to-cloudsimple-service-authorization-application) toepassing.  Autorisatie is een eenmalige bewerking.

## <a name="before-you-begin"></a>Voordat u begint

Alleen gebruikers met ingebouwde **eigenaar** en **Inzender** rollen kunnen CloudSimple portal openen.  De rollen die moeten worden geconfigureerd voor het abonnement.  Zie voor meer informatie over het controleren van uw rol [weergeven roltoewijzingen](https://docs.microsoft.com/azure/role-based-access-control/check-access) artikel.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Toegang tot de CloudSimple-portal

1. Selecteer **Alle services**.

2. Zoeken naar **CloudSimple Services**.

3. Selecteer de CloudSimple-service op die u wilt maken van uw Privécloud.

4. Op de **overzicht** pagina, klikt u op **gaat u naar de portal CloudSimple**.  Als u toegang wilt de portal CloudSimple vanuit de Azure-portal voor het eerst tot krijgen, wordt u gevraagd om te autoriseren de [CloudSimple Service autorisatie](#consent-to-cloudsimple-service-authorization-application) toepassing. 

    ![Starten van CloudSimple-portal](media/launch-cloudsimple-portal.png)

> [!TIP]
> Als u een Private Cloud-bewerking (zoals het maken of uitbreiden van een Private Cloud) rechtstreeks vanuit de Azure portal selecteert, wordt de portal CloudSimple geopend op de aangegeven pagina.

Selecteer in de portal CloudSimple **Start** in het menu naast overzichtsinformatie over uw Privéclouds worden weergegeven. De resources en capaciteit van uw Privéclouds worden weergegeven, samen met waarschuwingen en taken die aandacht vereisen. Klik op de benoemde pictogrammen aan de bovenkant van de pagina voor algemene taken.

![Startpagina](media/cloudsimple-portal-home.png)

## <a name="consent-to-cloudsimple-service-authorization-application"></a>Instemmen met toepassing CloudSimple Service autorisatie

Starten van de portal CloudSimple vanuit de Azure-portal voor het eerst nodig uw toestemming voor de toepassing CloudSimple Service autorisatie.  Selecteer **accepteren** verlenen van machtigingen die zijn aangevraagd en toegang tot de portal CloudSimple. 

![Instemmen met CloudSimple Service autorisatie - beheerders](media/cloudsimple-azure-consent.png)

Als u globale beheerder-bevoegdheden hebt, kunt u toestemming voor uw organisatie.  Selecteer **toestemming namens uw organisatie**.

![Instemmen met CloudSimple Service autorisatie - globale beheerder](media/cloudsimple-azure-consent-global-admin.png)

Als uw machtigingen staan niet toe voor toegang tot de portal CloudSimple, neem dan contact op met de globale beheerder van uw tenant om vereiste machtigingen te verlenen.  Een globale beheerder kan toestemming geven namens uw organisatie.

![Instemmen met CloudSimple Service autorisatie - beheerders vereist](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [een privécloud maken](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Meer informatie over het [een privécloud-omgeving configureren](quickstart-create-private-cloud.md)