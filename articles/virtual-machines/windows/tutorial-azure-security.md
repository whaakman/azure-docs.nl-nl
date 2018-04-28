---
title: Azure Security Center en Windows virtuele machines in Azure | Microsoft Docs
description: Meer informatie over de beveiliging van uw Windows Azure virtuele machine met Azure Security Center.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 35b6c8910b0dd5e8d1c388ee83223ce77a08f6d1
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>De beveiliging van virtuele machines bewaken met behulp van Azure Security Center

Met Azure Security Center kunt u meer inzicht krijgen in de beveiliging van uw Azure-resources. Security Center biedt geïntegreerde beveiligingsbewaking. De service kan bedreigingen detecteren die anders misschien onopgemerkt blijven. In deze zelfstudie krijgt u meer informatie over Azure Security Center en leert u het volgende te doen:
 
> [!div class="checklist"]
> * Gegevensverzameling instellen
> * Beveiligingsbeleid kunt instellen
> * Problemen met de configuratiestatus kunt weergeven en oplossen
> * Gedetecteerde bedreigingen kunt controleren  

## <a name="security-center-overview"></a>Overzicht van Security Center

Security Center identificeert mogelijke configuratieproblemen voor virtuele machine (VM's) en gerichte beveiligingsrisico's. Dit kan het volgende omvatten: virtuele machines waarbij netwerkbeveiligingsgroepen ontbreken, niet-versleutelde schijven en RDP-beveiligingsaanvallen (Remote Desktop Protocol). De informatie wordt op het Security Center-dashboard in duidelijke grafieken weergegeven.

Als u het Security Center-dashboard wilt openen, selecteert u in het menu van Azure Portal de optie **Security Center**. Op het dashboard kunt u de beveiligingsstatus van uw Azure-omgeving bekijken, een aantal huidige aanbevelingen zien en de huidige status van bedreigingswaarschuwingen weergeven. U kunt elke algemene grafiek uitvouwen voor gedetailleerdere gegevens.

![Dashboard van Security Center](./media/tutorial-azure-security/asc-dash.png)

Security Center biedt niet alleen gegevensdetectie maar ook aanbevelingen voor gedetecteerde problemen. Als een VM bijvoorbeeld zonder een gekoppelde netwerkbeveiligingsgroep is geïmplementeerd, geeft Security Center een aanbeveling weer inclusief herstelstappen. U krijgt automatisch herstelstappen zonder de context van Security Center te verlaten.  

![Aanbevelingen](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Gegevensverzameling kunt instellen

Voordat u VM-beveiligingsconfiguraties kunt inzien, moet u eerst gegevensverzameling instellen in Security Center. Dit omvat het inschakelen van gegevensverzameling en het maken van een Azure-opslagaccount voor het opslaan van verzamelde gegevens. 

1. Klik in het Security Center-dashboard op **Beveiligingsbeleid** en selecteer vervolgens uw abonnement. 
2. Bij **Gegevensverzameling** selecteert u **Aan**.
3. Selecteer **Een opslagaccount kiezen** om een opslagaccount te maken. Selecteer vervolgens **OK**.
4. Selecteer op de blade **Beveiligingsbeleid** de optie **Opslaan**. 

Vervolgens wordt de Security Center-agent voor gegevensverzameling op alle VM's geïnstalleerd en wordt de gegevensverzameling gestart. 

## <a name="set-up-a-security-policy"></a>Een beveiligingsbeleid instellen

Een beveiligingsbeleid wordt gebruikt voor het definiëren van de items waarvoor Security Center gegevens verzamelt en aanbevelingen doet. U kunt verschillende soorten beveiligingsbeleid toepassen op verschillende sets van Azure-resources. Hoewel Azure-resources standaard voor alle beleidsitems worden geëvalueerd, kunt u afzonderlijke beleidsitems uitschakelen voor alle Azure-resources of voor een resourcegroep. Zie [Beveiligingsbeleid instellen in Azure Security Center](../../security-center/security-center-policies.md) voor gedetailleerde informatie over Security Center-beveiligingsbeleid. 

Een beveiligingsbeleid voor alle Azure-resources instellen:

1. Selecteer in het Security Center-dashboard **Beveiligingsbeleid** en selecteer vervolgens uw abonnement.
2. Selecteer **Preventiebeleid**.
3. Schakel beleidsitems die u wilt toepassen op alle Azure-resources in of uit.
4. Wanneer u klaar bent met instellen, selecteert u **OK**.
5. Selecteer op de blade **Beveiligingsbeleid** de optie **Opslaan**. 

Een beleid voor een specifieke resourcegroep instellen:

1. Selecteer in het Security Center-dashboard **Beveiligingsbeleid** en selecteer vervolgens een resourcegroep.
2. Selecteer **Preventiebeleid**.
3. Schakel beleidsitems die u wilt toepassen op de resourcegroep in of uit.
4. Selecteer onder **OVERNAME** de optie **Uniek**.
5. Wanneer u klaar bent met instellen, selecteert u **OK**.
6. Selecteer op de blade **Beveiligingsbeleid** de optie **Opslaan**.  

U kunt op deze pagina ook gegevensverzameling voor een specifieke resourcegroep uitschakelen.

In het volgende voorbeeld is een uniek beleid gemaakt voor een resourcegroep met de naam *myResoureGroup*. In dit beleid zijn aanbevelingen voor schijfversleuteling en Web Application Firewall uitgeschakeld.

![Uniek beleid](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Configuratiestatus van VM weergeven

Zodra u gegevensverzameling hebt ingeschakeld en een beveiligingsbeleid hebt ingesteld, begint Security Center meldingen en aanbevelingen te geven. Wanneer virtuele machines worden geïmplementeerd, wordt de agent voor gegevensverzameling geïnstalleerd. Security Center wordt vervolgens gevuld met gegevens voor de nieuwe VM's. Zie [Protect your VMs in Security Center](../../security-center/security-center-virtual-machine-recommendations.md) (Uw VM's beschermen in Security Center) voor gedetailleerde informatie over de configuratiestatus van VM's. 

Terwijl er gegevens worden verzameld, worden de resourcestatus voor elke VM en de gerelateerde Azure-resource verzameld. De informatie wordt weergegeven in een duidelijk diagram. 

Resourcestatus bekijken:

1.  Selecteer op het Security Center-dashboard onder **Beveiligingsstatus van de resource** de optie **Berekenen**. 
2.  Selecteer op de blade **Berekenen** de optie **Virtuele machines**. Deze weergave biedt een samenvatting van de configuratiestatus van alle VM's.

![Status berekenen](./media/tutorial-azure-security/compute-health.png)

Selecteer een VM als u alle aanbevelingen voor deze VM wilt bekijken. Aanbevelingen en herstel worden in meer detail behandeld in het volgende gedeelte van deze zelfstudie.

## <a name="remediate-configuration-issues"></a>Configuratieproblemen oplossen

Wanneer Security Center begint met het invullen van configuratiegegevens, worden er aanbevelingen gemaakt op basis van het ingestelde beveiligingsbeleid. Als een VM bijvoorbeeld zonder een gekoppelde netwerkbeveiligingsgroep is ingesteld, wordt aanbevolen er een te maken. 

Een lijst met alle aanbevelingen weergeven: 

1. Selecteer in het Security Center-dashboard de optie **Aanbevelingen**.
2. Selecteer een specifieke aanbeveling. Er wordt een lijst weergegeven met alle resources waarop de aanbeveling van toepassing is.
3. Als u een aanbeveling wilt toepassen, selecteert u een specifieke resource. 
4. Volg de instructies voor herstelstappen. 

In veel gevallen biedt Security Center stappen om een aanbeveling uit te voeren zonder Security Center te verlaten. In het volgende voorbeeld detecteert Security Center een netwerkbeveiligingsgroep met een onbeperkte binnenkomende regel. Op de aanbevelingenpagina kunt u de knop **Binnenkomende regels bewerken** selecteren. De gebruikersinterface die nodig is voor het wijzigen van de regel wordt weergegeven. 

![Aanbevelingen](./media/tutorial-azure-security/remediation.png)

Wanneer aanbevelingen worden doorgevoerd, worden ze als opgelost gemarkeerd. 

## <a name="view-detected-threats"></a>Gedetecteerde bedreigingen bekijken

Security Center geeft naast aanbevelingen voor resourceconfiguratie ook meldingen voor geconstateerde bedreigingen weer. De functie voor beveiligingsmeldingen combineert de verzamelde gegevens van elke VM, Azure-netwerklogboeken en gekoppelde partneroplossingen om bedreigingen voor Azure-resources te detecteren. Lees [Detectiemogelijkheden van Azure Security Center](../../security-center/security-center-detection-capabilities.md) voor gedetailleerdere informatie over de detectiemogelijkheden van Azure Security Center.

De functie voor beveiligingsmeldingen vereist dat de Security Center-prijscategorie wordt verhoogd van *Gratis* naar *Standard*. Er is een **gratis proefversie** van 30 dagen beschikbaar wanneer u naar deze hogere prijscategorie overstapt. 

De prijscategorie wijzigen:  

1. Klik in het Security Center-dashboard op **Beveiligingsbeleid** en selecteer vervolgens uw abonnement.
2. Selecteer **Prijscategorie**.
3. Selecteer de nieuwe categorie en kies **Selecteren**.
4. Selecteer op de blade **Beveiligingsbeleid** de optie **Opslaan**. 

Nadat u de prijscategorie hebt gewijzigd, wordt de grafiek voor beveiligingsmeldingen ingevuld wanneer er beveiligingsbedreigingen worden gedetecteerd.

![Beveiligingswaarschuwingen](./media/tutorial-azure-security/security-alerts.png)

Selecteer een waarschuwing om informatie weer te geven. U kunt bijvoorbeeld een beschrijving van de bedreiging, het tijdstip van de detectie, alle bedreigingspogingen en de aanbevolen herstelstappen bekijken. In het volgende voorbeeld wordt een RDP-beveiligingsaanval gedetecteerd, met 294 mislukte RDP-pogingen. Er wordt een aanbevolen oplossing gegeven.

![RDP-aanval](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u Azure Security Center ingesteld en vervolgens VM's in Security Center gecontroleerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Gegevensverzameling kunt instellen
> * Beveiligingsbeleid kunt instellen
> * Problemen met de configuratiestatus kunt weergeven en oplossen
> * Gedetecteerde bedreigingen kunt controleren

Ga naar de volgende zelfstudie voor informatie over het maken van een CI/CD-pijplijn met Visual Studio Team Services en een Windows-VM waarop IIS wordt uitgevoerd.

> [!div class="nextstepaction"]
> [Visual Studio Team Services CI/CD-pipeline](./tutorial-vsts-iis-cicd.md)
