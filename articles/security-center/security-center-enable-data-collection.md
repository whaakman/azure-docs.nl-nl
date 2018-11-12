---
title: Verzamelen van gegevens in Azure Security Center | Microsoft Docs
description: " Informatie over het verzamelen van gegevens in Azure Security Center inschakelen. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/2/2018
ms.author: rkarlin
ms.openlocfilehash: 9100722042bfdcbee48af1fc44e294df046c828e
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007209"
---
# <a name="data-collection-in-azure-security-center"></a>Verzamelen van gegevens in Azure Security Center
Security Center verzamelt gegevens van uw virtuele Azure-machines (VM's) en niet-Azure-computers om te controleren op beveiligingsproblemen en bedreigingen. De gegevens worden verzameld met behulp van de MMA, die verschillende configuraties en gebeurtenislogboeken met betrekking tot beveiliging van de machine leest en de gegevens kopieert naar uw werkruimte voor analyse. Voorbeelden van dergelijke gegevens zijn: besturingssysteem systeemtype en versie, besturingssysteemlogboeken (Windows-gebeurtenislogboeken), actieve processen, computernaam, IP-adressen en aangemelde gebruiker. De Microsoft Monitoring Agent kopieert ook crashdumpbestanden naar uw werkruimte.

Het verzamelen van gegevens is vereist om aan te bieden inzicht in de ontbrekende updates, onjuist geconfigureerde OS-beveiligingsinstellingen, endpoint protection inschakelen en status- en bedreigingsbeveiliging detecties. 

Dit artikel bevat richtlijnen over hoe u Microsoft Monitoring Agent installeren en instellen van een werkruimte voor logboekanalyse voor het opslaan van de verzamelde gegevens. Beide bewerkingen zijn vereist om gegevens te verzamelen. 

> [!NOTE]
> - Het verzamelen van gegevens is alleen nodig voor de Compute-resources (virtuele machines en niet-Azure-computers). U kunt profiteren van Azure Security Center, zelfs als u agents; niet inrichten echter, u hebt beperkte beveiliging en bovenstaande functies worden niet ondersteund.  
> - Zie voor een lijst van ondersteunde platforms, [ondersteunde platforms in Azure Security Center](security-center-os-coverage.md).
> - Verzamelen van gegevens voor virtuele-machineschaalset is momenteel niet ondersteund.


## Automatische inrichting van Microsoft Monitoring Agent inschakelen <a name="auto-provision-mma"></a>

Het verzamelen van de gegevens van de machines die u moet beschikken over de Microsoft Monitoring Agent is geïnstalleerd.  Installatie van de agent automatisch kan worden (aanbevolen) of u kunt de agent handmatig installeren.  

>[!NOTE]
> Automatische inrichting is standaard uitgeschakeld. Als u wilt instellen in Security Center voor het installeren van automatische inrichting standaard, ingesteld op **op**.
>

Als automatisch inrichten ingeschakeld is, ondersteund levert Security Center de Microsoft Monitoring Agent op alle Azure-VM's en een nieuwe VM's die worden gemaakt. Automatische inrichting wordt sterk aanbevolen, maar handmatige agentinstallatie is ook beschikbaar. [Informatie over het installeren van de Microsoft Monitoring Agent-extensie](#manualagent).



Automatische inrichting van de MMA inschakelen:
1. Selecteer in het hoofdmenu van Security Center de optie **beveiligingsbeleid**.
2. Selecteer het abonnement.

  ![Abonnement selecteren][7]

3. Onder **Beveiligingsbeleid** selecteert u **Gegevensverzameling**.
4. Onder **automatische inrichting**, selecteer **op** voor automatische inrichting inschakelen.
5. Selecteer **Opslaan**.

  ![Automatische inrichting inschakelen][1]

>[!NOTE]
> - Zie voor instructies over het inrichten van een bestaande installatie [automatische inrichting in geval van een bestaande installatie van de agent](#preexisting).
> - Zie voor instructies over het inrichten van handmatige [handmatig installeren van de Microsoft Monitoring Agent-extensie](#manualagent).
> - Zie voor instructies voor automatische inrichting uitschakelen, [automatische inrichting uitschakelen](#offprovisioning).
> - Voor instructies over hoe onboarding Security Center met behulp van PowerShell, Zie [automatiseren onboarding van Azure Security Center met behulp van PowerShell](security-center-powershell-onboarding.md).
>

## <a name="workspace-configuration"></a>Configuratie van de werkruimte
Gegevens die zijn verzameld door Security Center wordt opgeslagen in Log Analytics-werkruimte.  U kunt gegevens die worden verzameld van Azure VM's die zijn opgeslagen in werkruimten die zijn gemaakt door Security Center of in een bestaande werkruimte die u hebt gemaakt. 

Configuratie van de standaardwerkruimte per abonnement is ingesteld en veel abonnementen kunnen dezelfde werkruimte gebruiken.

### <a name="using-a-workspace-created-by-security-center"></a>Een werkruimte gemaakt met Security Center gebruiken

Security center kan automatisch een standaardwerkruimte voor het opslaan van de gegevens maken. 

Een werkruimte te selecteren die zijn gemaakt door Security Center:

1.  Onder **configuratie van de standaardwerkruimte**, selecteer werkruimten die zijn gemaakt door Security center gebruiken.
   ![Prijscategorie selecteren][10] 

2. Klik op **Opslaan**.<br>
    Security Center maakt u een nieuwe resource-groep en de standaard werkruimte in die geolocatie en verbindt de agent voor de desbetreffende werkruimte. De naamconventie voor de werkruimte en de resource is:<br>
**Werkruimte: Standaardwerkruimte-[abonnement-ID]-[geo]<br> resourcegroep: Standaardresoucegroep-[geo]**

   Als een abonnement virtuele machines van meerdere geolocations bevat, maakt Security Center meerdere werkruimten. Meerdere werkruimten worden gemaakt voor het onderhouden van regels voor privacy van gegevens.
-   Security Center wordt automatisch ingeschakeld voor een oplossing voor Security Center in de werkruimte per de prijscategorie voor het abonnement instellen. 

> [!NOTE]
> Log Analytics-prijscategorie van werkruimten die zijn gemaakt door Security Center heeft geen invloed op de facturering van Security Center. Security Center-facturering is altijd gebaseerd op het Security Center-beveiligingsbeleid en de oplossingen die zijn geïnstalleerd op een werkruimte. Voor de laag gratis, schakelt Azure Security Center de *SecurityCenterFree* -oplossing op de standaardwerkruimte. Voor de Standard-laag, schakelt Azure Security Center de *Security* -oplossing op de standaardwerkruimte.

Zie voor meer informatie over prijzen [prijzen van Security Center](https://azure.microsoft.com/pricing/details/security-center/).

Zie voor meer informatie over bestaande Log Analytics-accounts, [bestaande Log Analytics-klanten](security-center-faq.md#existingloganalyticscust).

### <a name="using-an-existing-workspace"></a>Met behulp van een bestaande werkruimte

Als u al een bestaande Log Analytics-werkruimte is het raadzaam om te gebruiken dezelfde werkruimte.

Voor het gebruik van uw bestaande Log Analytics-werkruimte, hebt u lees- en schrijfmachtigingen heeft voor de werkruimte.

> [!NOTE]
> Oplossingen die zijn ingeschakeld op de bestaande werkruimte wordt toegepast op Azure-VM's die zijn verbonden met het. Voor betaalde oplossingen kan dit resulteren in extra kosten in rekening gebracht. Zorg ervoor dat de geselecteerde werkruimte zich in de juiste geografische regio voor overwegingen bij de privacy van gegevens.
>

Selecteer een bestaande Log Analytics-werkruimte:

1. Onder **configuratie van de standaardwerkruimte**, selecteer **gebruik een andere werkruimte**.

   ![Selecteer een bestaande werkruimte][2]

2. Selecteer een werkruimte voor het opslaan van verzamelde gegevens in de vervolgkeuzelijst.

  > [!NOTE]
  > In de vervolgkeuzelijst menu zijn alle werkruimten in al uw abonnementen beschikbaar. Zie [cross-werkruimte kunt selecteren. abonnement](security-center-enable-data-collection.md#cross-subscription-workspace-selection) voor meer informatie. U moet gemachtigd voor toegang tot de werkruimte.
  >
  >

3. Selecteer **Opslaan**.
4. Na het selecteren van **opslaan**, wordt u gevraagd als u wilt reconfigure bewaakte VM's die eerder zijn verbonden met een standaardwerkruimte.

   - Selecteer **Nee** als u wilt dat de nieuwe werkruimte-instellingen wilt toepassen op nieuwe virtuele machines. De nieuwe werkruimte-instellingen zijn alleen van toepassing op nieuwe agentinstallaties; nieuwe gedetecteerde virtuele machines die geen van de Microsoft Monitoring Agent is geïnstalleerd.
   - Selecteer **Ja** als u wilt dat de nieuwe werkruimte-instellingen wilt toepassen op alle virtuele machines. Bovendien wordt elke virtuele machine verbonden met een Security Center-werkruimte gemaakt verbonden aan de nieuwe doelwerkruimte.

   > [!NOTE]
   > Als u Ja selecteert, moet u de werkruimten die zijn gemaakt door Security Center totdat alle virtuele machines hebt opnieuw is verbonden met de nieuwe doelwerkruimte niet verwijderen. Met deze bewerking mislukt als een werkruimte te vroeg is verwijderd.
   >
   >

   - Selecteer **annuleren** om de bewerking te annuleren.

     ![Selecteer een bestaande werkruimte][3]

5. Selecteer de prijscategorie voor de gewenste werkruimte die u van plan bent om in te stellen van de Microsoft Monitoring agent. <br>Voor het gebruik van een bestaande werkruimte, stelt u de prijscategorie voor de werkruimte. Hiermee installeert u een security Center-oplossing in de werkruimte als deze nog niet is aanwezig.

    a.  Selecteer in het hoofdmenu van Security Center **beveiligingsbeleid**.
     
    b.  Selecteer de gewenste werkruimte waarin u van plan bent om de agent verbinding te maken.
        ![Selecteer werkruimte][8] c. De prijscategorie instellen.
        ![Prijscategorie selecteren][9] 
   
   >[!NOTE]
   >Als de werkruimte is al een **Security** of **SecurityCenterFree** oplossing is ingeschakeld, de prijzen wordt automatisch ingesteld. 

## <a name="cross-subscription-workspace-selection"></a>Abonnementoverschrijdende werkruimte kunt selecteren.
Wanneer u een werkruimte waarin u uw gegevens kunt opslaan selecteert, worden alle werkruimten in al uw abonnementen beschikbaar. Abonnementoverschrijdende werkruimte kunt selecteren. Hiermee kunt u gegevens van virtuele machines die worden uitgevoerd in verschillende abonnementen verzamelen en op te slaan in de werkruimte van uw keuze. Deze optie is handig als u van een gecentraliseerde werkruimte in uw organisatie gebruikmaakt en wilt gebruiken voor verzameling van beveiligingsgegevens. Zie voor meer informatie over het beheren van werkruimten [werkruimtetoegang beheren](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access).


## <a name="data-collection-tier"></a>Verzameling gegevenslaag
Een verzameling gegevenslaag selecteren in Azure Security Center heeft alleen invloed op de opslag van beveiligingsgebeurtenissen in uw Log Analytics-werkruimte. De Microsoft Monitoring Agent wordt nog steeds verzamelen en analyseren van de beveiligingsgebeurtenissen die vereist zijn voor Azure Security Center bedreigingen detecties, ongeacht welke laag van beveiligingsgebeurtenissen die u kiest voor het opslaan in uw Log Analytics-werkruimte (indien aanwezig). Voor het opslaan van beveiligingsgebeurtenissen in uw werkruimte te kiezen kunt voor onderzoek, zoeken en controle van de gebeurtenissen die in uw werkruimte. 
> [!NOTE]
> Opslaan van gegevens in Log Analytics mogelijk extra kosten voor gegevensopslag, Zie de pagina met prijzen voor meer informatie.
>
U kunt het recht filterbeleid voor uw abonnementen en werkruimten op vier sets gebeurtenissen worden opgeslagen in uw werkruimte kiezen: 

- **Geen** – gebeurtenisopslag beveiliging uitschakelen. Dit is de standaardinstelling.
- **Minimale** – een kleiner aantal gebeurtenissen voor klanten die willen minimaliseren van het volume van de gebeurtenis.
- **Algemene** : dit is een set van gebeurtenissen die voldoet aan de meeste klanten en kan ze een volledige audittrail.
- **Alle gebeurtenissen** – voor klanten die willen Zorg ervoor dat alle gebeurtenissen worden opgeslagen.


> [!NOTE]
> Deze gebeurtenissen security sets zijn alleen beschikbaar op Standard-laag van Security Center bevinden. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
Deze sets zijn ontworpen voor typische scenario's. Zorg ervoor dat u evalueren welke aan uw behoeften voldoet voordat u deze implementeert.
>
>

Om te bepalen van de gebeurtenissen die tot behoren de **algemene** en **minimale** evenementen, wij werken samen met klanten en industrienormen voor meer informatie over de ongefilterde frequentie van elke gebeurtenis en hun gebruik. We de volgende richtlijnen in dit proces gebruikt:

- **Minimale** -Zorg ervoor dat deze set omvat alleen de gebeurtenissen die kunnen wijzen op een geslaagde inbreuk en belangrijke gebeurtenissen die een zeer lage volume hebt. Bijvoorbeeld, deze verzameling bevat geslaagde en mislukte gebruikersaanmelding (gebeurtenis-id's 4624, 4625), maar het afmelden is belangrijk voor het controleren van, maar niet zinvol is voor de detectie en relatief hoog volume heeft geen bevat. De meeste van het gegevensvolume van deze set is de aanmeldgebeurtenissen en het maken van procesgebeurtenis (gebeurtenis-ID 4688).
- **Algemene** -bieden een volledige gebruikersaudittrail in deze set. Deze verzameling bevat bijvoorbeeld zowel gebruikersaanmelding en -afmelding (gebeurtenis-ID 4634). We nemen controle van acties zoals wijzigingen, belangrijke domain controller Kerberos operations en andere gebeurtenissen die door de bedrijfstak van organisaties worden aanbevolen.

Gebeurtenissen die zeer laag volume hebben zijn opgenomen in de gemeenschappelijke set als de belangrijkste reden om te kiezen dat het over alle gebeurtenissen is dat er minder i en niet voor het filteren van specifieke gebeurtenissen.

Hier volgt een compleet overzicht van de beveiliging en AppLocker gebeurtenis-id's voor elke set:

| Gegevenslaag | Verzamelde gebeurtenis indicatoren |
| --- | --- |
| Minimaal | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Algemeen | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,461,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> - Als u gebruikmaakt van groepsbeleidsobject (GPO), is het aanbevolen dat u controlebeleid proces maken gebeurtenis 4688 inschakelt en de *CommandLine* veld binnen gebeurtenis 4688. Zie voor meer informatie over het proces maken gebeurtenis 4688 van Security Center [Veelgestelde vragen over](security-center-faq.md#what-happens-when-data-collection-is-enabled). Voor meer informatie over deze controlebeleid, Zie [Audit beleid aanbevelingen](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
> -  Om gegevens te verzamelen voor [besturingselementen voor adaptieve toepassingen](security-center-adaptive-application.md), Security Center configureert u een lokale AppLocker-beleid in de controlemodus om toe te staan alle toepassingen. Dit zorgt ervoor dat AppLocker voor het genereren van gebeurtenissen die worden verzameld en gebruikt door Security Center. Het is belangrijk te weten dat dit beleid niet worden geconfigureerd op alle computers waarop er al een geconfigureerde AppLocker-beleid is. 
> - Voor het verzamelen van Windows-Filterplatform [gebeurtenis-ID 5156](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=5156), moet u inschakelen [Audit Filtering Platform verbinding](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection) (Auditpol / / subcategory: 'Filtering Platform verbinding' /Success:Enable)
>

Uw filterbeleid kiezen:
1. Op de **beveiligingsbeleid voor het verzamelen van gegevens** blade, selecteert u de filters gebruiken om beleid onder **beveiligingsgebeurtenissen**.
2. Selecteer **Opslaan**.

   ![Kies beleid filteren][5]

### Automatische inrichting in geval van een bestaande installatie van agent <a name="preexisting"></a> 

De volgende gevallen gebruik opgeven hoe automatische inrichting in gevallen wanneer er al een agent of -extensie is geïnstalleerd. 

- Microsoft Monitoring Agent is geïnstalleerd op de computer, maar niet als een uitbreiding<br>
Als de Microsoft Monitoring Agent rechtstreeks op de virtuele machine (en niet als een Azure-extensie) is geïnstalleerd, wordt in Security Center de Microsoft Monitoring Agent niet installeren. U kunt automatische inrichting inschakelen en selecteert u de betreffende gebruikerswerkruimte in Security Center de automatische inrichting van de configuratie. Als u ervoor de dezelfde werkruimte van die de virtuele machine al met de bestaande agent verbonden is zal worden verpakt met de extensie van de Microsoft Monitoring Agent kiest. 

> [!NOTE]
> Als u SCOM 2012-versie van de agent is geïnstalleerd, **niet** automatisch inrichten op. 

Zie voor meer informatie, [wat gebeurt er als een SCOM of OMS directe agent al is geïnstalleerd op mijn VM?](security-center-faq.md#scomomsinstalled)

-   Een bestaande VM-extensie is aanwezig<br>
    - Security center biedt ondersteuning voor bestaande installaties van de extensie en wordt de bestaande verbindingen niet overschreven. Security Center worden gegevens van de virtuele machine in de werkruimte al verbonden en beveiliging op basis van de oplossingen die zijn ingeschakeld op de werkruimte biedt opgeslagen.   
    - Om te zien op welke werkruimte de bestaande extensie verzendt gegevens naar de test uit te voeren [connectiviteit met Azure Security Center valideren](https://blogs.technet.microsoft.com/yuridiogenes/2017/10/13/validating-connectivity-with-azure-security-center/). U kunt ook Log analytics openen, selecteer een werkruimte, selecteert u de virtuele machine en kijken naar de Microsoft Monitoring Agent-verbinding. 
    - Als u een omgeving waar de Microsoft Monitoring Agent is geïnstalleerd op werkstations en rapporteren aan een bestaande Log Analytics-werkruimte, bekijk de lijst [besturingssystemen die worden ondersteund door Azure Security Center](security-center-os-coverage.md) naar Zorg ervoor dat het besturingssysteem wordt ondersteund, en Zie [bestaande Log Analytics-klanten](security-center-faq.md#existingloganalyticscust) voor meer informatie.
 
### Automatische inrichting uitschakelen <a name="offprovisioning"></a>
U kunt automatische inrichting van resources op elk gewenst moment door het uitschakelen van deze instelling in het beveiligingsbeleid uitschakelen. 


1. Ga terug naar het hoofdmenu van Security Center en selecteert u het beveiligingsbeleid.
2. Selecteer het abonnement waarvoor u automatisch inrichten wilt uitschakelen.
3. Op de **beveiligingsbeleid – gegevensverzameling** blade onder **automatische inrichting** Selecteer **uit**.
4. Selecteer **Opslaan**.

  ![Automatische inrichting uitschakelen][6]

Wanneer automatische inrichting is uitgeschakeld (uitgeschakeld), wordt de configuratiesectie van de standaard-werkruimte niet weergegeven.

Als u automatisch inrichten na uitschakelen voorheen op:
-   Agents worden niet ingericht op nieuwe virtuele machines.
-   Security Center stopt het verzamelen van gegevens van de standaardwerkruimte.
 
> [!NOTE]
>  Uitschakelen van automatische inrichting, wordt de Microsoft Monitoring Agent niet verwijderd van Azure VM's waarop de agent is ingericht. Zie voor meer informatie over het verwijderen van de OMS-extensie [hoe verwijder ik OMS-extensies geïnstalleerd door Security Center](security-center-faq.md#remove-oms).
>
    
## Handmatige configuratie van agent <a name="manualagent"></a>
 
Er zijn verschillende manieren de Microsoft Monitoring Agent handmatig installeren. Als u handmatig installeert, zorg er dan voor dat u de automatische inrichting uitschakelt.

### <a name="operations-management-suite-vm-extension-deployment"></a>Implementatie van operations Management Suite VM-extensie 

U kunt Microsoft Monitoring Agent, handmatig installeren, zodat Security Center kunt verzamelen van beveiligingsgegevens van uw virtuele machines en aanbevelingen en waarschuwingen bieden.
1.  Selecteer automatisch inrichten – uit.
2.  Een werkruimte maken en de prijscategorie voor de werkruimte die u van plan bent om in te stellen van de Microsoft Monitoring agent ingesteld:

    a.  Selecteer in het hoofdmenu van Security Center **beveiligingsbeleid**.
     
    b.  Selecteer de werkruimte waarin u van plan bent om de agent verbinding te maken. Zorg ervoor dat de werkruimte zich in hetzelfde abonnement u in Security Center en dat u lees-/ schrijfmachtigingen beschikt in de werkruimte hebben.
        ![Selecteer een werkruimte][8]
3. De prijscategorie instellen.
   ![Prijscategorie selecteren][9] 
   >[!NOTE]
   >Als de werkruimte is al een **Security** of **SecurityCenterFree** oplossing is ingeschakeld, de prijzen wordt automatisch ingesteld. 
   > 

4.  Als u de agents op de nieuwe virtuele machines met behulp van Resource Manager-sjabloon te implementeren wilt, installeert u de extensie van de OMS-virtuele machine:

    a.  [De extensie van de OMS-virtuele machine voor Windows installeren](../virtual-machines/extensions/oms-windows.md)
    
    b.  [De extensie van de OMS-virtuele machine voor Linux installeren](../virtual-machines/extensions/oms-linux.md)
5.  Volg de instructies in voor het implementeren van de extensies op bestaande virtuele machines, [gegevens verzamelen over Azure Virtual Machines](../log-analytics/log-analytics-quick-collect-azurevm.md).

  > [!NOTE]
  > De sectie **gebeurtenissen en prestatiegegevens verzamelen** is optioneel.
  >
6. Als u PowerShell wilt implementeren de extensie, gebruik de volgende PowerShell-voorbeeld:
    1.  Ga naar **Log Analytics** en klikt u op **geavanceerde instellingen**.
    
        ![Set-logboekanalyse][11]

    2. Kopieer de waarden van **WorkspaceID** en **primaire sleutel**.
  
       ![Waarden kopiëren][12]

    3. De openbare configuratie en de persoonlijke configuratie met deze waarden vullen:
     
            $PublicConf = '{
                "workspaceId": "WorkspaceID value",
                "MultipleConnectistopOnons": true
            }' 
 
            $PrivateConf = '{
                "workspaceKey": "<Primary key value>”
            }' 

      - Wanneer u installeert op een Windows-VM:
        
             Set-AzureRmVMExtension -ResourceGroupName $vm.ResourceGroupName -VMName $vm.Name -Name "MicrosoftMonitoringAgent" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "MicrosoftMonitoringAgent" -TypeHandlerVersion '1.0' -Location $vm.Location -Settingstring $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True 
    
       - Wanneer u installeert op een Linux-VM:
        
             Set-AzureRmVMExtension -ResourceGroupName $vm1.ResourceGroupName -VMName $vm1.Name -Name "OmsAgentForLinux" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "OmsAgentForLinux" -TypeHandlerVersion '1.0' -Location $vm.Location -Settingstring $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True`

> [!NOTE]
> Voor instructies over hoe onboarding Security Center met behulp van PowerShell, Zie [automatiseren onboarding van Azure Security Center met behulp van PowerShell](security-center-powershell-onboarding.md).

## <a name="troubleshooting"></a>Problemen oplossen

-   Zie voor het identificeren van problemen met de installatie automatisch inrichten, [problemen met de status van de agentstatus](security-center-troubleshooting-guide.md#mon-agent).

-  Zie voor het identificeren van de netwerkvereisten voor monitoring agent [probleemoplossing monitoring agent netwerkvereisten](security-center-troubleshooting-guide.md#mon-network-req).
-   Zie voor het identificeren van handmatige onboarding-problemen, [Operations Management Suite-onboarding oplossen](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).

- Zie voor het identificeren van niet-bewaakte VM's en computers problemen, [niet-bewaakte VM's en computers](security-center-virtual-machine-protection.md#unmonitored-vms-and-computers).

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe gegevens verzamelen en automatische inrichting in Security Center werkt. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.



<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
[8]: ./media/security-center-enable-data-collection/manual-provision.png
[9]: ./media/security-center-enable-data-collection/pricing-tier.png
[10]: ./media/security-center-enable-data-collection/workspace-selection.png
[11]: ./media/security-center-enable-data-collection/log-analytics.png
[12]: ./media/security-center-enable-data-collection/log-analytics2.png
