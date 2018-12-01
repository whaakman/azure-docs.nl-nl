---
title: Die gericht is op de beheeroplossingen in Azure | Microsoft Docs
description: Die gericht is op beheeroplossingen, kunt u oplossingen voor gegevensbeheer voor een specifieke set agents beperken.  In dit artikel wordt beschreven hoe u een scopeconfiguratie maken en dit toepassen op een oplossing.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: 53f28d29b9667bb885a5c3d0da8d926f756f3427
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682063"
---
# <a name="targeting-management-solutions-in-azure-preview"></a>Die gericht is op de beheeroplossingen in Azure (Preview)
Wanneer u een oplossing voor uw abonnement toevoegt, wordt deze automatisch geïmplementeerd standaard voor alle Windows en Linux-agents die zijn verbonden met uw Log Analytics-werkruimte.  Mogelijk wilt uw kosten beheren en beperken van de hoeveelheid gegevens die worden verzameld voor een oplossing door deze te beperken tot een bepaalde set van agents.  In dit artikel wordt beschreven hoe u **Oplossingstargeting** dit is een functie waarmee u een bereik toepassen op uw oplossingen.

## <a name="how-to-target-a-solution"></a>Het doel van een oplossing
Er zijn drie stappen voor het die gericht is op een oplossing zoals beschreven in de volgende secties. 


### <a name="1-create-a-computer-group"></a>1. Een computergroep maken
U opgeven dat de computers die u opnemen in een bereik wilt met het maken van een [computergroep](../../azure-monitor/platform/computer-groups.md) in Log Analytics.  De computergroep kan op basis van een zoeken in Logboeken of geïmporteerd uit andere bronnen, zoals Active Directory of WSUS-groepen. Als [die hieronder worden beschreven](#solutions-and-agents-that-cant-be-targeted), alleen de computers die rechtstreeks zijn verbonden met Log Analytics worden opgenomen in het bereik.

U hebt één keer groep op de computer in uw werkruimte hebt gemaakt en u zult deze opnemen in de configuratie van een scope die kan worden toegepast op een of meer oplossingen.
 
 
 ### <a name="2-create-a-scope-configuration"></a>2. Een scopeconfiguratie maken
 Een **scopeconfiguratie** bevat een of meer computergroepen en kan worden toegepast op een of meer oplossingen. 
 
 De configuratie van een bereik met behulp van het volgende proces maken.  

 1. In de Azure-portal, gaat u naar **Log Analytics** en selecteert u uw werkruimte.
 2. In de eigenschappen voor de werkruimte onder **gegevensbronnen voor werkruimte** Selecteer **Scopeconfiguraties**.
 3. Klik op **toevoegen** om een nieuwe scopeconfiguratie te maken.
 4. Typ een **naam** voor configuratie van het bereik.
 5. Klik op **computergroepen selecteren**.
 6. Selecteer de groep van de computer die u hebt gemaakt en eventueel andere groepen toe te voegen aan de configuratie.  Klik op **Selecteren**.  
 6. Klik op **OK** de scopeconfiguratie maken. 


 ### <a name="3-apply-the-scope-configuration-to-a-solution"></a>3. Configuratie van het bereik van toepassing op een oplossing.
Zodra u een bereik-configuratie hebt, kunt klikt u vervolgens u deze toepassen op een of meer oplossingen.  Houd er rekening mee dat tijdens de configuratie van een enkel bereik kan worden gebruikt met meerdere oplossingen, elke oplossing alleen één scopeconfiguratie gebruiken kunt.

De configuratie van een bereik met behulp van het volgende proces toepassen.  

 1. In de Azure-portal, gaat u naar **Log Analytics** en selecteert u uw werkruimte.
 2. Selecteer in de eigenschappen voor de werkruimte **oplossingen**.
 3. Klik op de oplossing die u wilt dat aan het bereik.
 4. In de eigenschappen voor de oplossing onder **gegevensbronnen voor werkruimte** Selecteer **Oplossingstargeting**.  Als de optie niet beschikbaar is vervolgens [deze oplossing kan niet worden gericht](#solutions-and-agents-that-cant-be-targeted).
 5. Klik op **scopeconfiguratie toevoegen**.  Als u al een configuratie die is toegepast op deze oplossing hebt worden met deze optie niet beschikbaar.  De bestaande configuratie voordat u een andere toevoegt, moet u verwijderen.
 6. Klik op de configuratie van het bereik dat u hebt gemaakt.
 7. Bekijk de **Status** van de configuratie om ervoor te zorgen dat u ziet **geslaagd**.  Als de status geeft aan dat een fout, klikt u op de ellips aan de rechterkant van de configuratie en selecteer **scopeconfiguratie bewerken** wijzigingen aanbrengen.

## <a name="solutions-and-agents-that-cant-be-targeted"></a>Oplossingen en agents die niet kunnen worden toegepast
Hieronder vindt u de criteria voor agents en oplossingen die niet worden gebruikt met oplossingstargeting.

- Oplossingstargeting geldt alleen voor oplossingen die op agents implementeren.
- Oplossingstargeting geldt alleen voor oplossingen die worden geleverd door Microsoft.  Dit geldt niet voor oplossingen [die zijn gemaakt door uzelf of partners](solutions-creating.md).
- U kunt alleen filteren van agents die rechtstreeks verbinding met Log Analytics maken.  Oplossingen wordt automatisch geïmplementeerd op alle agents die deel van een verbonden beheergroep van Operations Manager uitmaken of ze zijn opgenomen in de configuratie van een scope of niet.

### <a name="exceptions"></a>Uitzonderingen
Oplossingstargeting kan niet worden gebruikt met de volgende oplossingen zelfs als ze voldoen aan de opgegeven criteria.

- Agent-Statusevaluatie

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over oplossingen voor beheer, inclusief de oplossingen die kunnen worden geïnstalleerd in uw omgeving op [oplossingen voor Azure Log Analytics toevoegen aan uw werkruimte](solutions.md).
- Meer informatie over het maken van computergroepen op [computergroepen in Log Analytics-zoekopdrachten](../../azure-monitor/platform/computer-groups.md).
