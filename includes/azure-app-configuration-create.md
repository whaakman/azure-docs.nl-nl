---
title: bestand opnemen
description: bestand opnemen
services: azure-app-configuration
author: yegu
ms.service: azure-app-configuration
ms.topic: include
ms.date: 01/22/2019
ms.author: yegu
ms.custom: include file
ms.openlocfilehash: 71e63de247e05a4712687354ed548219b36e8f2f
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885121"
---
1. Als u een nieuw configuratiearchief voor de app wilt maken, moet u zich eerst aanmelden bij de [Azure-portal](https://aka.ms/azconfig/portal). Selecteer **+ Een resource maken** linksboven van de pagina. Typ in het tekstvak **Marketplace doorzoeken** de tekst **App Configuration** en druk op **Enter**.

    ![Zoek naar App Configuration](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

2. Klik in de lijst met zoekresultaten op **App Configuration** en vervolgens op **Maken**.

3. Voer op de pagina **App Configuration** > **Maken** de volgende instellingen in:

    | Instelling | Voorgestelde waarde | Beschrijving |
    |---|---|---|
    | **Resourcenaam** | Wereldwijd unieke naam | Voer een unieke resourcenaam in voor de resource van het app-configuratiearchief. De naam moet een tekenreeks zijn van 1 tot 63 tekens die alleen cijfers, letters en het teken `-` mag bevatten. De naam mag niet beginnen of eindigen met het teken `-`, en opeenvolgende `-`-tekens zijn niet toegestaan.  |
    | **Abonnement** | Uw abonnement | Selecteer het Azure-abonnement dat u wilt gebruiken om App Configuration te testen. Als uw account maar één abonnement heeft, wordt dit automatisch geselecteerd en wordt de vervolgkeuzelijst **Abonnement** niet weergegeven. |
    | **Resourcegroep** | *AppConfigTestResources* | Selecteer of maak een resourcegroep voor de resource van het app-configuratiearchief. Deze groep is handig voor het ordenen van meerdere resources die u mogelijk op een bepaald moment wilt verwijderen door resourcegroep te verwijderen. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](/azure/azure-resource-manager/resource-group-overview) voor meer informatie. |
    | **Locatie** | *US - centraal* | Gebruik **Locatie** om de geografische locatie op te geven waar het app-configuratiearchief wordt gehost. Voor de beste prestaties wordt u aangeraden om de resource te maken in dezelfde regio als de andere onderdelen van uw toepassing. |

    ![Resource voor app-configuratiearchief maken](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

4. Klik op **Create**. De implementatie kan enkele minuten duren.

5. Wanneer de implementatie is voltooid, klikt u op **Instellingen** > **Toegangssleutels**. Noteer de verbindingsreeks van de primaire alleen-lezen of lees/schrijf-sleutel. U hebt deze later nodig voor de configuratie van uw toepassing, zodat deze kan communiceren met het app-configuratiearchief dat u hebt gemaakt.

6. Klik op **Sleutel-waardeverkenner** en **+ Maken** om de volgende sleutel-waardeparen toe te voegen:

    | Sleutel | Waarde |
    |---|---|
    | TestApp:Settings:BackgroundColor | wit |
    | TestApp:Settings:FontSize | 24 |
    | TestApp:Settings:FontColor | zwart |
    | TestApp:Settings:Message | Gegevens van Azure App Configuration |

    **Label** en **Inhoudstype** laten we nog even achterwege.
