---
title: Azure uitproberen van de toepassing-aanbieding | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van test drive voor de aanbieding van Azure-toepassing op Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: de4c8aecefce334889c3fa6790c0ba42673896c7
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196793"
---
# <a name="azure-applications-test-drive-tab"></a>Azure-toepassingen Test Drive-tabblad

Gebruik het tabblad Test Drive voor een proefversie van uw klanten.

## <a name="test-drive-benefits"></a>Voordelen van Test Drive

Het maken van een evaluatieversie voor uw klanten is een aanbevolen procedure om te controleren of dat ze met vertrouwen kunnen kopen. Test Drive is van de proefversie opties die beschikbaar is, de meest effectief bij het genereren van hoge kwaliteit potentiële klanten en hogere conversie van deze leads.

Daardoor kunnen klanten met een praktische, zelfgestuurd evaluatieversie van de belangrijke functies en voordelen, gedemonstreerd in een implementatiescenario voor het werkelijke van uw product.

## <a name="how-a-test-drive-works"></a>De werking van een test uit

Een potentiële klant wordt gezocht en detecteert de toepassing op de Marketplace. De klant zich heeft aangemeld en gaat akkoord met de gebruiksvoorwaarden. Op dit moment ontvangt de klant uw vooraf geconfigureerde omgeving om te proberen voor een vast aantal uren, terwijl u een zeer gekwalificeerde lead te ontvangen. Zie voor meer informatie, [wat is Test Drive?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/what-is-test-drive)

## <a name="setting-up-a-test-drive"></a>Instellen van een test uit

Gebruik de volgende stappen uit het inschakelen en configureren van een test uit.

### <a name="to-enable-a-test-drive"></a>Een test uit inschakelen:

1. Onder **nieuwe aanbieding**, selecteer de **Test Drive** tabblad.
2. Onder **Test Drive**, selecteer **Ja** voor **inschakelen van een Test Drive**.

   ![Een test uit te schakelen](./media/managed-app-enable-testdrive.png)

### <a name="to-configure-a-test-drive"></a>Het configureren van een test uit:

Nadat u een test uit inschakelt, moet u de volgende formulieren voor het instellen van test drive invullen:
  
 - Details
 - Technische configuratie
 - Abonnementsgegevens van Test Drive-implementatie

De volgende schermopname ziet u alle Test Drive-formulieren. Een sterretje (*) toegevoegd aan de naam van het veld geeft aan dat dit is verplicht. 

![Configureren van een test uit](./media/managed-app-configure-testdrive.png)

De volgende tabel beschrijft de velden die vereist zijn voor het instellen van test drive voor uw beheerde toepassing.

|    **Veld**       |  **Beschrijving**  |
|  ---------------   |  ---------------  |
|      Description              |   Wat kan worden uitgevoerd op uw Test Drive wordt beschreven. U kunt eenvoudige HTML-codes gebruiken om deze beschrijving. Bijvoorbeeld, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt;, en koppen.                |
|    Gebruikershandleiding                |     Een gebruiker handmatig die uw klanten gebruiken kunnen om te zien hoe Test Drive uploaden. Dit document moet een PDF-bestand.              |
|         Test Drive demovideo (optioneel)           |       U kunt een video-overzicht van uw Test Drive opgeven. Een klant kan deze video kunt bekijken voordat ze een test uitvoeren. Geef een URL naar de video op YouTube of Vimeo. Als u selecteert **+ Video toevoegen**, wordt u gevraagd om de volgende informatie te geven:<ul><li>Name</li><li>URL</li><li>Miniatuur (in PNG-indeling hebben, 533 x 324 pixels)</li></ul>            |
|       Exemplaren             |        Hoeveel exemplaren u wilt configureren, in welke regio('s), en hoe snel uw klanten de Test Drive krijgen. Zie voor meer informatie, [het publiceren van een Test Drive](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/azure-resource-manager-test-drive#how-to-publish-a-test-drive).           |
|       Test Drive duur (uren)             |       Voer een geheel getal voor het aantal uren. Het toegestane bereik ligt tussen 1 en 999 liggen.            |
|        Test Drive ARM-sjabloon            |        Upload een gecomprimeerd bestand (.zip) met uw Azure Resource Manager-sjablonen voor uw app. Zie voor meer informatie, [Azure Resource Manager Test Drive](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/azure-resource-manager-test-drive).            |
|        Toegang tot informatie            |         Toegang tot informatie opgeven nadat de wordt de Test Drive van uw klant. Bijvoorbeeld, een URL voor toegang tot het station test en meld u informatie. . U kunt eenvoudige HTML-codes gebruiken om deze beschrijving. Bijvoorbeeld, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt;, en koppen.          |
|       Toegang tot abonnements-Id             |       Dit verleent toegang tot Azure-services en de Azure-portal. Het abonnement is waarbij Resourcegebruik wordt gerapporteerd, en services worden in rekening gebracht. Als u nog een afzonderlijke Azure-abonnement hebt voor de Test Drives alleen, maakt u een abonnement.             |
|          Azure AD-Tenant-Id          |        Geef een bestaande Tenant in Azure Active Directory of een tenant voor deze test drive maken.           |
|         Azure AD-App-Id           |       Maak en registreer een nieuwe toepassing. Microsoft gebruikt deze toepassing voor het uitvoeren van bewerkingen op uw Test Drive-exemplaar.            |
|          Azure AD-App-sleutel          |         Maak een verificatiesleutel voor de app en plak deze in dit veld.          |

Nadat u de vereiste gegevens hebt opgegeven, selecteert u **opslaan** instellen van test drive voltooid.

## <a name="next-steps"></a>Volgende stappen

[Tabblad voor Marketplace](./cpp-marketplace-tab.md)