---
title: Algemene beheertaken voor cloud-service | Microsoft Docs
description: Informatie over het beheren van Cloud-Services in de Azure portal. Deze voorbeelden worden de Azure portal gebruiken.
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
ms.openlocfilehash: e60bf5c82e68d49abaa44d80ac9fafba2d8265da
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="manage-cloud-services-in-the-azure-portal"></a>Cloudservices beheren in de Azure portal
In de **Cloudservices** gebied van de Azure portal kunt u:

* De functie van een service of een implementatie bijwerken.
* Een gefaseerde implementatie naar productie promoten.
* Koppelen aan uw cloudservice bronnen zodat u kunt de resourceafhankelijkheden zien en de resources samen te schalen.
* Verwijderen van een cloudservice of een implementatie.

Zie voor meer informatie over het schalen van uw cloudservice [configureren voor een cloudservice in de portal automatisch schalen](cloud-services-how-to-scale-portal.md).

## <a name="update-a-cloud-service-role-or-deployment"></a>Bijwerken van een cloud service-rol of de implementatie
Als u de toepassingscode voor uw cloudservice bijwerken moet, gebruikt u **bijwerken** op de blade cloud-service. U kunt een enkele of alle functies bijwerken. Als u wilt bijwerken, kunt u een nieuw servicepakket of het serviceconfiguratiebestand uploaden.

1. In de [Azure-portal][Azure portal], selecteer de cloudservice die u wilt bijwerken. Deze stap wordt de cloud service-exemplaar blade geopend.

2. Selecteer op de blade **Update**.

    ![Bijwerkknop](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Werk de implementatie met een nieuwe service-pakketbestand (.cspkg) en service-configuratiebestand (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. Eventueel het storage-account en het implementatielabel bijwerken.

5. Als geen rollen slechts één rolexemplaar hebben, selecteert u de **toch implementeren als een of meer rollen met één exemplaar** selectievakje in zodat de upgrade om door te gaan.

    Azure biedt alleen de beschikbaarheid van 99,95%-service tijdens een cloud service-update als elke rol ten minste twee rolexemplaren (virtuele machines heeft). Een virtuele machine verwerkt aanvragen van clients met twee rolexemplaren, terwijl de andere is bijgewerkt.

6. Selecteer de **implementatie Start** selectievakje voor het toepassen van de update nadat het uploaden van het pakket is voltooid.

7. Selecteer **OK** om te beginnen met het bijwerken van de service.

## <a name="swap-deployments-to-promote-a-staged-deployment-to-production"></a>Implementaties voor een gefaseerde implementatie naar productie promoten wisselen
Als u beslist een nieuwe release van een cloudservice, fase implementeren en testen van uw nieuwe release in uw cloud service-testomgeving. Gebruik **wisselen** overschakelen van de URL's waarmee de twee implementaties worden aangepakt en een nieuwe release naar productie promoten.

U kunt wisselen implementaties van de **Cloudservices** pagina of het dashboard.

1. In de [Azure-portal][Azure portal], selecteer de cloudservice die u wilt bijwerken. Deze stap wordt de cloud service-exemplaar blade geopend.

2. Selecteer op de blade **wisselen**.

    ![Knop voor cloud Services wisselen](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Hiermee opent u de volgende bevestiging wordt gevraagd:

    ![Wisseling van cloud-Services](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Nadat u de informatie over de implementatie controleren, selecteert u **OK** wisselen van de implementaties.

    Het wisselen van de implementatie plaatsvindt snel omdat het enige dat waardoor het virtuele IP-adressen (VIP's) voor de implementaties.

    Als u wilt opslaan compute-kosten, kunt u de staging-implementatie verwijderen nadat u hebt gecontroleerd of uw productie-implementatie werkt zoals verwacht.

### <a name="common-questions-about-swapping-deployments"></a>Veelgestelde vragen over het wisselen van implementaties

**Wat zijn de vereisten voor implementaties wisselen?**

Er zijn twee belangrijke vereisten voor een geslaagde implementatie wisseling:

- Als u een statisch IP-adres voor de productiesite gebruiken wilt, moet u één voor de faseringsite ook reserveren. Anders werkt de wisseling niet.

- Alle exemplaren van uw rollen moeten worden uitgevoerd voordat u de swap kunt uitvoeren. U kunt de status van uw exemplaren controleren op de **overzicht** blade van de Azure-portal. U kunt ook de [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) opdracht in Windows PowerShell.

Houd er rekening mee dat Gast OS-updates en service operations ook herstel leiden implementatie worden verwisseld tot kunnen mislukken. Zie voor meer informatie [problemen met cloud service-implementatie oplossen](cloud-services-troubleshoot-deployment-problems.md).

**Heeft een wisseling gevolgen voor de uitvaltijd voor de toepassing? Hoe moet ik verwerken?**

Zoals beschreven in de vorige sectie, is een wisseling implementatie doorgaans snelle omdat het een configuratiewijziging in de Azure load balancer. In sommige gevallen kan duren 10 of meer seconden en resultaat in de tijdelijke verbindingsfouten. Om te beperken van invloed op uw klanten, Overweeg de implementatie van [client Pogingslogica](../best-practices-retry-general.md).

## <a name="delete-deployments-and-a-cloud-service"></a>Verwijderen van implementaties en een cloudservice
Voordat u een cloudservice verwijderen kunt, moet u elke bestaande implementatie verwijderen.

Als u wilt opslaan compute-kosten, kunt u de staging-implementatie verwijderen nadat u hebt gecontroleerd of uw productie-implementatie werkt zoals verwacht. U wordt gefactureerd voor compute-kosten voor geïmplementeerde rolexemplaren die zijn gestopt.

Gebruik de volgende procedure om een implementatie of de cloudservice te verwijderen.

1. In de [Azure-portal][Azure portal], selecteer de cloudservice die u wilt verwijderen. Deze stap wordt de cloud service-exemplaar blade geopend.

2. Selecteer op de blade **verwijderen**.

    ![Knop voor cloud-Services verwijderen](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. De volledige in de cloud als service wilt verwijderen, selecteert u de **Cloud-service en implementaties** selectievakje. Of u kunt ofwel de **productie-implementatie** of de **voorbereide distributie** selectievakje.

    ![Cloud Services Delete](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Selecteer **verwijderen** onderaan.

5. Selecteer wilt verwijderen van de cloudservice **cloudservice verwijderen**. Selecteer vervolgens de bevestigingsprompt **Ja**.

> [!NOTE]
> Wanneer een cloudservice is verwijderd en uitgebreide bewaking is geconfigureerd, moet u de gegevens handmatig verwijderen uit uw storage-account. Zie voor meer informatie over waar u het vinden van de metrische gegevens tabellen [Inleiding in de cloud servicecontrole](cloud-services-how-to-monitor.md).


## <a name="find-more-information-about-failed-deployments"></a>Meer informatie over mislukte implementaties
De **overzicht** blade bevat een statusbalk aan de bovenkant. Wanneer u de balk selecteert, wordt een nieuwe blade wordt geopend en wordt alle informatie over de fout weergegeven. Als de implementatie geen fouten bevat, wordt de blade informatie is leeg.

![Cloud Services-overzicht](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Volgende stappen
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Meer informatie over hoe [implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md).
* Configureer [SSL-certificaten](cloud-services-configure-ssl-certificate-portal.md).
