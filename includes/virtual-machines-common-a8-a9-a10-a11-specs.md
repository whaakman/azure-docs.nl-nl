

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie
* **Azure-abonnement** – aan meer dan een paar rekenintensieve instanties implementeert, overweeg dan een abonnement op gebruiksbasis of andere Aankoopopties. Als u een [gratis account van Azure](https://azure.microsoft.com/free/) gebruikt, kunt u slechts een paar Azure Compute-resources van Azure gebruiken.

* **Prijzen en beschikbaarheid** -deze VM-grootten worden aangeboden in de standaard alleen prijscategorie. Controleer [producten beschikbaar per regio] (https://azure.microsoft.com/regions/services/) voor beschikbaarheid in een Azure-regio's. 
* **Quotum voor kernen** – mogelijk moet u het quotum voor kernen in uw Azure-abonnement vanuit de standaardwaarde verhogen. Uw abonnement mogelijk ook beperken het aantal kernen dat u in de grootte van families van bepaalde VM, waaronder de H-reeks kunt implementeren. Om aan te vragen een verhoging van het quotum [opent u een ondersteuningsaanvraag online klant](../articles/azure-supportability/how-to-create-azure-support-request.md) zonder kosten. (De standaardlimiet kunnen variëren, afhankelijk van uw abonnementscategorie).
  
  > [!NOTE]
  > Neem contact op met ondersteuning van Azure als u grootschalige die u nodig hebt. Azure-quota tegoed worden beperkt, niet de garanties capaciteit. Ongeacht de quota u zijn alleen in rekening gebracht voor kernen dat u gebruikt.
  > 
  > 
* **Virtueel netwerk** : een Azure [virtueel netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) is niet vereist voor de exemplaren rekenintensieve gebruiken. Echter, voor veel implementaties moet u ten minste een cloud-gebaseerde Azure-netwerk of een site-naar-site-verbinding als u nodig hebt voor toegang tot lokale bronnen. Indien nodig, maakt u een nieuw virtueel netwerk voor het implementeren van de exemplaren. Rekenintensieve VM's toe te voegen aan een virtueel netwerk in een affiniteitsgroep wordt niet ondersteund.
* **Vergroten of verkleinen** – vanwege hun speciale hardware, u kunt alleen de grootte van rekenintensieve exemplaren binnen dezelfde grootte familie (H-serie of rekenintensieve A-serie). Bijvoorbeeld, u kunt alleen het formaat van een VM H-serie van één H-serie grootte naar een andere. Bovendien wordt vergroten of verkleinen van de grootte van een computer intensieve rekenintensieve grootte niet ondersteund.  
