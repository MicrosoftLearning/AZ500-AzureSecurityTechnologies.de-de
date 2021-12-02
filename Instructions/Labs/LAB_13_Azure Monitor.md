---
lab:
  title: 13 – Azure Monitor
  module: Module 04 - Manage security operations
ms.openlocfilehash: cf654eb2550d7daf9f416953746e0e432f3d2662
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625644"
---
# <a name="lab-13-azure-monitor"></a>Lab 13: Azure Monitor
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden gebeten, einen Proof of Concept zur Überwachung der Leistung eines virtuellen Computers zu erstellen. Insbesondere möchten Sie Folgendes erledigen:

- Einen virtuellen Computer so konfigurieren, dass Telemetriedaten und Protokolle gesammelt werden können.
- Zeigen, welche Telemetriedaten und Protokolle gesammelt werden können.
- Zeigen, wie die Daten verwendet und abgefragt werden können. 

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## <a name="lab-objectives"></a>Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Sammeln von Daten von einem virtuellen Azure-Computer mit Azure Monitor

### <a name="exercise-1-collect-data-from-an-azure-virtual-machine-with-azure-monitor"></a>Übung 1: Sammeln von Daten von einem virtuellen Azure-Computer mit Azure Monitor

### <a name="exercise-timing-20-minutes"></a>Übungszeitrahmen: 20 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus: 

- Aufgabe 1: Bereitstellen eines virtuellen Azure-Computers 
- Aufgabe 2: Erstellen eines Log Analytics-Arbeitsbereichs
- Aufgabe 3: Aktivieren der Log Analytics-VM-Erweiterung
- Aufgabe 4: Sammeln von Ereignis- und Leistungsdaten für virtuelle Computer
- Aufgabe 5: Anzeigen und Abfragen von gesammelten Daten 

#### <a name="task-1-deploy-an-azure-virtual-machine"></a>Aufgabe 1: Bereitstellen eines virtuellen Azure-Computers

1. Melden Sie sich am Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich am Azure-Portal mit einem Konto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab nutzen.

1. Öffnen Sie Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Wählen Sie bei der entsprechenden Aufforderung **PowerShell** und **Speicher erstellen** aus.

1. Stellen Sie sicher, dass oben links im Bereich „Cloud Shell“ im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

1. Führen Sie in der PowerShell-Sitzung im Bereich „Cloud Shell“ den folgenden Code zum Erstellen einer Ressourcengruppe aus, die Sie in diesem Lab verwenden:
  
    ```powershell
    New-AzResourceGroup -Name AZ500LAB131415 -Location 'EastUS'
    ```

    >**Hinweis**: Diese Ressourcengruppe wird für die Labs 13, 14 und 15 verwendet. 

1. Führen Sie in der PowerShell-Sitzung im Bereich „Cloud Shell“ Folgendes aus, um einen neuen virtuellen Azure-Computer zu erstellen: 

    ```powershell
    New-AzVm -ResourceGroupName "AZ500LAB131415" -Name "myVM" -Location 'EastUS' -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName   "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress" -OpenPorts 80,3389
    ```

1.  Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden:

    |Einstellung|Wert|
    |---|---|
    |Benutzername|**localadmin**|
    |Kennwort|**Pa55w.rd1234**|

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. 

1. Führen Sie in der PowerShell-Sitzung im Bereich „Cloud Shell“ Folgendes aus, um zu bestätigen, dass der virtuelle Computer **myVM** erstellt wurde und sein Status **provisioningState** **Erfolgreich** lautet:

    ```powershell
    Get-AzVM -Name 'myVM' -ResourceGroupName 'AZ500LAB131415' | Format-Table
    ```

1. Schließen Sie den Cloud Shell-Bereich. 

#### <a name="task-2-create-a-log-analytics-workspace"></a>Aufgabe 2: Erstellen eines Log Analytics-Arbeitsbereichs

In dieser Aufgabe erstellen Sie einen Log Analytics-Arbeitsbereich. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Log Analytics-Arbeitsbereiche** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Log Analytics-Arbeitsbereiche** auf **+ Erstellen**.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Log Analytics-Arbeitsbereiche erstellen** die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte):

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab nutzen|
    |Resource group|**AZ500LAB131415**|
    |Name|Ein beliebiger gültiger, global eindeutiger Name|
    |Region|**(USA) USA, Osten**|

1. Klicken Sie auf **Weiter: Tarif >** , übernehmen Sie auf dem Blatt **Log Analytics-Arbeitsbereich erstellen** auf der Registerkarte **Tarif** den Standardtarif für **Nutzungsbasierte Bezahlung (pro GB 2018)** , und klicken Sie auf **Überprüfen + erstellen**.

1. Klicken Sie auf dem Blatt **Log Analytics-Arbeitsbereich erstellen** auf der Registerkarte **Überprüfen + erstellen** auf **Erstellen**.

#### <a name="task-3-enable-the-log-analytics-virtual-machine-extension"></a>Aufgabe 3: Aktivieren der Log Analytics-VM-Erweiterung

In dieser Aufgabe aktivieren Sie die Log Analytics-VM-Erweiterung. Diese Erweiterung installiert den Log Analytics-Agent auf virtuellen Windows- und Linux-Computern. Dieser Agent sammelt Daten vom virtuellen Computer und überträgt sie in den von Ihnen festgelegten Log Analytics-Arbeitsbereich. Sobald der Agent installiert wurde, wird für ihn automatisch ein Upgrade durchgeführt, um sicherzustellen, dass Sie immer über die neuesten Features und Hotfixes verfügen. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Log Analytics-Arbeitsbereiche**, und klicken Sie in der Liste der Arbeitsbereiche auf den Eintrag für den Arbeitsbereich, den Sie in der vorherigen Aufgabe erstellt haben.

1. Klicken Sie auf dem Blatt „Log Analytics-Arbeitsbereich“ im Abschnitt **Mit einer Datenquelle verbinden** auf den Eintrag **Virtuelle Computer**.

    >**Hinweis**: Damit der Agent erfolgreich installiert werden kann, muss der virtuelle Computer ausgeführt werden.

1. Suchen Sie in der Liste der virtuellen Computer den Eintrag für die Azure-VM **myVM**, die Sie in der ersten Aufgabe dieser Übung bereitgestellt haben, und beachten Sie, dass sie als **Nicht verbunden** aufgeführt wird.

1. Klicken Sie auf den Eintrag **myVM** und dann auf dem Blatt **myVM** auf **Verbinden**. 

1. Warten Sie, bis der virtuelle Computer eine Verbindung mit dem Log Analytics-Arbeitsbereich herstellt.

    >**Hinweis**: Dies kann einige Minuten dauern. Der auf dem Blatt **myVM** angezeigte **Status** ändert sich von **Verbindung wird hergestellt mit** in **Dieser Arbeitsbereich**. 

#### <a name="task-4-collect-virtual-machine-event-and-performance-data"></a>Aufgabe 4: Sammeln von Ereignis- und Leistungsdaten für virtuelle Computer

In dieser Aufgabe konfigurieren Sie die Sammlung des Windows-Systemprotokolls und von mehreren allgemeinen Leistungsindikatoren. Außerdem überprüfen Sie andere verfügbare Quellen.

1. Navigieren Sie im Azure-Portal zurück zu dem Log Analytics-Arbeitsbereich, den Sie weiter oben in dieser Übung erstellt haben.

1. Klicken Sie auf dem Blatt „Log Analytics-Arbeitsbereich“ im Abschnitt **Einstellungen** auf **Agents-Konfiguration**.

1. Überprüfen Sie auf dem Blatt **Agents-Konfiguration** die konfigurierbaren Einstellungen, einschließlich „Windows-Ereignisprotokolle“, „Windows-Leistungsindikatoren“, „Linux-Leistungsindikatoren“, „IIS-Protokolle“ und „Syslog“. 

1. Stellen Sie sicher, dass **Windows-Ereignisprotokolle** ausgewählt ist. Klicken Sie in der Liste der Ereignisprotokolltypen auf **+ Windows-Ereignisprotokoll hinzufügen**, und wählen Sie **System** aus.

    >**Hinweis**: Auf diese Weise fügen Sie dem Arbeitsbereich Ereignisprotokolle hinzu. Andere Optionen sind z. B. **Hardwareereignisse** oder **Schlüsselverwaltungsdienst**.  

1. Deaktivieren Sie das Kontrollkästchen **Informationen**, und lassen Sie die Kontrollkästchen **Fehler** und **Warnung** aktiviert.

1. Klicken Sie auf **Windows-Leistungsindikatoren**, anschließend auf **+ Leistungsindikator hinzufügen**, überprüfen Sie die Liste der verfügbaren Leistungsindikatoren, und fügen Sie die folgenden hinzu:

    - Prozess(\*)\%Prozessorzeit
    - Ereignisablaufverfolgung für Windows\Gesamtspeicherauslastung – nicht ausgelagerter Pool
    - Ereignisablaufverfolgung für Windows\Gesamtspeicherauslastung – ausgelagerter Pool

    >**Hinweis**: Die Leistungsindikatoren werden hinzugefügt und mit einem Sammlungsstichprobenintervall von 60 Sekunden konfiguriert.
  
1. Klicken Sie auf dem Blatt **Agents-Konfiguration** auf **Anwenden**.

#### <a name="task-5-view-and-query-collected-data"></a>Aufgabe 5: Anzeigen und Abfragen von gesammelten Daten

In dieser Aufgabe führen Sie eine Protokollsuche für Ihre Datensammlung durch. 

1. Navigieren Sie im Azure-Portal zurück zu dem Log Analytics-Arbeitsbereich, den Sie weiter oben in dieser Übung erstellt haben.

1. Klicken Sie auf dem Blatt „Log Analytics-Arbeitsbereich“ im Abschnitt **Allgemein** auf **Protokolle**.

1. Schließen Sie bei Bedarf das Fenster **Willkommen bei Log Analytics!** . 

1. Scrollen Sie im Bereich **Abfragen** in der Spalte **Alle Abfragen** nach unten bis zum Ende der Liste mit den Ressourcentypen, und klicken Sie auf **Virtuelle Computer**.
    
1. Überprüfen Sie die Liste der vordefinierten Abfragen, wählen Sie **Arbeitsspeicher- und CPU-Auslastung** aus, und klicken Sie auf die entsprechende Schaltfläche **Ausführen**.

    >**Hinweis**: Sie können mit der Abfrage **Verfügbarer Arbeitsspeicher des virtuellen Computers** beginnen. Wenn keine Ergebnisse angezeigt werden, überprüfen Sie, ob der Bereich auf den virtuellen Computer festgelegt wurde.

1. Die Abfrage wird automatisch auf einer neuen Abfrageregisterkarte geöffnet. 

    >**Hinweis**: Log Analytics verwendet die Abfragesprache „Kusto“. Sie können die vorhandenen Abfragen anpassen oder eigene erstellen. 

    >**Hinweis**: Die Ergebnisse der ausgewählten Abfrage werden unter dem Abfragebereich automatisch angezeigt. Wenn Sie die Abfrage erneut ausführen möchten, klicken Sie **Ausführen**.

    >**Hinweis**: Da dieser virtuelle Computer soeben erstellt wurde, sind möglicherweise noch keine Daten vorhanden. 

    >**Hinweis**: Sie haben die Möglichkeit, Daten in verschiedenen Formaten anzuzeigen. Außerdem haben Sie die Möglichkeit, eine Warnungsregel basierend auf den Ergebnissen der Abfrage zu erstellen.

    >**Hinweis**: Mit den folgenden Schritten können Sie zusätzliche Last auf der Azure-VM generieren, die Sie weiter oben in diesem Lab bereitgestellt haben:

    1. Navigieren Sie zum Blatt „Azure-VM“.
    1. Wählen Sie auf dem Blatt „Azure-VM“ im Abschnitt **Vorgänge** die Option **Befehl ausführen** aus. Geben Sie auf dem Blatt **Befehlsskript ausführen** das folgende Skript ein, und klicken Sie auf **Ausführen**:
    2. 
       ```cmd
       cmd
       :loop
       dir c:\ /s > SWAP
       goto loop
       ```
       
    1. Wechseln Sie zurück zum Blatt „Log Analytics“, und führen Sie die Abfrage erneut aus. Möglicherweise müssen Sie einige Minuten warten, bis Daten gesammelt wurden, und dann die Abfrage erneut ausführen.

> Ergebnisse: Sie haben mithilfe eines Log Analytics-Arbeitsbereichs Datenquellen und Abfrageprotokolle konfiguriert. 

**Bereinigen von Ressourcen**

>**Hinweis**: Entfernen Sie nicht die Ressourcen aus diesem Lab, da sie für das Azure Security Center-Lab und das Azure Sentinel-Lab benötigt werden.
 
