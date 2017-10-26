---
title: Azure Security Center en Azure Virtual Machines | Microsoft Docs
description: In dit document wordt uitgelegd hoe virtuele machines van Azure kunnen worden beveiligd met Azure Security Center.
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5fe5a12c-5d25-430c-9d47-df9438b1d7c5
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/24/2017
ms.author: yurid
ms.openlocfilehash: 137ed1c65d2eb619192364dd7d0d17a55d98403c
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2017
---
# <a name="azure-security-center-and-azure-virtual-machines"></a>Azure Security Center en Azure Virtual Machines
[Azure Security Center](https://azure.microsoft.com/services/security-center/) helpt u bedreigingen te voorkomen, te detecteren en erop te reageren. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

In dit artikel wordt gedemonstreerd hoe Security Center u kan helpen uw VM (virtuele machine) van Azure te beveiligen.

## <a name="why-use-security-center"></a>Waarom Security Center gebruiken?
Met Security Center kunt u gegevens van virtuele machines in Azure beveiligen door inzicht te bieden in de beveiligingsinstellingen van de virtuele machine. Wanneer VM's worden beveiligd via Security Center, zijn de volgende mogelijkheden beschikbaar:

* Beveiligingsinstellingen voor het besturingssysteem met de aanbevolen configuratieregels
* Updates voor systeembeveiliging en essentiële updates die ontbreken
* Aanbevelingen voor eindpuntbeveiliging
* Validatie voor schijfversleuteling
* Beoordeling en herstel van beveiligingslekken
* Detectie van bedreigingen

Naast het beveiligen van Azure VM's biedt Security Center ook beveiligingscontrole en -beheer voor cloudservices, app-services, virtuele netwerken en meer. 

> [!NOTE]
> Zie [Introduction to Azure Security Center](security-center-intro.md) (Inleiding tot Azure Security Center) voor meer informatie over Azure Security Center.
> 
> 

## <a name="prerequisites"></a>Vereisten
Als u aan de slag wilt met Azure Security Center, moet u aan het volgende voldoen:

* U moet een abonnement hebben op Microsoft Azure. Zie [Prijzen van Beveiligingscentrum](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie over de gratis laag en standaardlaag in Security Center.
* Plan de invoer van Security Center. Zie [Plannings- en bedieningsgids voor het Beveiligingscentrum](security-center-planning-and-operations-guide.md) voor meer informatie over plannings- en bedieningsoverwegingen.
* Zie [Azure Security Center frequently asked questions (FAQ)](security-center-faq.md) (Veelgestelde vragen over Azure Security Center) voor informatie over de ondersteuning van besturingssystemen. 

## <a name="set-security-policy"></a>Beveiligingsbeleid instellen
Gegevensverzameling moet zijn ingeschakeld, zodat met Azure Security Center de informatie kan worden verkregen die nodig is om aanbevelingen en waarschuwingen te bieden die zijn gegenereerd op basis van het beveiligingsbeleid dat u configureert. In de onderstaande afbeelding ziet u dat **Gegevensverzameling** is ingesteld op **Aan**.

Een beveiligingsbeleid bepaalt welke set controles wordt aanbevolen voor resources binnen het opgegeven abonnement of de opgegeven resourcegroep. Voordat u het beveiligingsbeleid inschakelt, moet gegevensverzameling zijn ingeschakeld. Met Security Center worden gegevens van uw virtuele machines verzameld om de beveiligingsstatus van de VM's te beoordelen, aanbevelingen voor beveiliging te bieden en u te waarschuwen bij dreigingen. In Security Center definieert u beleid voor Azure-abonnementen of -resourcegroepen overeenkomstig de behoeften van uw bedrijf en het type toepassingen of de vertrouwelijkheid van de gegevens in elk abonnement. 

![Beveiligingsbeleid](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

> [!NOTE]
> Zie het artikel [Beveiligingsbeleid instellen](security-center-policies.md) voor meer informatie over elk **Preventiebeleid**.
> 
> 

## <a name="manage-security-recommendations"></a>Aanbevelingen voor beveiliging beheren
De beveiligingsstatus van uw Azure-resources wordt gecontroleerd met Security Center. Wanneer met Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen gedaan. Deze aanbevelingen begeleiden u bij het configureren van de benodigde besturingselementen.

Nadat er een beveiligingsbeleid is ingesteld, wordt met Security Center de beveiligingsstatus van de Azure-resources geanalyseerd om potentiële beveiligingsproblemen op te sporen. De aanbevelingen worden weergegeven in tabelindeling, waarbij elke regel één bepaalde aanbeveling vertegenwoordigt. In de onderstaande tabel ziet u een aantal voorbeelden van aanbevelingen voor Azure VM's en informatie over wat er gebeurt als u deze toepast. Wanneer u een aanbeveling selecteert, wordt er informatie weergegeven waarin staat hoe u de aanbeveling kunt implementeren in Security Center.

| Aanbeveling | Beschrijving |
| --- | --- |
| [Gegevensverzameling voor abonnementen inschakelen](security-center-enable-data-collection.md) |Hiermee wordt aanbevolen om gegevensverzameling in te schakelen in het beveiligingsbeleid voor elk van de abonnementen en voor alle VM's (virtuele machines) in uw abonnementen. |
| [Beveiligingsproblemen met het besturingssysteem herstellen](security-center-remediate-os-vulnerabilities.md) |Hiermee wordt aanbevolen om de configuraties voor het besturingssysteem uit te lijnen met de aanbevolen configuratieregels, bijvoorbeeld niet toestaan dat wachtwoorden worden opgeslagen. |
| [Systeemupdates toepassen](security-center-apply-system-updates.md) |Aanbeveling voor het implementeren van ontbrekende updates voor systeembeveiliging en essentiële updates op VM's. |
| [Opnieuw opstarten na systeemupdates](security-center-apply-system-updates.md#reboot-after-system-updates) |Hiermee wordt aanbevolen om een VM opnieuw op te starten om het proces van het toepassen van systeemupdates te voltooien. |
| [Eindpuntbeveiliging installeren](security-center-install-endpoint-protection.md) |Hiermee wordt aanbevolen om antimalwareprogramma's op VM's te installeren. (Alleen voor Windows-VM's.) |
| [VM-agent inschakelen](security-center-enable-vm-agent.md) |Hiermee kunt u zien voor welke VM's de VM-agent is vereist. De VM-agent moet zijn geïnstalleerd op VM's om patches en basislijnen te scannen en antimalwareprogramma's uit te voeren. De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Het artikel [VM Agent and Extensions – Part 2)](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (VM-agent en -extensies – deel 2) bevat informatie over het installeren van de VM-agent. |
| [Schijfversleuteling toepassen](security-center-apply-disk-encryption.md) |Hiermee wordt aanbevolen om de VM-schijven te versleutelen met behulp van Azure Disk Encryption. (Voor VM's van Windows en Linux.) Versleuteling wordt aanbevolen voor het besturingssysteem en voor de gegevensvolumes op de VM. |
| [Beoordeling van beveiligingslekken is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) |Hiermee wordt aanbevolen om een oplossing voor de beoordeling van beveiligingslekken te installeren op de VM. |
| [Beveiligingsproblemen herstellen](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Hiermee ziet u beveiligingsproblemen in het systeem en in de toepassing die zijn gedetecteerd met de oplossing voor beveiligingslekken die op de VM is geïnstalleerd. |

> [!NOTE]
> Zie het artikel [Managing security recommendations](security-center-recommendations.md) (Aanbevelingen voor beveiliging beheren) voor meer informatie over aanbevelingen.
> 
> 

## <a name="monitor-security-health"></a>Beveiligingsstatus controleren
Nadat u een [beveiligingsbeleid](security-center-policies.md) voor de resources van een abonnement hebt ingeschakeld, analyseert Security Center de beveiliging van uw resources om mogelijke beveiligingsproblemen op te sporen.  U kunt de beveiligingsstatus van uw resources samen met eventuele problemen bekijken op de blade **Beveiligingsstatus van de resource**. Wanneer u op de statustegel **Resourcebeveiliging** klikt op **Virtuele machines**, wordt de blade **Virtuele machines** geopend met de aanbevelingen voor uw VM's. 

![Beveiligingsstatus](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Beveiligingswaarschuwingen beheren en erop reageren
Security Center verzamelt, analyseert en integreert automatisch logboekgegevens van uw Azure-resources, het netwerk en verbonden partneroplossingen, zoals firewall- en eindpuntbeveiligingsoplossingen om werkelijke dreigingen te detecteren en fout-positieven te reduceren. Door gebruik te maken van een combinatie van [detectiemogelijkheden](security-center-detection-capabilities.md), kan Security Center beveiligingswaarschuwingen genereren waarmee u het probleem snel kunt onderzoeken en waarmee aanbevelingen kunnen worden geboden voor het herstellen van mogelijke aanvallen.

![Beveiligingswaarschuwingen](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Selecteer een beveiligingswaarschuwing voor meer informatie over de gebeurtenis(sen) waarmee de waarschuwing is geactiveerd en welke stappen u zo nodig moet uitvoeren om een aanval te verhelpen. Beveiligingswaarschuwingen zijn gegroepeerd op [type](security-center-alerts-type.md) en datum.

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.

