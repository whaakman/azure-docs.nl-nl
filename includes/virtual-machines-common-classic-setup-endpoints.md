
Elk eindpunt heeft een *openbare poort* en een *particuliere poort*:

* De openbare poort wordt gebruikt door de Azure load balancer te luisteren naar binnenkomende verkeer aan de virtuele machine vanaf het Internet.
* De particuliere poort wordt gebruikt door de virtuele machine om te luisteren naar binnenkomende verkeer, meestal is bestemd voor een toepassing of service wordt uitgevoerd op de virtuele machine.

Standaardwaarden voor de IP-protocol en TCP of UDP-poorten voor bekende netwerk protocollen beschikbaar zijn wanneer u eindpunten met de Azure-portal maken. Voor aangepaste eindpunten moet u het juiste IP-protocol (TCP of UDP) en de openbare en particuliere poort opgeven. Als u wilt willekeurige binnenkomend verkeer verdelen over meerdere virtuele machines, moet u een set met gelijke taakverdeling die bestaan uit meerdere eindpunten maken.

Nadat u een eindpunt hebt gemaakt, kunt u een toegangsbeheerlijst (ACL) voor het definiëren van regels voor toestaan of weigeren van het binnenkomende verkeer naar de openbare poort van het eindpunt op basis van de bron-IP-adres. Als de virtuele machine zich in een Azure-netwerk, moet u in plaats daarvan een netwerkbeveiligingsgroepen gebruiken. Zie voor meer informatie [over netwerkbeveiligingsgroepen](../articles/virtual-network/virtual-networks-nsg.md).

> [!NOTE]
> Firewall-configuratie voor virtuele machines in Azure gebeurt automatisch voor de poorten die zijn gekoppeld aan externe connectiviteit-eindpunten die Azure automatisch ingesteld. Er is geen configuratie wordt voor poorten die zijn opgegeven voor alle andere eindpunten, automatisch uitgevoerd aan de firewall van de virtuele machine. Wanneer u een eindpunt voor de virtuele machine maakt, moet u ervoor te zorgen dat de firewall van de virtuele machine ook het verkeer voor het protocol en particuliere poort overeenkomt met de eindpuntconfiguratie toestaat. Zie de documentatie of de online help voor het besturingssysteem op de virtuele machine configureren van de firewall.
>
>

## <a name="create-an-endpoint"></a>Een eindpunt maken
1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als u dat nog niet hebt gedaan.
2. Klik op **virtuele Machines**, en klik vervolgens op de naam van de virtuele machine die u wilt configureren.
3. Klik op **eindpunten** in de **instellingen** groep. De **eindpunten** pagina worden alle huidige eindpunten voor de virtuele machine. (In dit voorbeeld is een virtuele machine van Windows. Een Linux-VM wordt standaard weergegeven een eindpunt voor SSH.)

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Eindpunten](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)

4. Klik in de opdrachtbalk boven de eindpunt-vermeldingen op **toevoegen**.
5. Op de **eindpunt toevoegen** pagina, typ een naam voor het eindpunt in **naam**.
6. In **Protocol**, kiest u **TCP** of **UDP**.
7. In **openbare poort**, typt u het poortnummer voor het binnenkomende verkeer vanaf Internet. In **particuliere poort**, typ het poortnummer waar de virtuele machine luistert. Deze poortnummers kan afwijken. Zorg ervoor dat de firewall op de virtuele machine is geconfigureerd voor het verkeer dat overeenkomt met het protocol (in stap 6) en particuliere poort.
10. Klik op **OK**.

Het nieuwe eindpunt wordt weergegeven op de **eindpunten** pagina.

![Maken van het eindpunt geslaagd](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-the-acl-on-an-endpoint"></a>De ACL voor een eindpunt beheren
Als u definieert de set van computers die verkeer kunnen verzenden, kunt de ACL voor een eindpunt op basis van IP-bronadres verkeer beperken. Volg deze stappen als u wilt toevoegen, wijzigen of verwijderen van een ACL voor een eindpunt.

> [!NOTE]
> Als het eindpunt deel uit van een set met gelijke taakverdeling maakt, moet u alle wijzigingen aan de ACL voor een eindpunt worden toegepast op alle eindpunten in de set.
>
>

Als de virtuele machine zich in een Azure-netwerk, raden wij aan netwerkbeveiligingsgroepen in plaats van ACL's. Zie voor meer informatie [over netwerkbeveiligingsgroepen](../articles/virtual-network/virtual-networks-nsg.md).

1. Als u dit nog niet hebt gedaan, aanmelding bij de Azure-portal.
2. Klik op **virtuele Machines**, en klik vervolgens op de naam van de virtuele machine die u wilt configureren.
3. Klik op **Eindpunten**. Selecteer het juiste eindpunt in de lijst. De ACL-lijst is aan de onderkant van de pagina.

   ![Geef details op ACL](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Gebruik de rijen in de lijst wilt toevoegen, verwijderen, of de regels voor een ACL bewerken en de volgorde wijzigen. De **extern Subnet** waarde is een IP-adresbereik voor inkomend verkeer van het Internet die gebruikmaakt van de Azure load balancer toestaan of weigeren van het verkeer op basis van de bron-IP-adres. Zorg ervoor dat het IP-adresbereik in CIDR-notatie, ook wel bekend als adres voorvoegselindeling opgeven. Een voorbeeld is `10.1.0.0/8`.

 ![Nieuwe ACL-vermelding](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


U kunt regels gebruiken alleen het verkeer van specifieke computers die overeenkomen met uw computers op het Internet toestaan of weigeren van verkeer van specifieke, bekende adresbereiken.

De regels worden geëvalueerd in de volgorde en de eerste regel begint en eindigt met de laatste regel. Dit betekent dat regels moeten worden gerangschikt van minst beperkende naar de meest beperkende. Zie voor meer informatie en voorbeelden [wat is er een Network Access Control List](../articles/virtual-network/virtual-networks-acl.md).
