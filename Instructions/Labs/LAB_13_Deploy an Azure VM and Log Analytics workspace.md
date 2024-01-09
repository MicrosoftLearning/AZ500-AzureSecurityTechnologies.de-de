---
lab:
  title: 13 – Bereitstellen einer Azure-VM und eines Log Analytics-Arbeitsbereichs
  module: Module 04 - Manage security operations
---

# Lab 13: Bereitstellen einer Azure-VM und eines Log Analytics-Arbeitsbereichs

# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie wurden aufgefordert, eine Azure-VM und einen Log Analytics-Arbeitsbereich zu erstellen.

- Stellen Sie eine Azure-VM bereit.
- Erstellen eines Log Analytics-Arbeitsbereichs

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Erstellen eines virtuellen Azure-Computers
  
## Anweisungen

### Übung 1: Bereitstellen einer Azure-VM

### Übungszeitrahmen: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus: 

- Aufgabe 1: Bereitstellen eines virtuellen Azure-Computers 

#### Aufgabe 1: Bereitstellen eines virtuellen Azure-Computers

1. Melden Sie sich am Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich am Azure-Portal mit einem Konto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab nutzen.

2. Öffnen Sie Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Wenn Sie dazu aufgefordert werden, wählen Sie **PowerShell** und dann **Speicher erstellen** aus.

3. Stellen Sie sicher, dass oben links im Bereich „Cloud Shell“ im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

4. Führen Sie in der PowerShell-Sitzung im Bereich „Cloud Shell“ den folgenden Code zum Erstellen einer Ressourcengruppe aus, die Sie in diesem Lab verwenden:
  
    ```powershell
    New-AzResourceGroup -Name AZ500LAB131415 -Location 'EastUS'
    ```

    >**Hinweis**: Diese Ressourcengruppe wird für die Labs 13, 14 und 15 verwendet. 

5. Führen Sie in der PowerShell-Sitzung im Bereich „Cloud Shell“ Folgendes aus, um einen neuen virtuellen Azure-Computer zu erstellen: 

    ```powershell
    New-AzVm -ResourceGroupName "AZ500LAB131415" -Name "myVM" -Location 'EastUS' -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName   "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress" -PublicIpSku Standard -OpenPorts 80,3389 -Size Standard_DS1_v2 
    ```
    
6.  Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden:

    |Einstellung|Wert|
    |---|---|
    |Benutzer |**localadmin**|
    |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 04 in der Übung 1 und dem Vorgang 1 in Schritt 9 erstellt haben.**|

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. 

7. Führen Sie in der PowerShell-Sitzung im Bereich „Cloud Shell“ Folgendes aus, um zu bestätigen, dass der virtuelle Computer **myVM** erstellt wurde und sein Status **provisioningState** **Erfolgreich** lautet:

    ```powershell
    Get-AzVM -Name 'myVM' -ResourceGroupName 'AZ500LAB131415' | Format-Table
    ```

8. Schließen Sie den Cloud Shell-Bereich. 

#### Aufgabe 2: Erstellen eines Log Analytics-Arbeitsbereichs

In dieser Aufgabe erstellen Sie einen Log Analytics-Arbeitsbereich. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Log Analytics-Arbeitsbereiche** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Log Analytics-Arbeitsbereiche** auf  **+ Erstellen**.

3. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Log Analytics-Arbeitsbereiche erstellen** die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte):

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
    |Resource group|**AZ500LAB131415**|
    |Name|Ein beliebiger gültiger, global eindeutiger Name|
    |Region|**USA, Osten**|

4. Klicken Sie auf **Überprüfen + erstellen**.

5. Wählen Sie auf dem Blatt **Log Analytics-Arbeitsbereich erstellen** auf der Registerkarte **Überprüfen + erstellen** die Option **Erstellen** aus.

> Ergebnisse: Sie haben eine Azure-VM bereitgestellt und einen Log Analytics-Arbeitsbereich erstellt.

>**Hinweis**: Entfernen Sie nicht die Ressourcen aus diesem Lab, da sie für das Microsoft Defender for Cloud-Lab und das Microsoft Sentinel-Lab benötigt werden.
 
