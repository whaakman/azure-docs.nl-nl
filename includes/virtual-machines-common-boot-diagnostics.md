Ondersteuning voor twee foutopsporing functies is nu beschikbaar in Azure: ondersteuning voor Console-uitvoer, en Schermafbeeldingsfunctionaliteit voor virtuele machines van Azure Resource Manager-implementatiemodel. 

Wanneer u uw eigen installatiekopie brengt naar Azure of een van de platform-installatiekopieën zelfs wordt opgestart, kunnen er diverse redenen waarom een virtuele machine in een niet-opstartbare status opgehaald. Deze functies kunnen u gemakkelijk te analyseren en herstellen van uw virtuele machines fouten.

Voor virtuele Linux-machines, kunt u eenvoudig de uitvoer van de console-aanmelding via de Portal bekijken:

![Azure Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
Echter, voor Windows- en Linux virtuele machines Azure kunt u ook een schermopname van de virtuele machine van de hypervisor Zie:

![Fout](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

Beide van deze functies worden ondersteund voor virtuele machines in alle regio's in Azure. Houd er rekening mee dat het tot 10 minuten kan duren voordat de schermafbeeldingen en uitvoer worden weergegeven in uw opslagaccount.

## <a name="common-boot-errors"></a>Veelvoorkomende opstartfouten

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Er is geen besturingssysteem gevonden](https://support.microsoft.com/help/4010142)
- [Opstartfout of OPSTARTAPPARAAT_NIET_TOEGANKELIJK](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Diagnostische gegevens op een nieuwe virtuele machine inschakelen
1. Wanneer u een nieuwe virtuele machine maakt vanuit de Azure Portal, selecteer de **Azure Resource Manager** uit de vervolgkeuzelijst van implementatie model:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. In **instellingen**, inschakelen de **opstarten diagnostics**, en selecteer vervolgens een opslagaccount dat u wilt deze diagnostische bestanden moeten worden geplaatst.
 
    ![VM maken](./media/virtual-machines-common-boot-diagnostics/create-storage-account.png)

    > [!NOTE]
    > De functie voor Boot diagnostics ondersteunt geen premium-opslagaccount. Als u de premium-opslagaccount voor diagnostische gegevens over opstarten gebruikt, ontvangt u mogelijk de StorageAccountTypeNotSupported-fout bij het starten van de virtuele machine.
    >
    > 

3. Als u met een Azure Resource Manager-sjabloon implementeert, gaat u naar de bron van de virtuele machine en de sectie diagnostics-profiel toevoegen. Vergeet niet de API-versieheader '2015-06-15' te gebruiken.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Met het diagnostische profiel kunt u het opslagaccount selecteren waarin u deze logboeken wilt opslaan.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Bekijk onze opslagplaats hier voor het implementeren van een steekproef virtuele machine met diagnostische gegevens over opstarten is ingeschakeld.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Diagnostische gegevens over opstarten op bestaande virtuele machine inschakelen 

Schakel diagnostische gegevens over opstarten op een bestaande virtuele machine door de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en selecteer vervolgens de virtuele machine.
2. In **ondersteuning + probleemoplossing**, selecteer **diagnostics opstarten** > **instellingen**, de status wijzigen in **op**, en vervolgens Selecteer een opslagaccount. 
4. Zorg ervoor dat de optie opstarten diagnostics is geselecteerd en sla de wijziging.

    ![Bestaande VM bijwerken](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

3. Start de VM opnieuw op om de wijzigingen door te voeren.


