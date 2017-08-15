Ondersteuning voor twee foutopsporingsfuncties is nu beschikbaar in Azure: ondersteuning voor Console-uitvoer en Schermafbeelding voor het Azure Virtual Machines Resource Manager-implementatiemodel. 

Wanneer u uw eigen installatiekopie importeert in Azure of een van de installatiekopieën van het platform opstart, kan een VM vanwege verschillende redenen in een status belanden waarin deze niet kan worden opgestart. Met deze functies kunt u uw virtuele machines eenvoudig analyseren en herstellen na fouten bij het opstarten.

Bij virtuele Linux-machines kunt u eenvoudig de uitvoer van uw consolelogboek bekijken via de portal:

![Azure Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
U kunt echter bij zowel virtuele Windows- als Linux-machines een schermopname van de virtuele machine bekijken via de hypervisor van Azure:

![Fout](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

Beide functies worden ondersteund voor virtuele Azure-machines in alle regio's. Houd er rekening mee dat het tot 10 minuten kan duren voordat de schermafbeeldingen en uitvoer worden weergegeven in uw opslagaccount.

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
1. Wanneer u een nieuwe virtuele machine maakt in de Preview Portal, selecteert u de **Azure Resource Manager** uit de vervolgkeuzelijst van het implementatiemodel:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. Configureer de optie Bewaking om het opslagaccount te selecteren waarin u wilt deze diagnostische bestanden wilt plaatsen.
 
    ![VM maken](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. Als u met een Azure Resource Manager-sjabloon implementeert, gaat u naar de resource van de virtuele machine en voegt u de sectie diagnostisch profiel toe. Vergeet niet de API-versieheader '2015-06-15' te gebruiken.

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

Bekijk hier onze opslagplaats om een voorbeeld-VM te implementeren waarop met diagnostische gegevens over het opstarten zijn ingeschakeld.

## <a name="update-an-existing-virtual-machine"></a>Een bestaande virtuele machine bijwerken ##

U kunt ook een bestaande virtuele machine bijwerken via de portal zodat de diagnostische gegevens over het opstarten via de portal zijn ingeschakeld. Selecteer de optie Diagnostische gegevens over het opstarten en selecteer vervolgens Opslaan. Start de VM opnieuw op om de wijzigingen door te voeren.

![Bestaande VM bijwerken](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

