

## <a name="memory-preserving-updates"></a>Updates met geheugenbehoud
Voor een bepaalde klasse van updates in Microsoft Azure zien klanten geen impact op hun actuele virtuele machines. Dit zijn veelal updates van onderdelen of services die kunnen worden bijgewerkt zonder de actieve sessie te verstoren. Sommige van deze updates zijn updates van de platforminfrastructuur op de host waarop het besturingssysteem wordt uitgevoerd. Deze kunnen worden toegepast zonder dat de virtuele machines geheel opnieuw moeten worden opgestart.

Deze updates worden uitgevoerd met technologie die in-place live migratie mogelijk maakt, oftewel een update met geheugenbehoud. Tijdens het bijwerken wordt de virtuele machine in de status Onderbroken geplaatst, waarbij het geheugen in het RAM behouden blijft. Ondertussen ontvangt het besturingssysteem van de onderliggende host de noodzakelijke updates en patches. De virtuele machine wordt 30 seconden na het onderbreken hervat. Wanneer de virtuele machine is hervat, wordt de klok van de virtuele machine automatisch gesynchroniseerd.

Niet alle updates kunnen met deze methode worden geïmplementeerd, maar als dit wel mogelijk is, zorgt de korte onderbreking ervoor dat de impact op de virtuele machines aanzienlijk wordt gereduceerd.

Updates voor meerdere exemplaren (voor virtuele machines in een beschikbaarheidsset) worden telkens voor één updatedomein tegelijk toegepast.  

## <a name="virtual-machine-configurations"></a>VM-configuraties
Er zijn twee soorten VM-configuraties: voor meerdere exemplaren en voor één exemplaar. In een configuratie met meerdere exemplaren worden vergelijkbare virtuele machines in een beschikbaarheidsset geplaatst.

De configuratie met meerdere exemplaren biedt redundantie voor fysieke computers, kracht en netwerkmogelijkheden. Deze wordt aanbevolen om de beschikbaarheid van uw toepassing te garanderen. Alle virtuele machines in de beschikbaarheidsset moeten voor hetzelfde doel in uw toepassing worden gebruikt.

Voor meer informatie over het configureren van uw virtuele machines voor maximale beschikbaarheid raadpleegt u [Manage the availability of your Windows virtual machines](../articles/virtual-machines/virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (De beschikbaarheid van uw virtuele machines in Windows beheren) of [Manage the availability of your Linux virtual machines](../articles/virtual-machines/virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (De beschikbaarheid van uw virtuele machines in Linux beheren).

Een configuratie met één exemplaar wordt daarentegen gebruikt voor zelfstandige virtuele machines die niet in een beschikbaarheidsset zijn geplaatst. Deze virtuele machines komen niet in aanmerking voor de Service Level Agreement (SLA), waarvoor twee of meer virtuele machines in dezelfde beschikbaarheidsset moeten worden geïmplementeerd.

Zie voor meer informatie over de SLA's het gedeelte 'Cloud Services en virtuele machines' in [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="multi-instance-configuration-updates"></a>Een configuratie met meerdere exemplaren bijwerken
Tijdens gepland onderhoud werkt het Azure-platform eerst de set virtuele machines bij die worden gehost in een configuratie met meerdere exemplaren. De update zorgt ervoor dat deze virtuele machines na ongeveer 15 minuten uitvaltijd opnieuw worden opgestart.

Bij een update van een configuratie met meerdere exemplaren wordt ervan uitgegaan dat elke virtuele machine een vergelijkbare functie heeft als de andere virtuele machines in de beschikbaarheidsset. In deze situatie worden de virtuele machines op zodanige wijze bijgewerkt dat de beschikbaarheid tijdens het proces behouden blijft.

Elke virtuele machine in een beschikbaarheidsset krijgt door het onderliggende Azure-platform een updatedomein en een foutdomein toegewezen. Elk updatedomein is een groep virtuele machines die in hetzelfde tijdvenster opnieuw worden opgestart. Elk foutdomein is een groep virtuele machines met een gemeenschappelijke voedingsbron en netwerkswitch.


Zie [Configure multiple virtual machines in an availability set for redundancy](../articles/virtual-machines/virtual-machines-windows-manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) (Meerdere virtuele machines in een beschikbaarheidsset configureren voor redundantie) voor meer informatie over update- en foutdomeinen.

Om tijdens een update de beschikbaarheid te behouden, voert Azure het onderhoud per updatedomein uit, waarbij telkens één domein tegelijk wordt bijgewerkt. Tijdens het onderhoud in een updatedomein wordt elke virtuele machine in het domein uitgeschakeld. Vervolgens wordt de update op de hostmachines toegepast en worden de virtuele machines weer opgestart. Wanneer het onderhoud in het domein is voltooid, herhaalt Azure het proces voor het volgende updatedomein totdat elk domein is bijgewerkt.

De volgorde waarin de updatedomeinen opnieuw worden opgestart, verloopt tijdens gepland onderhoud niet altijd sequentieel, maar er wordt slechts één updatedomein tegelijk opnieuw opgestart. Gepland onderhoud aan virtuele machines voor configuraties met meerdere exemplaren wordt momenteel één week van tevoren door Azure aangekondigd.

Hier volgt een voorbeeld van wat er mogelijk in uw Windows-logboeken wordt weergegeven nadat een virtuele machine is hersteld:

<!--Image reference-->
![][image2]


Gebruik de viewer als u met Azure Portal, Azure PowerShell of Azure CLI een overzicht wilt genereren van de virtuele machines die in een configuratie met meerdere exemplaren zijn geconfigureerd. Zo kunt u met Azure Portal de _beschikbaarheidsset_ toevoegen aan het dialoogvenster Bladeren van **Virtuele machines (klassiek)**. De virtuele machines die dezelfde beschikbaarheidsset rapporteren, maken deel uit van een configuratie met meerdere exemplaren. In het volgende voorbeeld bestaat de configuratie met meerdere exemplaren uit de virtuele machines uit SQLContoso01 en SQLContoso02.

<!--Image reference-->
  ![Virtuele machines (klassieke weergave) in Azure Portal][image4]

## <a name="single-instance-configuration-updates"></a>Een configuratie met één exemplaar bijwerken
Wanneer het bijwerken van configuraties met meerdere exemplaren is voltooid, gaat Azure verder met het bijwerken van configuraties met één exemplaar. Bij deze updates worden ook virtuele machines opnieuw opgestart die niet in beschikbaarheidssets worden uitgevoerd.

> [!NOTE]
> Als er in een beschikbaarheidsset maar één exemplaar van een virtuele machine wordt uitgevoerd, wordt dit door het Azure-platform toch beschouwd als het bijwerken van een configuratie met meerdere exemplaren.
>

Tijdens het onderhoud in een configuratie met één exemplaar wordt elke virtuele machine die op een hostmachine wordt uitgevoerd, uitgeschakeld. Vervolgens wordt de hostcomputer bijgewerkt en worden de virtuele machines weer gestart. Het onderhoud gaat gepaard met circa 15 minuten uitvaltijd. De geplande onderhoudsgebeurtenis wordt in één onderhoudsvenster uitgevoerd voor alle virtuele machines in een regio.


Ten aanzien van configuraties met één exemplaar hebben geplande onderhoudsgebeurtenissen invloed op de beschikbaarheid van uw toepassing. Gepland onderhoud aan virtuele machines voor configuraties met één exemplaar wordt één week van tevoren door Azure aangekondigd.

## <a name="email-notification"></a>E-mailmelding
Alleen voor VM-configuraties met één exemplaar en voor VM-configuraties met meerdere exemplaren verzendt Azure een e-mailmelding over het komende geplande onderhoud (één week van tevoren). Dit e-mailbericht wordt verzonden naar de e-mailaccounts van de beheerder en medebeheerder van het abonnement. Dit is een voorbeeld van een dergelijk e-mailbericht:

<!--Image reference-->
![][image1]

## <a name="region-pairs"></a>Regioparen

Tijdens onderhoud werkt Azure alleen de VM-exemplaren in één regio van het paar bij. Wanneer bijvoorbeeld de virtuele machines in Noord-centraal VS worden bijgewerkt, worden er tegelijkertijd geen virtuele machines in Zuid-centraal VS bijgewerkt. Deze update wordt op een ander tijdstip gepland, om failover of taakverdeling tussen regio's mogelijk te maken. Tegelijkertijd met VS - oost kan er echter wel onderhoud plaatsvinden in andere regio's, zoals Noord-Europa.

Zie de volgende tabel voor de huidige regioparen:

| Regio 1 | Regio 2 |
|:--- | ---:|
| VS - oost |VS - west |
| VS - oost 2 |VS - midden |
| Noord-centraal VS |Zuid-centraal VS |
| West-centraal VS |VS - west 2 |
| Canada - oost |Canada - midden |
| Brazilië - zuid |Zuid-centraal VS |
| VS (overheid) - Iowa |VS (overheid) - Virginia |
| US DoD - oost |US DoD - centraal |
| Noord-Europa |West-Europa |
| Verenigd Koninkrijk West |Verenigd Koninkrijk Zuid |
| Duitsland - centraal |Duitsland - noordoost |
| Zuidoost-Azië |Oost-Azië |
| Australië - zuidoost |Australië - oost |
| India - midden |India - zuid |
| India - west |India - zuid |
| Japan - oost |Japan - west |
| Korea - centraal |Korea - zuid |
| China - oost |China - noord |


<!--Anchors-->
[image1]: ./media/virtual-machines-common-planned-maintenance/vmplanned1.png
[image2]: ./media/virtual-machines-common-planned-maintenance/EventViewerPostReboot.png
[image3]: ./media/virtual-machines-planned-maintenance/RegionPairs.PNG
[image4]: ./media/virtual-machines-common-planned-maintenance/availabilitysetexample.png


<!--Link references-->
[Virtual Machines Manage Availability]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md

[Understand planned versus unplanned maintenance]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
