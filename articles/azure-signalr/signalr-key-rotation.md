---
title: Toegangssleutels roteren voor Azure SignalR Service
description: In dit artikel leest u waarom klanten de toegangssleutels regelmatig moeten roteren en hoe ze dit kunnen doen met de gebruikersinterface van de portal en CLI.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 2c0f60b0ef3a90372fc4a095c830f39bc148f354
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53636063"
---
# <a name="access-key-rotation-for-azure-signalr-service"></a>Toegangssleutels roteren voor Azure SignalR Service

Elk Azure SignalR Service-exemplaar heeft een paar toegangssleutels: een primaire en een secundaire sleutel. Deze worden gebruikt om SignalR-klanten te verifiëren bij aanvragen bij de service. De sleutels zijn gekoppeld aan de eindpunt-URL van het exemplaar. Het is verstandig uw sleutels te beveiligen en regelmatig te roteren. Er worden twee toegangssleutels geleverd, zodat u met één sleutel verbonden kunt blijven terwijl u de andere sleutel opnieuw genereert.

## <a name="why-rotate-access-keys"></a>Waarom moet u toegangssleutels roteren?

Aan ontwikkelaars wordt om veiligheidsredenen en nalevingsvereisten aangeraden toegangssleutels regelmatig te roteren.

## <a name="how-to-regenerate-access-keys"></a>Hoe worden toegangssleutels opnieuw gegenereerd?

1. Ga naar [Azure Portal](https://portal.azure.com/) en meld u aan met uw referenties.

1. Zoek de sectie **Sleutels** in het Azure SignalR Service-exemplaar waarvan u de sleutels opnieuw wilt genereren.

1. Klik op **Sleutels** in het navigatiemenu.

1. Klik op **Primaire sleutel opnieuw genereren** of **Secundaire sleutel opnieuw genereren**.

Er worden een nieuwe sleutel en bijbehorende verbindingsreeks gemaakt en weergegeven.

 ![Sleutels opnieuw genereren](media/signalr-key-rotation/regenerate-keys.png)

U kunt sleutels ook opnieuw genereren met [Azure CLI](/cli/azure/ext/signalr/signalr/key?view=azure-cli-latest#ext-signalr-az-signalr-key-renew).

## <a name="update-configurations-with-new-connection-strings"></a>Configuraties bijwerken met nieuwe verbindingsreeksen

1. Kopieer de opnieuw gegenereerde verbindingsreeks.

1. Werk alle configuraties zo bij dat deze gebruikmaken van de nieuwe verbindingsreeks.

1. Start de toepassing opnieuw op, indien nodig.

## <a name="forced-access-key-regeneration"></a>Toegangssleutels geforceerd opnieuw genereren

Azure SignalR-Service kan in bepaalde situaties afdwingen dat een toegangssleutel opnieuw wordt gegenereerd. Klanten worden hiervan per e-mail en via de portal op de hoogte gesteld. Als u een dergelijk bericht ontvangt of er een fout in de service optreedt met betrekking tot de toegangssleutel, moet u de sleutels roteren volgens de instructies in dit artikel.

## <a name="next-steps"></a>Volgende stappen

Om veiligheidsredenen is het raadzaam om de toegangssleutels regelmatig te roteren.

In dit artikel hebt u geleerd hoe u toegangssleutels opnieuw kunt genereren. Ga door met de volgende zelfstudies over verificatie met OAuth of met Azure Functions.

> [!div class="nextstepaction"]
> [Integreren met ASP.NET Core-identiteit](./signalr-authenticate-oauth.md)

> [!div class="nextstepaction"]
> [Een serverloze, realtime app met verificatie bouwen](./signalr-authenticate-azure-functions.md)