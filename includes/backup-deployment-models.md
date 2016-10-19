De Azure Backup-service heeft twee soorten kluizen: de Backup-kluis en de Recovery Services-kluis. De Backup-kluis was er het eerst. Vervolgens kwam de Recovery Services-kluis ter ondersteuning van de uitgebreide Resource Manager-implementaties. Microsoft raadt u aan Resource Manager-implementaties te gebruiken, tenzij u specifiek een klassieke implementatie wenst.

| **Implementatie** | **Portal** | **Kluis** |
|-----------|------|-----|
|Klassiek|[Klassiek](https://manage.windowsazure.com)|Back-up|
|Resource Manager|[Azure](https://portal.azure.com)|Recovery Services|

> [AZURE.NOTE] Oplossingen die met Resource Manager zijn geïmplementeerd, kunnen niet met Backup-kluizen worden beveiligd. U kunt wel een Recovery Services-kluis gebruiken om servers en VM's te beschermen die met de klassieke methode zijn geïmplementeerd.  


<!--HONumber=Sep16_HO3-->


