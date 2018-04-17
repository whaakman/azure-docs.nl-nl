
* Voor de conversie moet de VM opnieuw worden opgestart. Plan daarom de migratie van uw VM's tijdens een reeds bestaand onderhoudsvenster. 

* De conversie kan niet ongedaan worden gemaakt. 

* Houd er rekening mee dat elke worden gebruikers met de [Virtual Machine Contributor](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol kan niet worden de VM-grootte wijzigen (zoals ze vooraf conversie kunnen). Dit is omdat de virtuele machines met beheerde schijven moeten de gebruiker gemachtigd is Microsoft.Compute/disks/write op de OS-schijven.

* Test de conversie. Migreer de test-VM voordat u de migratie in de productieomgeving uitvoert.

* Tijdens de conversie moet u de toewijzing van de VM ongedaan maken. De VM ontvangt een nieuw IP-adres wanneer deze na de conversie wordt opgestart. Indien vereist kunt u [een statisch IP-adres toewijzen](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) aan de VM.

* De oorspronkelijke VHD's en het opslagaccount die vóór de conversie werden gebruikt door de VM worden niet verwijderd. Hiervoor worden nog altijd kosten in rekening gebracht. Als u wilt voorkomen dat er kosten worden doorberekend voor de artefacten, verwijdert u de oorspronkelijke VHD-blobs nadat u hebt gecontroleerd of de conversie is voltooid.

* Controleer de minimale versie van de Azure VM-agent vereist voor de ondersteuning van het conversieproces. Zie voor meer informatie over het controleren en werk uw agentversie [minimaal versie-ondersteuning voor VM-agents in Azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)