---
title: Veelvoorkomende cloudservicebeheertaken | Microsoft Docs
description: Leer hoe u Cloud Services beheren in Azure portal. Deze voorbeelden gebruiken de Azure-portal.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeconnoc
ms.openlocfilehash: e9f4153c68f0a2a4ce83f900ff63152311163ff6
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42056825"
---
# <a name="manage-cloud-services-in-the-azure-portal"></a>Cloud Services beheren in Azure portal
In de **Cloudservices** gebied van Azure portal, kunt u:

* Een rol- of een implementatie bijwerken.
* Een gefaseerde implementatie naar productie promoten.
* Resources koppelen aan uw cloudservice zodat u kunt zien van de resourceafhankelijkheden en de resources die samen worden geschaald.
* Een cloudservice of een implementatie verwijderen.

Zie voor meer informatie over hoe u uw cloudservice schalen [automatische schaling voor een cloudservice in de portal configureren](cloud-services-how-to-scale-portal.md).

## <a name="update-a-cloud-service-role-or-deployment"></a>Een cloud service-rol of de implementatie bijwerken
Als u de code van de toepassing voor je cloudservice bijwerken moet, gebruikt u **bijwerken** op de cloud service-blade. U kunt één functie of alle rollen bijwerken. Als u wilt bijwerken, kunt u een nieuwe service-pakket of configuratiebestand uploaden.

1. In de [Azure-portal][Azure portal], selecteert u de cloudservice die u wilt bijwerken. Deze stap opent u de blade van cloud service-exemplaar.

2. Selecteer op de blade **Update**.

    ![Bijwerkknop](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Werk de implementatie met een nieuwe service-pakketbestand (.cspkg) en het serviceconfiguratiebestand (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. (Optioneel) het opslagaccount en het implementatielabel bijwerken.

5. Als alle rollen slechts één rolexemplaar hebben, selecteert u de **toch implementeren als een of meer rollen met één exemplaar** selectievakje in om in te schakelen van de upgrade om door te gaan.

    Azure biedt alleen de beschikbaarheid van 99,95 procent-service tijdens een cloud service-update als elke rol minimaal twee rolinstanties (virtuele machines heeft). Een virtuele machine verwerkt aanvragen van clients met twee rolexemplaren, terwijl de andere is bijgewerkt.

6. Selecteer de **implementatie starten** selectievakje in om toe te passen van de update nadat het uploaden van het pakket is voltooid.

7. Selecteer **OK** om te beginnen met het bijwerken van de service.

## <a name="swap-deployments-to-promote-a-staged-deployment-to-production"></a>Wisselen van implementaties voor een gefaseerde implementatie naar productie promoten
Als u beslist naar een nieuwe versie van een cloudservice, fase implementeren en testen van uw nieuwe versie in de faseringsomgeving van cloud-service. Gebruik **wisselen** om over te schakelen van de URL's waarmee de twee implementaties worden behandeld en een nieuwe release naar productie promoten.

U kunt wisselen-implementaties van de **Cloudservices** pagina of het dashboard.

1. In de [Azure-portal][Azure portal], selecteert u de cloudservice die u wilt bijwerken. Deze stap opent u de blade van cloud service-exemplaar.

2. Selecteer op de blade **wisselen**.

    ![Knop voor cloud Services wisselen](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Hiermee opent u de volgende bevestiging wordt gevraagd:

    ![Wisselen van cloud Services](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Nadat u de informatie over de implementatie controleren, selecteert u **OK** wisselen van de implementaties.

    De implementatie-swap gebeurt snel omdat het enige wat wijzigingen het virtuele IP-adressen (VIP's is) voor de implementaties.

    Om computerkosten te besparen, kunt u de staging-implementatie verwijderen nadat u hebt gecontroleerd of uw productie-implementatie werkt zoals verwacht.

### <a name="common-questions-about-swapping-deployments"></a>Veelgestelde vragen over het wisselen van implementaties

**Wat zijn de vereisten voor het wisselen van implementaties?**

Er zijn twee belangrijke vereisten voor een geslaagde implementatie wisselen:

- Als u een statisch IP-adres voor uw productiesite gebruiken wilt, moet u één voor de staging-site ook reserveren. Anders mislukt de wisseling.

- Alle exemplaren van de rollen moeten worden uitgevoerd voordat u de wisseling kunt uitvoeren. U kunt de status van uw instanties controleren op de **overzicht** blade van de Azure-portal. U kunt ook kunt u de [Get-AzureRole](/powershell/module/servicemanagement/azure/get-azurerole?view=azuresmps-3.7.0) opdracht in Windows PowerShell.

Houd er rekening mee dat Gast-OS-updates en service healing bewerkingen ook leiden implementatie swaps tot kunnen mislukken. Zie voor meer informatie, [problemen met cloud service-implementatie oplossen](cloud-services-troubleshoot-deployment-problems.md).

**Heeft een wisseling gevolgen voor de downtime voor mijn toepassing? Hoe moet ik verwerken?**

Zoals wordt beschreven in de vorige sectie, is een implementatie wisselen doorgaans snelle omdat het een configuratiewijziging in de Azure load balancer. Het kan in sommige gevallen, 10 of meer seconden en resultaat duren in tijdelijke verbindingsfouten. Als u wilt beperken de gevolgen voor uw klanten, Overweeg de implementatie van [client Pogingslogica](../best-practices-retry-general.md).

## <a name="delete-deployments-and-a-cloud-service"></a>Implementaties en een service in de cloud verwijderen
Voordat u een cloudservice verwijderen kunt, moet u elke bestaande implementatie verwijderen.

Om computerkosten te besparen, kunt u de staging-implementatie verwijderen nadat u hebt gecontroleerd of uw productie-implementatie werkt zoals verwacht. U wordt gefactureerd voor de kosten voor rekenuren voor geïmplementeerde rolinstanties die zijn gestopt.

Gebruik de volgende procedure om een implementatie of uw service in de cloud te verwijderen.

1. In de [Azure-portal][Azure portal], selecteert u de cloudservice die u wilt verwijderen. Deze stap opent u de blade van cloud service-exemplaar.

2. Selecteer op de blade **verwijderen**.

    ![Knop voor cloud Services verwijderen](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Selecteer wilt verwijderen van de volledige in de cloud-service de **Cloud-service en de implementaties** selectievakje. Of u kunt ofwel de **productie-implementatie** of de **Staging-implementatie** selectievakje.

    ![Cloud Services verwijderen](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Selecteer **verwijderen** aan de onderkant.

5. Selecteer wilt verwijderen van de cloudservice **cloudservice verwijderen**. Selecteer vervolgens op de bevestiging wordt gevraagd, **Ja**.

> [!NOTE]
> Als een cloudservice is verwijderd en uitgebreide bewaking is geconfigureerd, moet u de gegevens handmatig verwijderen van uw opslagaccount. Zie voor meer informatie over het zoeken van de tabellen voor metrische gegevens naar [Inleiding in de cloud servicecontrole](cloud-services-how-to-monitor.md).


## <a name="find-more-information-about-failed-deployments"></a>Meer informatie over mislukte implementaties
De **overzicht** blade bevat een statusbalk weergegeven aan de bovenkant. Wanneer u de balk selecteert, wordt een nieuwe blade wordt geopend en wordt foutinformatie wordt weergegeven. Als de implementatie geen fouten bevat, wordt de blade informatie is leeg.

![Overzicht van cloud Services](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Volgende stappen
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Meer informatie over het [implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md).
* Configureer [SSL-certificaten](cloud-services-configure-ssl-certificate-portal.md).
