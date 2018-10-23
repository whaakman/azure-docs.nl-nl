---
title: Virtuele machine Test Drive-tabblad in de Cloud Partner-Portal voor Azure Marketplace | Microsoft Docs
description: Beschrijving van de Test Drive-tabblad gebruikt bij het maken van een virtuele machine in Azure Marketplace-aanbieding.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 97fb21dc390bd365357f6395c72aa282423c83c9
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639706"
---
# <a name="virtual-machine-test-drive-tab"></a>Virtuele machine Test Drive-tabblad

<!-- TD: The AMP tree needs a conceptual/business overview of Test Drive. I've deleted all the marketing fluff and most of overview from this topic.   See also https://azure.microsoft.com/blog/azure-marketplace-test-drive/ and https://github.com/Azure/AzureTestDrive/wiki/What-is-a-Test-Drive. --> 

De **Test Drive** tabblad van de **nieuwe aanbieding** pagina kunt u uw potentiële klanten voorzien van een praktische, zelfgestuurd demonstratie van de belangrijkste functies van uw product en de voordelen, gedemonstreerd in een gestandaardiseerde scenario.  Test Drive is een optionele functie voor de typen die ondersteuning bieden voor Test Drive.  Test Drive vereist ondersteuning van activa op juiste wijze worden geïmplementeerd.  Zie voor meer informatie het artikel [Azure Marketplace Test Drive](https://azure.microsoft.com/blog/azure-marketplace-test-drive/).  <!--TD: Replace with migrated version of Test Drive article! -->

Deze functie inschakelen op de **Test Drive** en klik op de **Ja** kiezen op **inschakelen van een Test Drive**.  De **Test Drive** tabblad beschikbaar is voor het bewerken van velden worden weergegeven.  Een toegevoegde sterretje (*) op de veldnaam van het geeft aan dat dit vereist is.

![Test Drive-tabblad van het formulier nieuwe aanbieding voor virtuele machines](./media/publishvm_007.png)

<br/>

De volgende tabel beschrijft het doel en de inhoud van deze velden.


|  **Veld**                |     **Beschrijving**                                                          |
|  ---------                |     ---------------                                                          |
|  *Details*   |  |
| **Beschrijving**           | Biedt een overzicht van uw Test Drive-scenario. Deze tekst wordt weergegeven aan de gebruiker tijdens de Test Drive wordt ingericht. Dit veld ondersteunt eenvoudige HTML als u wilt opgemaakte inhoud bieden.  |
| **Gebruikershandleiding**           | Upload een gedetailleerde handleiding (PDF) waarmee Test Drive-gebruikers te begrijpen hoe u uw oplossing.  |
| **Demovideo van Test Drive** | Upload een video die uw oplossing worden gepresenteerd.  Als u deze optie kiest, moet u een naam, de URL naar de video (die worden gehost op YouTube of Vimeo) en een miniatuur (533 x 324 pixel) opgeven voor de video. |
| *Technische configuratie* |  |
| **exemplaren**             | Beschikbaarheid in regio's en relatief beschikbaarheid van het vm-exemplaar opgeven (Klik op het pictogram details voor meer informatie).  <br/>Mogelijke gelijktijdige Test Drive-sessies mag niet groter zijn dan de quotumlimiet voor uw abonnement.  De vorige wordt als volgt berekend: [nummer van de regio's geselecteerd] x [' hot ' exemplaren] + [nummer van de regio's geselecteerd] x [warme exemplaren] + [nummer van de regio's geselecteerd] x [koude exemplaren] |
| **De duur van station**   | Maximale sessieduur in uren. De Test Drive-sessie wordt automatisch beëindigd na deze periode wordt overschreden.  |
|**Test Drive ARM-sjabloon**| De Azure Resource Manager-sjabloon die is gekoppeld aan deze Test Drive uploaden. Zie voor meer informatie, [transformeren implementatiesjabloon voor virtuele machines voor Test Drive](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Virtual-Machine-Deployment-Template-for-Test-Drive). |
| **Toegang tot informatie**    | Toegang van Azure Resource Manager en proefversie aanmeldingsgegevens, geschreven als tekst zonder opmaak of een eenvoudige HTML. |
| *Abonnementsgegevens van Test Drive-implementatie* |  |
| **Azure-abonnement-Id** | Kan worden verkregen met het aanmelden bij de [Microsoft Azure portal](https://ms.portal.azure.com) en te klikken op **abonnementen** op de menubalk linksboven. (Voorbeeld: "a83645ac-1234-5ab6-6789-1h234g764ghty")    Deze id moet een GUID van het formulier `a83645ac-1234-5ab6-6789-1h234g764ghty`.|
| **Azure AD-Tenant-Id**    | Azure Active Directory-tenant-ID.  Kan worden verkregen met het aanmelden bij de [Microsoft Azure portal](https://ms.portal.azure.com) en te klikken op **Azure Active Directory** op de menubalk linksboven, vervolgens te klikken op **eigenschappen** in de middelste menubalk vervolgens kopieert de **map-ID** in het formulier.  Deze id moet ook een GUID zijn.  Als dit leeg is, moet u een tenant-ID maken voor uw organisatie. |
| **Azure AD-App-Id**       | ID voor uw geregistreerde Azure VM-oplossing  |
| **Azure AD-App-sleutel**      | Verificatiesleutel voor uw geregistreerde oplossing |
|  |  |

<br/>

In de volgende [Marketplace](./cpp-marketplace-tab.md) tabblad, biedt u marketing- en juridische informatie over uw oplossing.
