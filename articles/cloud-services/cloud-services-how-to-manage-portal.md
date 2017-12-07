---
title: Algemene beheertaken voor cloud-service | Microsoft Docs
description: Informatie over het beheren van cloudservices in de Azure portal. Deze voorbeelden worden de Azure portal gebruiken.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: a06becda42541b22d3e090087dd96cd08c980f44
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="how-to-manage-cloud-services"></a>Cloudservices beheren
In de **Cloudservices** gebied van de Azure portal, kunt u de functie van een service of een implementatie bijwerken, een gefaseerde implementatie naar productie promoten, resources koppelen aan uw cloudservice, zodat u de bronafhankelijkheden en schaal ziet de resources samen en verwijderen van een cloud service of een implementatie.

Meer informatie over het schalen van uw cloudservice is beschikbaar [hier](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Procedure: een functieservice van de cloud of implementatie bijwerken
Als u de toepassingscode voor uw cloudservice bijwerken moet, gebruikt u **bijwerken** op de blade cloud-service. U kunt een enkele of alle functies bijwerken. Als u wilt bijwerken, kunt u een nieuw servicepakket of het serviceconfiguratiebestand uploaden.

1. In de [Azure-portal][Azure portal], selecteer de cloudservice die u wilt bijwerken. Deze stap wordt de cloud service-exemplaar blade geopend.
2. Klik op de blade de **Update** knop.

    ![Bijwerkknop](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Werk de implementatie met een nieuwe service-pakketbestand (.cspkg) en service-configuratiebestand (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Eventueel** bijwerken van het implementatielabel en het opslagaccount.
5. Als geen rollen slechts één rolexemplaar hebben, selecteert u de **toch implementeren als een of meer rollen met één exemplaar** inschakelen van de upgrade om door te gaan.

    Azure kan alleen garanderen van 99,95% servicebeschikbaarheid tijdens een cloud service-update als elke rol ten minste twee rolexemplaren (virtuele machines heeft). Een virtuele machine verwerkt aanvragen van clients met twee rolexemplaren, terwijl de andere is bijgewerkt.

6. Controleer **implementatie Start** om de update is toegepast, nadat het uploaden van het pakket is voltooid.
7. Klik op **OK** om te beginnen met het bijwerken van de service.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Hoe: wisselen implementaties voor een gefaseerde implementatie naar productie promoten
Als u beslist een nieuwe release van een cloudservice, fase implementeren en testen van uw nieuwe release in uw cloud service-testomgeving. Gebruik **wisselen** overschakelen van de URL's waarmee de twee implementaties worden aangepakt en een nieuwe release naar productie promoten.

U kunt wisselen implementaties van de **Cloudservices** pagina of het dashboard.

1. In de [Azure-portal][Azure portal], selecteer de cloudservice die u wilt bijwerken. Deze stap wordt de cloud service-exemplaar blade geopend.
2. Klik op de blade de **wisselen** knop.

    ![Wisseling van cloud-Services](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Hiermee opent u de volgende bevestiging wordt gevraagd.

    ![Wisseling van cloud-Services](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Nadat u de informatie over de implementatie controleren, klikt u op **OK** wisselen van de implementaties.

    Het wisselen van de implementatie plaatsvindt snel omdat het enige dat waardoor het virtuele IP-adressen (VIP's) voor de implementaties.

    Als u wilt opslaan compute-kosten, kunt u de staging-implementatie verwijderen nadat u hebt gecontroleerd of uw productie-implementatie werkt zoals verwacht.

### <a name="common-questions-about-swapping-deployments"></a>Veelgestelde vragen over het wisselen van implementaties

**Wat zijn de vereisten voor implementaties wisselen?**

Er zijn twee belangrijke vereisten voor een geslaagde implementatie wisseling:

- Als u gebruiken een statische IP-adres voor de productiesite wilt, moet u één voor de faseringsite ook reserveren. Anders werkt de wisseling niet.

- Alle exemplaren van uw rollen moeten worden uitgevoerd voordat u de swap kunt uitvoeren. U kunt de status van uw exemplaren in de overzichtsblade van de Azure portal controleren. U kunt ook de [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) opdracht in Windows PowerShell.

Houd er rekening mee dat Gastbesturingssysteem updates en service operations herstel kan ook worden veroorzaakt worden verwisseld implementatie mislukken. Zie voor meer informatie [problemen met cloud service-implementatie oplossen](cloud-services-troubleshoot-deployment-problems.md).

**Heeft een wisseling gevolgen voor de uitvaltijd voor de toepassing? Hoe moet ik verwerken?**

Zoals beschreven in de laatste sectie, is een wisseling implementatie doorgaans snelle omdat het een configuratiewijziging in de Azure load balancer. In sommige gevallen kan echter kunt tien of meer seconden duren en leiden tot tijdelijke verbindingsfouten. Om te beperken van invloed op uw klanten, Overweeg de implementatie van [client Pogingslogica](../best-practices-retry-general.md).

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Hoe: implementaties en een cloudservice verwijderen
Voordat u een cloudservice verwijderen kunt, moet u elke bestaande implementatie verwijderen.

Als u wilt opslaan compute-kosten, kunt u de staging-implementatie verwijderen nadat u hebt gecontroleerd of uw productie-implementatie werkt zoals verwacht. U wordt gefactureerd voor compute-kosten voor geïmplementeerde rolexemplaren die zijn gestopt.

Gebruik de volgende procedure om een implementatie of de cloudservice te verwijderen.

1. In de [Azure-portal][Azure portal], selecteer de cloudservice die u wilt verwijderen. Deze stap wordt de cloud service-exemplaar blade geopend.
2. Klik op de blade de **verwijderen** knop.

    ![Wisseling van cloud-Services](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. U kunt de volledige in de cloud-service verwijderen door te controleren **Cloud-service en implementaties** of kies de **productie-implementatie** of de **voorbereide distributie**.

    ![Wisseling van cloud-Services](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Klik op de **verwijderen** knop onderaan.
5. Als u wilt verwijderen van de cloudservice, klikt u op **cloudservice verwijderen**. Op de bevestiging wordt gevraagd, klikt u vervolgens **Ja**.

> [!NOTE]
> Wanneer een cloudservice is verwijderd en uitgebreide bewaking is geconfigureerd, moet u de gegevens handmatig verwijderen uit uw storage-account. Zie voor meer informatie over waar u het vinden van de metrische gegevens tabellen [dit](cloud-services-how-to-monitor.md) artikel.


## <a name="how-to-find-more-information-about-failed-deployments"></a>Hoe: meer informatie over mislukte implementaties
De **overzicht** blade bevat een statusbalk aan de bovenkant. Wanneer u op de werkbalk klikt, wordt een nieuwe blade wordt geopend en wordt alle informatie over de fout weergegeven. Als de implementatie geen fouten bevat, wordt de blade informatie is leeg.

![Wisseling van cloud-Services](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Volgende stappen
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Meer informatie over hoe [implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md).
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md).
