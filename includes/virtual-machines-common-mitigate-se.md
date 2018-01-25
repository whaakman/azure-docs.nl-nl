


**Laatste document update**: 22 januari 3:00 PM PST.

De recente openbaarmaking van een [nieuwe klasse van CPU-beveiligingsproblemen](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) die bekend staat als speculatieve uitvoering side-kanaal aanvallen heeft geleid tot vragen van klanten meer duidelijkheid te zoeken.  

De infrastructuur die wordt uitgevoerd van Azure en werkbelastingen van de klant van elkaar isoleert is beveiligd.  Dit betekent dat andere klanten uitgevoerd op Azure uw toepassing met behulp van deze beveiligingslekken kunnen geen aanvallen.

> [!NOTE] 
> Azure oplossingen eerder aangekondigd op 3 Jan 2018 worden hierdoor niet beïnvloed door de recente [richtlijnen bijgewerkt](https://newsroom.intel.com/news/root-cause-of-reboot-issue-identified-updated-guidance-for-customers-and-partners/) van Intel. Er zijn geen extra onderhoud activiteit van de klant VM's als gevolg van deze nieuwe gegevens.
>
> Er wordt nog deze best practices bijwerken wanneer we microcode-updates van hardwareleveranciers ontvangen. Controleer of terug naar het bijgewerkte richtlijnen.
>

## <a name="keeping-your-operating-systems-up-to-date"></a>Uw besturingssystemen up-to-date te houden

Tijdens een update van het besturingssysteem niet vereist is voor het isoleren van uw toepassingen die worden uitgevoerd op Azure van andere klanten uitgevoerd op Azure, maar het is altijd een best practice uw versies van het besturingssysteem up-to-date kunt houden. 

In de volgende aanbiedingen vindt hier u de aanbevolen acties in uw besturingssysteem bijwerken: 

<table>
<tr>
<th>Aanbieding</th> <th>Aanbevolen actie </th>
</tr>
<tr>
<td>Azure Cloud Services </td>  <td>Automatisch bijwerken inschakelen of zorg ervoor dat u de nieuwste Gast-besturingssysteem op worden uitgevoerd.</td>
</tr>
<tr>
<td>Azure Linux virtuele Machines</td> <td>Updates installeren vanaf uw besturingssysteem provider indien beschikbaar. </td>
</tr>
<tr>
<td>Windows Azure virtuele Machines </td> <td>Controleer of u een ondersteunde antivirusprogramma toepassing worden uitgevoerd voordat u updates voor het besturingssysteem installeert. Neem contact op met de leverancier van uw antivirussoftware voor informatie over de compatibiliteit.<p> Installeer de [totalisering van de beveiliging januari](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </p></td>
</tr>
<tr>
<td>Andere Azure PaaS-Services</td> <td>Er is geen actie nodig is voor klanten die gebruikmaken van deze services. Azure blijft automatisch de versies van het besturingssysteem up-to-date. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Aanvullende richtlijnen als u werkt met niet-vertrouwde code 

Geen aanvullende klant-actie is vereist tenzij u niet-vertrouwde code uitvoert. Als u de code die u niet vertrouwt toestaat (u kunt bijvoorbeeld een van uw klanten voor het uploaden van een binair of codefragment die u vervolgens wordt uitgevoerd in de cloud in uw toepassing), en vervolgens de volgende aanvullende stappen moeten worden genomen.  


### <a name="windows"></a>Windows 
Als u met behulp van Windows en niet-vertrouwde code te hosten, moet u ook een Windows-functie aangeroepen Kernel virtueel adres (KVA) schaduwkopie maken die zorgt voor extra beveiliging tegen speculatieve beveiligingslekken voor side-kanaal inschakelen. Deze functie is standaard uitgeschakeld en kan invloed hebben op prestaties als ingeschakeld. Ga als volgt [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) instructies voor het inschakelen van beveiliging op de Server. Als u Azure Cloud Services uitvoert, controleert u of dat u altijd WA-GUEST-OS-5.15_201801-01 of WA-GUEST-OS-4.50_201801-01 (beschikbaar vanaf op 10 januari) en schakelt u het register sleutel via een taak starten.


### <a name="linux"></a>Linux
Als u met behulp van Linux en niet-vertrouwde code te hosten, moet u ook Linux bijwerken naar een recentere versie waarop kernel tabel pagina isolatie (KPTI) die worden gescheiden van de pagina tabellen die worden gebruikt door de kernel van de referenties die horen bij de gebruikersruimte die is geïmplementeerd. Deze oplossingen vereisen een update voor de Linux-besturingssysteem en kunnen worden verkregen van uw provider distributie indien beschikbaar. Uw OS-provider kunt u zien of beveiliging zijn ingeschakeld of standaard uitgeschakeld.



## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [beveiligen van Azure-klanten CPU beveiligingslek](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
