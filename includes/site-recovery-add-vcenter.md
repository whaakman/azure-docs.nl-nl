* Geef in **vCenter toevoegen** een beschrijvende naam op voor de vSphere-host of vCenter-server, en geef vervolgens het IP-adres of de FQDN op voor de server. Behoud poort 443 tenzij de VMware-servers zijn geconfigureerd om te luisteren naar aanvragen van een andere poort. Selecteer het account dat is verbonden met de VMware vCenter-of vSphere ESXi-server. Klik op **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Als u de VMware vCenter-server of VMware vSphere-host toevoegt met een account zonder beheerdersbevoegdheden voor de vCenter- of hostserver, moet u ervoor zorgen dat voor het account deze bevoegdheden zijn ingeschakeld: Datacenter, Gegevensopslag, Map, Host, Netwerk, Resource, Virtuele machine en Gedistribueerde vSphere-switch. Daarnaast moet voor de VMware vCenter-server de bevoegdheid Opslagweergaven zijn ingeschakeld.
