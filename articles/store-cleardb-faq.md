---
title: Veelgestelde vragen voor ClearDB MySql-databases met Azure App Service | Microsoft Docs
description: Antwoorden op veelgestelde vragen over het gebruik van ClearDB MySQL-databases met Azure App Service.
documentationcenter: php
services: mysql
author: sunbuild
manager: yochayk
tags: mysql
ms.service: multiple
ms.workload: data-management
ms.topic: article
ms.date: 10/27/2016
ms.author: sumuth
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: cce6555c71829110781a6dd13757078b105dbb41
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51230628"
---
# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Veelgestelde vragen voor ClearDB MySql-databases met Azure App Service
Deze Veelgestelde vragen vindt u antwoorden op veelgestelde vragen over het gebruik en het kopen van de ClearDB MySQL databases voor Azure Web Apps.

> [!IMPORTANT]
> ClearDB is overgegaan vanaf 13 juni 2018 is momenteel door Microsoft aan een directe facturering van maandabonnementen met ClearDB gefactureerd op basis van een Azure-klanten. De informatie in dit artikel is nu verouderd. Niet meer mogelijk te maken of bijwerken van een ClearDB-database die is gemaakt in Azure.
>
> Zie voor meer details en de volgende stappen, [wijzigingen in de serviceabonnementen ClearDB](http://w2.cleardb.net/important-change-of-billing-notice-for-all-azure-cleardb-service-plans/).


## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Welke opties heb ik voor MySQL in Azure?
U hebt verschillende mogelijkheden:

* [ClearDB gedeeld MySQL-database](/marketplace/partners/cleardb/databases/)
* [ClearDB MySQL-Premium-clusters](/marketplace/partners/cleardb-clusters/cluster/)
* [MySQL-cluster in een Azure-VM](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)
* [Één exemplaar van MySQL die worden uitgevoerd op een Azure-VM](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

ClearDB is een hostingservice MySQL en beheert de MySQL-infrastructuur voor u. Wanneer u uw eigen MySQL-cluster of de database op een Azure-Machine uitvoeren, hebt u de MySQL-server instellen en werken met patches.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Heb ik een creditcard nodig voor de Web-app + MySQL-sjabloon in de Azure Marketplace?
Dit is afhankelijk van het type abonnement dat u gebruikt. Hier volgen enkele veelgebruikte abonnementstypen:

* [Betalen](https://azure.microsoft.com/offers/ms-azr-0003p/): vereist een creditcard is geregistreerd, en bij de aankoop van een betaalde MySQL-database voor uw creditcard in rekening gebracht.
* [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/): tegoeden voor gebruik met Microsoft Azure-services, maar aankoop van resources van derden is niet toegestaan. Voor het kopen van services van derden of een betaald MySQL-database moet u een creditcard ingeschakeld abonnement gebruiken. U kunt een gratis ClearDB MySQL-database maken voor Web-Apps.
* [MSDN-abonnement](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/) en **MSDN Dev Test betalen**: net als bij gratis proefversie, MSDN-abonnement vereist dat u hebt een creditcard nodig om aan te schaffen een betaalde MySQL-oplossing van ClearDB.
* [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/): EA-klanten worden gefactureerd op basis van hun EA elk kwartaal voor al hun aankopen op Azure Marketplace (derde) op een afzonderlijke, geconsolideerde factuur. U wordt gefactureerd buiten de monetaire toezegging voor een marketplace-aankopen. Houd er rekening mee dat op dit moment Azure Store niet beschikbaar voor klanten die zijn ingeschreven in Azerbeidzjan, Kroatië, Noorwegen en Porto Rico is. 

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Waarom is ik $3,50 voor een Web-app + MySQL op Azure Marketplace in rekening gebracht?
De standaardoptie voor de database is Titan, wat $3,50. We niet de kosten weergeven tijdens het maken van de database, en u per ongeluk een database die u niet van plan om te bent kan aanschaffen. We willen een manier vinden om de ervaring te verbeteren, maar tot die tijd moet u alle uw geselecteerde Prijscategorieën voor web-app en database controleren voordat u op **maken** en de implementatie van de resources te starten.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Ik ben MySQL op mijn eigen Azure-machine uitgevoerd. Kan ik mijn Azure-web-app verbinden met mijn database?
Ja. U kunt uw web-app verbinden met uw database, zolang uw Azure-VM heeft gegeven van het externe toegang in uw web-app. Zie voor meer informatie, [MySQL installeren op een virtuele machine](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Landen zijn waarin ClearDB Premium MySQL-clusters ondersteund?
[ClearDB Premium MySQL clusters](/marketplace/partners/cleardb-clusters/cluster/) zijn beschikbaar in alle Azure-regio's over de hele wereld met uitzondering van India, Australië, Brazilië-Zuid en China.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Kan ik een nieuw cluster vóór het maken van een database met ClearDB premium clusteroplossing maken?
Nee, het maken van lege ClearDB-clusters wordt niet ondersteund. De Azure-portal kunt u maken van databases in een cluster, wat een nieuw cluster op hetzelfde moment maken mogelijk.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Wordt Word ik gewaarschuwd als ik probeer te verwijderen van een ClearDB MySQL-database die wordt gebruikt door een van mijn toepassingen?
Nee, Azure waarschuwt u niet als u een marketplace aankoop verwijdert waarvan uw toepassing afhankelijk is.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Welke regio's kan ik ClearDB-databases in maken?
Azure Marketplace is niet beschikbaar voor klanten die zijn ingeschreven in Azerbeidzjan, Kroatië, Noorwegen of Porto Rico. ClearDB is niet beschikbaar in deze regio's.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Prijscategorie voor welke moet ik kiezen voor een productie-web-app en -database?
Basic of een hogere prijscategorie voor Web-Apps gebruiken. Voor ClearDB raden wij een Saturn- of Jupiter-abonnement. Bekijk de functies en beperkingen van elke prijscategorie voor zowel [Web-Apps](https://azure.microsoft.com/pricing/details/app-service/) en [ClearDB MySQL-databases](/marketplace/partners/cleardb/databases/) om te kiezen die past bij uw behoeften.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Hoe ik mijn ClearDB-database upgraden van één plan naar een andere?
In de [Azure-portal](https://portal.azure.com), kunt u een ClearDB gedeelde hostdatabase opschalen. Lees dit [artikel](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) voor meer informatie. Op dit moment ondersteund niet upgrade voor ClearDB-Premium-clusters in Azure portal.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Ik zie niet dat mijn ClearDB-database in Azure portal?
Als u een ClearDB-database in de klassieke versie gemaakt, kunt u zich niet ziet u uw database in de [Azure Portal](https://portal.azure.com). Er is geen tijdelijke oplossing voor dit scenario.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Wie moet ik contact opnemen voor ondersteuning als mijn database niet beschikbaar is?
Neem contact op met [ClearDB-ondersteuning](https://www.cleardb.com/developers/help/support) voor elke database gerelateerde problemen. Zodat ze beschikken over de gegevens van uw Azure-abonnement worden voorbereid.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Kan ik extra gebruikers maken voor de clusteroplossing van mijn ClearDB MySQL-database?
Nee. U kunt geen extra gebruikers maken, maar u kunt extra databases maken voor uw cluster ClearDB-database.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Kan de Basic/Pro-serie databases worden bijgewerkt in-place vergelijkbaar planeten plannen vandaag nog op de ClearDB-portal?
Ja, basismodule databases kunnen worden bijgewerkt in-place (standaard 60 via eenvoudige 500). Pro-serie kunnen worden bijgewerkt ter plekke (Pro 125 via Pro 1000), met uitzondering van Pro 60. We bieden geen ondersteuning Pro 60-database op dit moment bijwerken. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Wanneer ik mijn resources van één abonnement naar een andere migreert, Mijn ClearDB MySQL-database gemigreerd ook?
Wanneer u Resourcemigratie uitvoert tussen abonnementen, sommige [beperkingen](azure-resource-manager/resource-group-move-resources.md#app-service-limitations) toepassen. Een ClearDB MySQL-database is een service van derden en daarom niet gemigreerd tijdens de migratie van de Azure-abonnement. Als u niet de migratie van uw MySQL-database vóór migratie Azure-resources beheren, kunnen uw ClearDB MySQL-databases worden uitgeschakeld. Handmatig migreren van uw databases eerst en voer de migratie van de Azure-abonnement voor uw web-app. 

## <a name="i-hit-the-spending-limit-on-my-subscription-i-removed-the-limit-and-my-app-service-is-online-however-the-database-is-not-accessible-how-do-i-re-enable-the-cleardb-database"></a>Kan ik bereikt de bestedingslimiet voor mijn abonnement. Kan ik de limiet verwijderd en Mijn App Service is online, maar de database niet toegankelijk is. Hoe kan ik de ClearDB-database opnieuw inschakelen?
Neem contact op met [ClearDB-ondersteuning](https://www.cleardb.com/developers/help/support) opnieuw inschakelen van de database. Geef ze de naam van uw Azure-abonnement informatie en database.

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Kan ik een ClearDB-database van een creditcard wordt betaald abonnement overbrengen naar een EA-abonnement?
Bestaande ClearDB-databases gebruiken de creditcard die is gekoppeld aan de bestaande abonnementen. Als een EA-abonnement wilt gebruiken, moet u uw gegevens migreren naar een nieuwe database:

* Een nieuwe ClearDB-database met uw EA-abonnement aanschaffen.
* Migreer uw gegevens naar uw nieuwe database.
* Uw toepassing bijwerken voor gebruik van de nieuwe database.
* Uw oude ClearDB-database verwijderd.

Wanneer u een nieuwe WebApp met MySQL (ClearDB maken) of een MySQL-database (ClearDB) maakt, wordt op basis van het abonnement dat u bepaalt hoe u betaalt voor de service. Met een EA-abonnement, wordt er niet de aanschaf van de services van derden zoals ClearDB in Azure portal geblokkeerd. EA-abonnementen worden buiten de monetaire toezegging en worden in rekening gebracht per kwartaal en achteraf in rekening gebracht. De EA-klant zou hebben voor het instellen van een betalingswijze wordt gebruikt, zoals een creditcard betaalt voor services van derden marketplace.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Waar kan ik zien dat de kosten voor ClearDB-resources in een EA-abonnement?
Azure Marketplace-kosten zijn voor Direct EA-klanten, zichtbaar in de Enterprise Portal. Houd er rekening mee dat alle marketplace-aankopen in rekening buiten de monetaire toezegging gebracht worden en -verbruik in rekening gebracht per kwartaal en achteraf in rekening gebracht worden. EA-klanten betalen rechtstreeks naar de externe serviceproviders en kunnen dit doen door in te schakelen van een betalingswijze wordt gebruikt, zoals een creditcard is geregistreerd via hun EA.

Indirect EA-klanten hun Azure Marketplace-abonnementen kunnen vinden op de **abonnementen beheren** pagina van de Enterprise-Portal, maar de prijzen wordt verborgen. Klanten moeten contact opnemen met hun LSP voor informatie over de marketplace-kosten.

Toegang tot Azure Marketplace voor services van derden kan worden beheerd door de beheerders van uw Azure EA-inschrijving. Ze kunnen in- of uitschakelen opnieuw toegang 3e partij aankopen uit de Store in het beheren van Accounts en abonnementen onder het gedeelte Accounts in de Enterprise Portal.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Wie moet ik contact opnemen voor vragen over mijn factuur voor ClearDB-services in mijn EA-abonnement?
Neem contact op met [Enterprise Customer Support](https://aka.ms/AzureEntSupport) met betrekking tot facturering onder hun EA-inschrijving. Het ondersteuningsteam voor EA-Portal, wordt uw vraag worden beantwoord of uw probleem op te lossen.

## <a name="more-information"></a>Meer informatie
[Veelgestelde vragen over Azure Marketplace](https://azure.microsoft.com/marketplace/faq/)

