
* Voor de conversie moet de VM opnieuw worden opgestart. Plan daarom de migratie van uw VM's tijdens een reeds bestaand onderhoudsvenster. 

* De conversie kan niet ongedaan worden gemaakt. 

* Test de conversie. Migreer de test-VM voordat u de migratie in de productieomgeving uitvoert.

* Tijdens de conversie moet u de toewijzing van de VM ongedaan maken. De VM ontvangt een nieuw IP-adres wanneer deze na de conversie wordt opgestart. Indien vereist kunt u [een statisch IP-adres toewijzen](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) aan de VM.

* De oorspronkelijke VHD's en het opslagaccount die vóór de conversie werden gebruikt door de VM worden niet verwijderd. Hiervoor worden nog altijd kosten in rekening gebracht. Als u wilt voorkomen dat er kosten worden doorberekend voor de artefacten, verwijdert u de oorspronkelijke VHD-blobs nadat u hebt gecontroleerd of de conversie is voltooid.
