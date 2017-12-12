---
title: Veelgestelde vragen over ClearDB MySql-databases met Azure App Service | Microsoft Docs
description: Antwoorden op veelgestelde vragen over het gebruik van ClearDB MySQL-databases met Azure App Service.
documentationcenter: php
services: 
author: sunbuild
manager: yochayk
editor: 
tags: mysql
ms.assetid: c2ed5e78-6d7d-4d0c-b7ee-a52ae41ceab8
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.author: sumuth
ms.openlocfilehash: 0d1e3c826e61b8dee264e1c01fd2d54ba7ebb636
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Veelgestelde vragen voor ClearDB MySql-databases met Azure App Service
Deze Veelgestelde vragen over de antwoorden op veelgestelde vragen over het gebruik en de aanschaf van ClearDB MySQL databases voor Web-Apps van Azure.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Welke opties heb ik voor MySQL in Azure?
U hebt verschillende mogelijkheden:

* [ClearDB gedeeld MySQL-database](/marketplace/partners/cleardb/databases/)
* [ClearDB MySQL Premium clusters](/marketplace/partners/cleardb-clusters/cluster/)
* [MySQL-cluster uitgevoerd op een virtuele machine in Azure](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)
* [Één exemplaar van MySQL uitgevoerd op een virtuele machine in Azure](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

ClearDB is van een hostingservice MySQL en de MySQL-infrastructuur voor u beheert. Wanneer u uw eigen MySQL-cluster of de database op een virtuele Machine van Azure uitvoert, hebt u de MySQL-server instellen en werken met patches.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Heb ik een creditcard nodig voor de Web-app + MySQL-sjabloon in Azure Marketplace?
Dit is afhankelijk van het type van het abonnement dat u gebruikt. Hier volgen enkele typen vaak gebruikte abonnement:

* [U betaalt](/offers/ms-azr-0003p/): vereist een creditcard en bij de aankoop van een betaald MySQL-database uw creditcard in rekening gebracht.
* [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/): tegoed bevat voor gebruik met Microsoft Azure-services, maar aankoop van resources van derden is niet toegestaan. Om aan te schaffen services van derden of een betaald MySQL-database moet u een creditcard ingeschakeld-abonnement gebruiken. U kunt een gratis ClearDB MySQL-database maken voor Web-Apps.
* [MSDN-abonnement](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/) en **omslagstelsel MSDN ontwikkelen, testen**: vergelijkbaar met de gratis proefversie, een MSDN-abonnement moet u een creditcard koopt van een betaald MySQL-oplossing van ClearDB hebben.
* [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/): EA klanten worden gefactureerd tegen hun EA elk kwartaal voor alle hun aankopen Azure Marketplace (derde) op een afzonderlijke, geconsolideerde factuur. U wordt gefactureerd buiten de maandbedrag gedurende een marketplace-aankopen. Houd er rekening mee dat op dit moment Azure Store niet beschikbaar voor klanten die zijn ingeschreven in Azerbeidzjan, Kroatië, Noorwegen en Portorico is. 
* [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): U kunt alleen gratis ClearDB databases voor Web-Apps maken. Er is geen limiet voor het aantal vrije ClearDB MySQL-databases die u kunt maken. Houd er rekening mee dat de gratis databases zijn niet moet worden gebruikt voor productie-web-apps, als deze service is alleen bedoeld voor proefversie.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Waarom is ik $3,50 voor een Web-app + MySQL vanuit Azure Marketplace in rekening gebracht?
De databaseoptie standaard is Titan die $3,50. We niet de kosten weergeven tijdens het maken van de database en u hebt per ongeluk een database die u niet wilt aanschaffen. We proberen toegang te zoeken naar een manier om de ervaring te verbeteren, maar tot die tijd moet u alle uw geselecteerde Prijscategorieën voor web-app en database controleren voordat u op **maken** en de implementatie van de resources te starten.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Ik heb MySQL op mijn eigen virtuele machine van Azure. Kan ik mijn Azure-web-app verbinden met mijn database?
Ja. U kunt uw web-app verbinden met uw database, zolang uw Azure VM externe toegang in uw web-app heeft gegeven. Zie voor meer informatie [MySQL installeren op een virtuele machine](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>In welke landen zijn ClearDB Premium MySQL-clusters ondersteund?
[ClearDB Premium MySQL clusters](/marketplace/partners/cleardb-clusters/cluster/) zijn beschikbaar in alle Azure-regio's wereldwijd met uitzondering van India, Australië, Brazilië-Zuid en China.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Kan ik een nieuw cluster voordat u een database met de premium-clusteroplossing ClearDB maken?
Nee, leeg ClearDB-clusters maken wordt niet ondersteund. De Azure-portal kunt u om databases te maken in een cluster, waarmee een nieuw cluster tegelijkertijd kan worden gemaakt.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Ik gewaarschuwd als ik wil verwijderen van een ClearDB MySQL-database die wordt gebruikt door een van mijn toepassingen?
Nee, Azure wordt geen waarschuwing als u een marketplace-aankopen die uw toepassing afhankelijk van is verwijderen.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Welke regio's kan ik ClearDB-databases in maken?
Azure Marketplace is niet beschikbaar voor klanten die zijn ingeschreven in Azerbeidzjan, Kroatië, Noorwegen of Portorico. ClearDB is niet beschikbaar in deze regio's.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Welke prijscategorie moet ik kiezen voor een productie-web-app en database?
Basic of een hogere prijscategorie voor Web-Apps gebruiken. Voor ClearDB, wordt aangeraden een Saturnus of Jupiter plan. Controleer de kenmerken en beperkingen van elke prijscategorie voor beide [Web-Apps](https://azure.microsoft.com/pricing/details/app-service/) en [ClearDB MySQL-databases](/marketplace/partners/cleardb/databases/) te kiezen die past bij uw behoeften.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Hoe voer ik een upgrade mijn ClearDB-database van het ene plan naar een andere?
In de [Azure-portal](https://portal.azure.com), kunt u een database ClearDB gedeelde hosting opschalen. Lees dit [artikel](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) voor meer informatie. Momenteel ondersteund niet upgrade voor ClearDB Premium clusters in de Azure portal.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Ik kan mijn ClearDB-database in Azure-portal niet zien?
Als u een ClearDB-database in de klassieke hebt gemaakt, kunt u zich niet zien van de database in de [Azure Portal](https://portal.azure.com). Er is geen tijdelijke oplossing voor dit scenario.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Wie ik contact opnemen voor ondersteuning bij mijn database niet beschikbaar is?
Neem contact op met [ClearDB ondersteuning](https://www.cleardb.com/developers/help/support) voor alle databasequery gerelateerde problemen. Voorbereiden om ze te bieden met de informatie van uw Azure-abonnement.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Kan ik extra gebruikers maken voor de clusteroplossing van mijn ClearDB MySQL-database?
Nee. U kunt geen extra gebruikers maken, maar u kunt aanvullende databases maken op uw cluster ClearDB-database.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Kan Basic/Pro-serie databases worden in-place vergelijkbaar met vandaag planeten plannen op ClearDB portal bijgewerkt?
Ja, basismodule databases kunnen worden bijgewerkt in-place (standaard 60 via Basic 500). Pro-serie kan worden bijgewerkt in-place (Pro 125 via Pro 1000), met uitzondering van Pro 60. Pro 60 database momenteel bijwerken wordt niet ondersteund. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Wanneer ik mijn resources uit één abonnement naar een andere migreert, Mijn ClearDB MySQL-database worden overgezet ook?
Wanneer u Resourcemigratie uitvoert voor abonnementen, enkele [beperkingen](app-service/app-service-move-resources.md) toepassen. Een ClearDB MySQL-database is een service van derden en daarom niet ophalen gemigreerd tijdens de migratie van de Azure-abonnement. Als u de migratie van uw MySQL-database vóór de migratie Azure-resources niet beheert, kunnen uw ClearDB MySQL-databases worden uitgeschakeld. Handmatig migreren van uw databases eerst en voert u de migratie van de Azure-abonnement voor uw web-app. 

## <a name="i-hit-the-spending-limit-on-my-subscription-i-removed-the-limit-and-my-app-service-is-online-however-the-database-is-not-accessible-how-do-i-re-enable-the-cleardb-database"></a>Ik heb bereikt de bestedingslimiet op mijn abonnement. Ik heb de limiet hebt verwijderd en Mijn App-Service is online, maar de database niet toegankelijk is. Hoe kan ik de ClearDB-database opnieuw inschakelen?
Neem contact op met [ClearDB ondersteuning](https://www.cleardb.com/developers/help/support) opnieuw inschakelen van de database. Geef ze met de naam van uw Azure-abonnement en de database.

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Kan ik een ClearDB-database van een creditcard abonnement overbrengen naar een EA-abonnement?
Bestaande ClearDB-databases maken gebruik van de creditcard die is gekoppeld aan de bestaande abonnementen. Als een EA-abonnement wilt gebruiken, moet u uw gegevens te migreren naar een nieuwe database:

* Een nieuwe database ClearDB met uw abonnement EA aanschaffen.
* Uw gegevens migreren naar de nieuwe database.
* Uw toepassing bijwerken voor gebruik van de nieuwe database.
* Uw oude ClearDB-database verwijderd.

Wanneer u een nieuwe WebApp met MySQL (ClearDB maken) of een MySQL-database (ClearDB) maakt, bepaalt het abonnement dat u kiezen hoe u betaalt voor de service. Met een abonnement EA blokkeert we niet de aanschaf van de services van derden zoals ClearDB in de Azure portal. EA abonnementen zijn buiten bedrag in rekening gebracht en worden gefactureerd per kwartaal en achterstallig. De klant EA zou hebben voor het instellen van een betalingswijze zoals een creditcard betaalt voor alle services van derden marketplace.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Waar kan ik de kosten voor ClearDB-resources in een abonnement EA zien?
Azure Marketplace-kosten zijn voor klanten Direct EA is zichtbaar in de Enterprise Portal. Houd er rekening mee dat alle marketplace-aankopen en het verbruik buiten bedrag in rekening gebracht zijn en worden gefactureerd per kwartaal en achterstanden. EA klanten betalen rechtstreeks aan de service van derden-providers en kunnen dit doen door een betalingswijze zoals een creditcard aan hun account EA inschakelen.

Indirecte EA klanten hun Azure Marketplace-abonnementen kunnen vinden op de **abonnementen beheren** pagina van de Enterprise Portal, maar prijzen wordt verborgen. Klanten moeten contact opnemen met hun gelaagde Serviceproviders voor informatie over de marketplace-kosten.

Toegang tot Azure Marketplace voor services van derden kan worden beheerd door uw inschrijving EA Azure beheerders. Ze kunnen uitschakelen of opnieuw toegang inschakelen voor 3e partij aankopen uit het archief in Accounts beheren en abonnementen onder de sectie Accounts in de Enterprise Portal.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Wie ik contact opnemen voor vragen over mijn factuur voor ClearDB-services in mijn abonnement EA?
Neem contact op met [Enterprise Customer Support](http://aka.ms/AzureEntSupport) met betrekking tot facturering onder hun EA registratie. Het ondersteuningsteam van EA Portal wordt uw vraag te beantwoorden of uw probleem op te lossen.

## <a name="more-information"></a>Meer informatie
[Veelgestelde vragen over Azure Marketplace](/marketplace/faq/)

