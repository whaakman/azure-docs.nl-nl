---
title: Algemene cloud service-beheertaken (klassiek) | Microsoft Docs
description: Informatie over het beheren van cloudservices in de klassieke Azure portal.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 41a30e50-067c-485b-96fd-434a6d057abc
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 2ee76dfcb579e53975b1f61a6590f8d78dc0961b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-cloud-services"></a>Cloudservices beheren
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-manage-portal.md)
> * [Klassieke Azure Portal](cloud-services-how-to-manage.md)
>
>

In de **Cloudservices** gebied van de klassieke Azure portal, kunt u de functie van een service of een implementatie bijwerken, een gefaseerde implementatie naar productie promoten, resources koppelen aan uw cloudservice zodat de bronafhankelijkheden Zie en schalen van de resources samen en verwijderen van een cloudservice of een implementatie.

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Procedure: een functieservice van de cloud of implementatie bijwerken
Als u de toepassingscode voor uw cloudservice bijwerken moet, gebruikt u **bijwerken** op het dashboard **Cloudservices** pagina of **exemplaren** pagina. U kunt een enkele of alle functies bijwerken. U moet een nieuwe servicepakket en serviceconfiguratiebestand uploaden.

1. In de [klassieke Azure-portal](https://manage.windowsazure.com/), op het dashboard **Cloudservices** pagina of **exemplaren** pagina, klikt u op **Update**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. In **implementatielabel**, voer een unieke naam voor de implementatie (bijvoorbeeld mycloudservice4). U vindt het implementatielabel onder **snel starten** op het dashboard.
3. In **pakket**, gebruik **Bladeren** voor het uploaden van het pakketbestand (.cspkg).
4. In **configuratie**, gebruik **Bladeren** voor het uploaden van het service-configuratiebestand (.cscfg).
5. In **rol**, selecteer **alle** als u wilt upgraden van alle functies in de cloudservice. Als u wilt een enkele rol bijwerkt, selecteer de rol die u wilt bijwerken. Zelfs als u een specifieke rol bijwerken selecteert, worden de updates in het serviceconfiguratiebestand worden toegepast op alle rollen.
6. Als de update van het aantal rollen of de grootte van een rol wijzigt, selecteert u de **update toestaan als grootten van de functie of het aantal rollen verandert** selectievakje in zodat de update om door te gaan.

    Let wel dat als u de grootte van een rol (dat wil zeggen, de grootte van een virtuele machine die als host fungeert voor een rolinstantie) of het aantal rollen wijzigt, elke rolinstantie (virtuele machine) siteservercomputer moet, en alle lokale gegevens gaan verloren.

7. Als geen service-rollen slechts één rolexemplaar, selecteert u de **bijwerken, zelfs als een of meer rollen bevatten een selectievakje voor één exemplaar** inschakelen van de upgrade om door te gaan.

    Azure kan alleen garanderen van 99,95% servicebeschikbaarheid tijdens een cloud service-update als elke rol ten minste twee rolexemplaren (virtuele machines heeft). Waarmee één virtuele machine voor het verwerken van aanvragen van clients, terwijl de andere wordt bijgewerkt.

8. Klik op **OK** (ingeschakeld) om te beginnen met het bijwerken van de service.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Hoe: wisselen implementaties voor een gefaseerde implementatie naar productie promoten
Gebruik **wisselen** te promoveren van een gefaseerde installatie-implementatie van een cloudservice voor productie. Wanneer u beslist voor het implementeren van een nieuwe release van een cloudservice, kunt u Faseren en testen van uw nieuwe release in uw cloud service-testomgeving terwijl uw klanten met de huidige release in productie. Wanneer u klaar bent voor de nieuwe release naar productie promoten, kunt u **wisselen** overschakelen van de URL's waarmee de twee implementaties worden aangepakt.

U kunt wisselen implementaties van de **Cloudservices** pagina of het dashboard.

1. In de [klassieke Azure-portal](https://manage.windowsazure.com/), klikt u op **Cloudservices**.
2. In de lijst met cloudservices, klikt u op de cloudservice om deze te selecteren.
3. Klik op **wisselen**.

    Hiermee opent u de volgende bevestiging wordt gevraagd.

    ![Wisseling van cloud-Services](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Nadat u de informatie over de implementatie controleren, klikt u op **Ja** wisselen van de implementaties.

    Het wisselen van de implementatie plaatsvindt snel omdat het enige dat waardoor het virtuele IP-adressen (VIP's) voor de implementaties.

    Als u wilt opslaan compute-kosten, kunt u de implementatie in de testomgeving verwijderen als u zeker weet dat de nieuwe productie-implementatie wordt uitgevoerd zoals verwacht.

### <a name="common-questions-about-swapping-deployments"></a>Veelgestelde vragen over het wisselen van implementaties

**Wat zijn de vereisten voor implementaties wisselen?**

Er zijn twee belangrijke vereisten voor een geslaagde implementatie wisseling:

- Als u gebruiken een statische IP-adres voor de productiesite wilt, moet u één voor de faseringsite ook reserveren. Anders mislukt de wisselruimte.

- Alle exemplaren van uw rollen moeten worden uitgevoerd voordat u de swap kunt uitvoeren. U kunt de status van uw exemplaren in de klassieke Azure portal of met behulp van controleren [de opdracht Get-AzureRole in Windows PowerShell](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0).

Houd er rekening mee dat Gast OS-updates en herstel van de operations-service kunnen ook worden veroorzaakt worden verwisseld implementatie mislukken. Zie [problemen met cloud service-implementatie oplossen](cloud-services-troubleshoot-deployment-problems.md) voor meer informatie.

**Heeft een wisseling gevolgen voor de uitvaltijd voor de toepassing? Hoe moet ik verwerken?**

Zoals beschreven in de laatste sectie, is een wisseling implementatie meestal erg snel omdat het een configuratiewijziging in de Azure load balancer. In sommige gevallen kan echter kunt tien of meer seconden duren en leiden tot tijdelijke verbindingsfouten. Om te beperken van invloed op uw klanten, Overweeg de implementatie van [client Pogingslogica](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Procedure: een bron koppelen aan een cloudservice
Om de afhankelijkheden van uw Cloud Services van andere bronnen aan te geven, kunt u een Azure SQL Database-instantie of een opslagaccount koppelen aan de Cloud Service. U kunt koppelen en ontkoppelen van bronnen op de **gekoppelde Resources** pagina en vervolgens controleren hun gebruik op het servicedashboard van de cloud. Als een gekoppeld opslagaccount bewaking ingeschakeld heeft, kunt u totaal aantal aanvragen dat op het servicedashboard cloud bewaken.

Gebruik **koppeling** een nieuwe of bestaande SQL-Database-exemplaar of storage-account koppelen aan uw service in de cloud. U kunt vervolgens de database samen met de functie van de cloud-service die wordt gebruikt voor schalen de **Scale** pagina. (Een opslagaccount schaalt automatisch als gebruik toeneemt.) Zie voor meer informatie [schalen van een Cloudservice en de gekoppelde Resources](cloud-services-how-to-scale.md).

U ook kunt bewaken, beheren en schalen van de database in de **Databases** knooppunt van de klassieke Azure portal.

Een resource in deze zin ' koppelen', niet uw app verbinden met de resource. Als u een nieuwe database via maakt **koppeling**, moet u de verbindingsreeksen toevoegen aan uw toepassingscode en werk vervolgens de cloudservice. U moet ook verbindingsreeksen toevoegen als uw app gebruikmaakt van resources in een gekoppelde opslagaccount.

De volgende procedure beschrijft hoe een nieuw exemplaar van het SQL-Database geïmplementeerd op een nieuwe SQL-Database-server op een cloudservice gekoppeld.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Een SQL Database-exemplaar koppelen aan een cloudservice
1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**. Klik vervolgens op de naam van de cloudservice om het dashboard te openen.
2. Klik op **gekoppelde Resources**.

    De **gekoppelde Resources** pagina wordt geopend.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klik op **koppelen van een Resource** of **koppeling**.

    De **koppeling Resource** wizard wordt gestart.

    ![Koppeling pagina 1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klik op **Maak een nieuwe resource** of **koppelen van een bestaande resource**.
5. Kies het type resource te koppelen. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **SQL-Database**. (Alleen de klassieke Azure portal ondersteunt een storage-account koppelen aan een cloudservice.)
6. Voor het voltooien van de databaseconfiguratie, volg de instructies in de help voor de **SQL-Databases** gebied van de klassieke Azure portal.

    U kunt de voortgang van de koppelingsbewerking in het berichtgedeelte volgen.

    Wanneer het koppelen is voltooid, kunt u de status van de gekoppelde resources op het servicedashboard van de cloud controleren. Zie voor meer informatie over het schalen van een gekoppelde SQL-Database [schalen van een Cloudservice en de gekoppelde Resources](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Ontkoppelen van een gekoppelde resource
1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**. Klik vervolgens op de naam van de cloudservice om het dashboard te openen.
2. Klik op **gekoppelde Resources**, en selecteer vervolgens de resource.
3. Klik op **ontkoppelen**. Klik vervolgens op **Ja** op bevestiging wordt gevraagd.

    Ontkoppelen van een SQL-Database heeft geen effect op de database of verbindingen met de database van de toepassing. U kunt nog steeds beheren met de database in de **SQL-Databases** gebied van de klassieke Azure portal.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Hoe: implementaties en een cloudservice verwijderen
Voordat u een cloudservice verwijderen kunt, moet u elke bestaande implementatie verwijderen.

Als u wilt opslaan compute-kosten, kunt u uw implementatie test verwijderen nadat u hebt gecontroleerd of uw productie-implementatie werkt zoals verwacht. U bent gefactureerd compute-kosten voor rolinstanties, zelfs als een cloudservice wordt niet uitgevoerd.

Gebruik de volgende procedure om een implementatie of de cloudservice te verwijderen.

1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**.
2. Selecteer de cloudservice en klik vervolgens op **verwijderen**. (U selecteert een cloudservice zonder het dashboard te openen, klikt u op overal behalve de naam in de cloud service-vermelding.)

    Als u een implementatie in test- of productieomgeving hebt, ziet u een menu met opties die lijkt op het volgende aan de onderkant van het venster. Voordat u de cloudservice verwijderen kunt, moet u eventuele bestaande implementaties verwijderen.

    ![Menu verwijderen](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)

3. Als u wilt verwijderen van een implementatie, klikt u op **productie-implementatie verwijderen** of **verwijderen van fasering implementatie**. Op de bevestiging wordt gevraagd, klikt u vervolgens **Ja**.
4. Als u verwijderen van de cloudservice wilt, herhaalt u stap 3, indien nodig, verwijderen van uw andere implementatie.
5. Als u wilt verwijderen van de cloudservice, klikt u op **cloudservice verwijderen**. Op de bevestiging wordt gevraagd, klikt u vervolgens **Ja**.

> [!NOTE]
> Als uitgebreide bewaking is geconfigureerd voor uw cloudservice, verwijdert Azure niet de bewakingsgegevens van uw opslagaccount wanneer u de cloudservice verwijderen. U moet de gegevens handmatig verwijderen. Zie voor meer informatie over waar u de tabellen metrische gegevens vinden ' How to: toegang uitgebreide bewaking gegevens buiten de klassieke Azure portal ' in [hoe Monitor Cloud Services](cloud-services-how-to-monitor.md).
>
>

## <a name="next-steps"></a>Volgende stappen
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure.md).
* Meer informatie over hoe [implementeren van een cloudservice](cloud-services-how-to-create-deploy.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name.md).
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate.md).
