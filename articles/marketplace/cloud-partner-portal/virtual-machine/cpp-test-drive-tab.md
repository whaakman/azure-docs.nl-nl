---
title: Virtuele machine Test Drive-tabblad in de Cloud Partner-Portal voor Azure Marketplace
description: Beschrijving van de Test Drive-tabblad gebruikt bij het maken van een virtuele machine in Azure Marketplace-aanbieding.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 04/25/2019
ms.author: pabutler
ms.openlocfilehash: 31c7968d0d96a44ff166444f73807e0ccb5dc583
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938008"
---
# <a name="virtual-machine-test-drive-tab"></a>Virtuele machine Test Drive-tabblad

De **Test Drive** tabblad van de **nieuwe aanbieding** pagina kunt u uw potentiële klanten voorzien van een praktische, zelfgestuurd demonstratie van de belangrijkste functies van uw product en de voordelen, gedemonstreerd in een gestandaardiseerde scenario.  Test Drive is een optionele functie voor de typen die ondersteuning bieden voor Test Drive.  Test Drive vereist ondersteuning van activa op juiste wijze worden geïmplementeerd.  Zie voor meer informatie het artikel [Azure Marketplace Test Drive](https://azure.microsoft.com/blog/azure-marketplace-test-drive/).  <!--TD: Replace with migrated version of Test Drive article! -->

Deze functie inschakelen op de **Test Drive** en klik op de **Ja** kiezen op **inschakelen van een Test Drive**.  De **Test Drive** tabblad beschikbaar is voor het bewerken van velden worden weergegeven.  Een toegevoegde sterretje (*) op de veldnaam van het geeft aan dat dit vereist is.

![Test Drive-tabblad van het formulier nieuwe aanbieding voor virtuele machines](./media/publishvm_007.png)


## <a name="field-values"></a>Veldwaarden

De volgende tabel beschrijft het doel en de inhoud van deze velden.  Verplichte velden zijn indicted met een asterisk (*).


|    Veld                  |       Description                                                            |
|  ---------                |     ---------------                                                          |
|  *Details*   |  |
| **Beschrijving\***           | Biedt een overzicht van uw Test Drive-scenario. Deze tekst wordt weergegeven aan de gebruiker tijdens de Test Drive wordt ingericht. Dit veld ondersteunt eenvoudige HTML als u wilt opgemaakte inhoud bieden.  |
| **Gebruikershandleiding\***           | Upload een gedetailleerde handleiding (PDF) waarmee Test Drive-gebruikers te begrijpen hoe u uw oplossing.  |
| **Test Drive Demo Video** | Upload een video die uw oplossing worden gepresenteerd.  Als u deze optie kiest, moet u een naam, de URL naar de video (die worden gehost op YouTube of Vimeo) en een miniatuur (533 x 324 pixel) opgeven voor de video. |
| *Technische configuratie* |  |
| **exemplaren\***             | Beschikbaarheid in regio's en relatief beschikbaarheid van het vm-exemplaar opgeven (Klik op het pictogram details voor meer informatie).  <br/>Mogelijke gelijktijdige Test Drive-sessies mag niet groter zijn dan de quotumlimiet voor uw abonnement.  De vorige wordt als volgt berekend: [nummer van de regio's geselecteerd] x [' hot ' exemplaren] + [nummer van de regio's geselecteerd] x [warme exemplaren] + [nummer van de regio's geselecteerd] x [koude exemplaren] |
| **De duur van station\***   | Maximale sessieduur in uren. De Test Drive-sessie wordt automatisch beëindigd na deze periode wordt overschreden.  |
|**Test Drive ARM-sjabloon\***| De Azure Resource Manager-sjabloon die is gekoppeld aan deze Test Drive uploaden. Zie voor meer informatie, [transformeren implementatiesjabloon voor virtuele machines voor Test Drive](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Virtual-Machine-Deployment-Template-for-Test-Drive). |
| **Toegang tot informatie\***    | Toegang van Azure Resource Manager en proefversie aanmeldingsgegevens, geschreven als tekst zonder opmaak of een eenvoudige HTML. |
| *Abonnementsgegevens van Test Drive-implementatie* |  |
| **Azure-abonnement-Id\*** | Kan worden verkregen met het aanmelden bij de [Microsoft Azure portal](https://ms.portal.azure.com) en te klikken op **abonnementen** op de menubalk linksboven. (Voorbeeld: "a83645ac-1234-5ab6-6789-1h234g764ghty")    Deze id moet een GUID van het formulier `a83645ac-1234-5ab6-6789-1h234g764ghty`.|
| **Azure AD Tenant Id\***    | Azure Active Directory-tenant-ID.  Kan worden verkregen met het aanmelden bij de [Microsoft Azure portal](https://ms.portal.azure.com) en te klikken op **Azure Active Directory** op de menubalk linksboven, vervolgens te klikken op **eigenschappen** in de middelste menubalk vervolgens kopieert de **map-ID** in het formulier.  Deze id moet ook een GUID zijn.  Als dit leeg is, moet u een tenant-ID maken voor uw organisatie. |
| **Azure AD App Id\***       | ID voor uw geregistreerde Azure VM-oplossing  |
| **Azure AD-App-sleutel\***      | Verificatiesleutel voor uw geregistreerde oplossing |
|   |   |


## <a name="next-steps"></a>Volgende stappen

In de volgende [Marketplace](./cpp-marketplace-tab.md) tabblad, biedt u marketing- en juridische informatie over uw oplossing.
