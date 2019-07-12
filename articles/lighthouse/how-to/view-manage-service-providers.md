---
title: Weergeven en beheren van serviceproviders in Azure portal
description: Klanten kunnen de pagina serviceproviders in Azure portal gebruiken om informatie over serviceproviders, serviceprovider-aanbiedingen en gedelegeerde resources weer te geven.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: a45458e7417bba058522fdc0dbc34fee04ad9af8
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809839"
---
# <a name="view-and-manage-service-providers"></a>Weergeven en beheren van serviceproviders

Klanten kunnen gebruikmaken van de **serviceproviders** pagina in de [Azure-portal](https://portal.azure.com) als u wilt weergeven van informatie over service- en serviceprovider-aanbiedingen, overdragen specifieke resources via [Azure gedelegeerd resourcebeheer van](../concepts/azure-delegated-resource-management.md), en koop extra serviceprovider-aanbiedingen. Hoewel we naar serviceproviders en klanten hier verwijzen, kunnen ondernemingen die meerdere tenants beheren hetzelfde proces gebruiken om te consolideren van hun ervaring.

Toegang krijgen tot de **serviceproviders** pagina in de Azure portal, de klant kunt selecteren **alle services**, zoek vervolgens naar **serviceproviders** en selecteer deze. Ze kunnen deze ook vinden door te voeren 'Serviceproviders' in het zoekvak boven aan de Azure-portal.

Houd er rekening mee dat de **serviceproviders** pagina bevat alleen informatie over de serviceproviders die toegang tot de abonnementen of resourcegroepen via Azure-resource gedelegeerd beheer van de klant hebben. Als een klant met additionele serviceproviders die Azure-gedelegeerde Resourcemanagement niet gebruiken werkt voor toegang tot resources van de klant, wordt informatie over deze serviceproviders niet hier weergegeven.

> [!NOTE]
> Serviceproviders vindt informatie over hun klanten door te navigeren naar **mijn klanten** in Azure portal. Zie voor meer informatie, [weergeven en beheren van klanten en gedelegeerde resources](view-manage-customers.md).

## <a name="view-service-provider-details"></a>Serviceprovider-details weergeven

Als u wilt weergeven van informatie over de dienstverlener waarmee een klant werkt, kunnen ze selecteren **Provider wordt geboden** aan de linkerkant van de **serviceproviders** pagina.

Voor elke serviceprovider-aanbieding ziet de klant de naam van de serviceprovider en de aanbieding die zijn gekoppeld aan dit, samen met de naam die de klant hebt opgegeven tijdens het voorbereidingsproces.

In de **overdrachten** kolom, de klant krijgt te zien hoeveel abonnementen en/of resourcegroepen zijn toegewezen aan de serviceprovider voor deze aanbieding. De service-provider is mogelijk om te openen en beheren van deze abonnementen en/of de resourcegroepen op basis van de toegangsniveaus die is opgegeven in de aanbieding.

## <a name="delegate-resources"></a>Resources delegeren

Voordat een serviceprovider openen en beheren van resources van een klant, moeten ze worden overgedragen. Als een klant een aanbieding heeft geaccepteerd, maar alle resources die nog niet is overgedragen, zien ze een opmerking aan de bovenkant van de **Provider wordt geboden** sectie. Hiermee wordt de klant weten dat ze nodig hebben om actie te ondernemen voordat de service-provider krijgen resources van de klant tot toegang.

Gemachtigde abonnementen of resourcegroepen:

1. Schakel het selectievakje voor de rij met de serviceprovider, de aanbieding en de naam. Selecteer vervolgens **resources delegeren** aan de bovenkant van het scherm.
1. In de **details van de aanbieding** sectie van de **resources delegeren** pagina, lees de informatie over de serviceprovider en bieden. Als u wilt controleren roltoewijzingen voor de aanbieding, selecteer **Klik hier om te zien van de details van het geselecteerde pakket**.
1. In de **gemachtigde** sectie, selecteer **abonnementen overdragen** of **delegeren resourcegroepen**.
1. Kies de abonnementen en/of de resourcegroepen die u wilt delegeren voor deze aanbieding, selecteer vervolgens **toevoegen**.
1. Schakel het selectievakje onder aan de pagina om te bevestigen dat u deze service-provider toegang verlenen tot de resources die u hebt geselecteerd wilt, en selecteer vervolgens **gemachtigde**.

## <a name="add-or-remove-service-provider-offers"></a>Toevoegen of verwijderen van serviceprovider-aanbiedingen

Een klant kan een nieuwe serviceprovider-aanbieding van toevoegen de **Provider wordt geboden** pagina door te selecteren **toevoegen aanbieding**. De serviceprovider moet een aanbieding hebt gepubliceerd voor de klant. De klant kan deze aanbieding van selecteren de **persoonlijke aanbiedingen** scherm en selecteer vervolgens **maken**.

Als de klant wil verwijderen van een serviceprovider-aanbieding, kunnen ze het prullenbakpictogram in de rij voor deze aanbieding selecteren. Nadat is bevestigd dat de verwijdering, heeft die serviceprovider niet langer toegang tot de resources van de klant die eerder zijn overgedragen voor deze aanbieding.

## <a name="view-delegations"></a>Weergave-overdrachten

Overdrachten vertegenwoordigen de roltoewijzingen die machtigingen verlenen voor de service-provider voor de resources die een klant is overgedragen. Als u wilt deze gegevens weergeven, selecteert u **overdrachten** aan de linkerkant van de **serviceproviders** pagina.

Filters aan de bovenkant van de pagina kunt u sorteren en groeperen uw overdracht info of filteren op specifieke klanten, aanbiedingen of trefwoorden.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure naar](../overview.md).
- Informatie over hoe serviceproviders kunnen [weergeven en beheren van klanten](view-manage-customers.md) door te gaan naar **mijn klanten** in Azure portal.