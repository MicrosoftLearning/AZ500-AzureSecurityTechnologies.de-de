---
lab:
  title: 15 – Azure Sentinel
  module: Module 04 - Manage Security Operations
ms.openlocfilehash: c324766cb22da958236b00df8a872e490cf43b51
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625667"
---
# <a name="lab-15-azure-sentinel"></a>Lab 15: Azure Sentinel
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden gebeten, einen Proof of Concept für Azure Sentinel-basierte Bedrohungserkennung und die Reaktion auf Bedrohungen zu erstellen. Insbesondere möchten Sie Folgendes erledigen:

- Mit dem Sammeln von Daten aus Azure-Aktivität und Security Center beginnen
- Integrierte und benutzerdefinierte Warnungen hinzufügen 
- Überprüfen, wie eine Reaktion auf einen Incident mithilfe von Playbooks automatisiert werden kann

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## <a name="lab-objectives"></a>Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Implementieren von Azure Sentinel

## <a name="lab-files"></a>Labdateien:

- **\\Allfiles\\Labs\\15\\changeincidentseverity.json**

### <a name="exercise-1-implement-azure-sentinel"></a>Übung 1: Implementieren von Azure Sentinel

### <a name="estimated-timing-30-minutes"></a>Geschätzte Zeit: 30 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Durchführen des Onboardings von Azure Sentinel
- Aufgabe 2: Verbinden von Azure-Aktivität mit Sentinel
- Aufgabe 3: Erstellen einer Regel, die den Azure-Aktivitätsdatenconnector verwendet 
- Aufgabe 4: Erstellen eines Playbooks
- Aufgabe 5: Erstellen einer benutzerdefinierten Warnung und Konfigurieren des Playbooks als automatisierte Antwort
- Aufgabe 6: Aufrufen eines Incidents und Überprüfen der zugeordneten Aktionen

#### <a name="task-1-on-board-azure-sentinel"></a>Aufgabe 1: Durchführen des Onboardings von Azure Sentinel

In dieser Aufgabe führen Sie das Onboarding für Azure Sentinel durch und verbinden es mit dem Log Analytics-Arbeitsbereich. 

1. Melden Sie sich am Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich am Azure-Portal mit einem Konto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab nutzen.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure Sentinel** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Azure Sentinel** auf **+ Erstellen**.

1. Wählen Sie auf dem Blatt **Azure Sentinel zu einem Arbeitsbereich hinzufügen** den Log Analytics-Arbeitsbereich aus, den Sie im Lab „Azure Monitor“ erstellt haben, und klicken Sie auf **Hinzufügen**.

    >**Hinweis**: Bei Azure Sentinel gibt es sehr spezifische Anforderungen für Arbeitsbereiche. So können beispielsweise von Azure Security Center erstellte Arbeitsbereiche nicht verwendet werden. Weitere Informationen finden Sie unter [Schnellstart: Durchführen des Onboardings für Azure Sentinel](https://docs.microsoft.com/en-us/azure/sentinel/quickstart-onboard).
    
#### <a name="task-2-configure-azure-sentinel-to-use-the-azure-activity-data-connector"></a>Aufgabe 2: Konfigurieren von Azure Sentinel für die Verwendung des Azure-Aktivitätsdatenconnectors 

In dieser Aufgabe konfigurieren Sie Sentinel für die Verwendung des Azure-Aktivitätsdatenconnectors.  

1. Klicken Sie im Azure-Portal auf dem Blatt **Azure Sentinel \| Übersicht** im Abschnitt **Konfiguration** auf **Datenconnectors**. 

1. Überprüfen Sie auf dem Blatt **Azure Sentinel \| Datenconnectors** die Liste der verfügbaren Connectors, geben Sie in die Suchleiste den Begriff **Azure** ein, wählen Sie den Eintrag für den Connector **Azure-Aktivität** aus (blenden Sie die Menüleiste auf der linken Seite bei Bedarf mit \<< aus), überprüfen Sie die Beschreibung und den Status, und klicken Sie auf **Connectorseite öffnen**.

1. Auf dem Blatt **Azure-Aktivität** sollte die Registerkarte **Anweisungen** ausgewählt sein. Notieren Sie sich die **Voraussetzungen**, und scrollen Sie nach unten zu **Konfiguration**. Notieren Sie sich die Informationen, mit denen das Connectorupdate beschrieben wird. Weil Ihr Azure Pass-Abonnement die Legacy-Verbindungsmethode nie verwendet hat, können Sie Schritt 1 überspringen (die Schaltfläche **Alle trennen** ist abgeblendet) und mit Schritt 2 weitermachen.

1. Lesen Sie in Schritt 2 **Verbinden Sie Ihre Abonnements über die Diagnoseeinstellungen „neue Pipeline“** die Anleitungen unter „Starten Sie den Azure Policy-Zuweisungs-Assistenten, und führen Sie diese Schritte aus“. Klicken Sie dann auf **Azure Policy-Zuweisungs-Assistent starten\>** .

1. Klicken Sie auf der Registerkarte **Azure-Aktivitätsprotokolle zum Streaming an den angegebenen Log Analytics-Arbeitsbereich konfigurieren** (Seite „Richtlinie zuweisen“) **Grundlagen** auf die Schaltfläche mit den Auslassungspunkten **Bereich (...)** . Wählen Sie auf der Seite **Bereich** in der Dropdownliste „Abonnement“ Ihr Azure Pass-Abonnement aus, und klicken Sie unten auf der Seite auf die Schaltfläche **Auswählen**.

    >**Hinweis**:Wählen Sie *keine* Ressourcengruppe aus.

1. Klicken Sie unten auf der Registerkarte **Grundlagen** auf die Schaltfläche **Weiter**, um zur Registerkarte **Parameter** zu wechseln. Klicken Sie dann auf der Registerkarte **Parameter** auf die Schaltfläche mit den Auslassungspunkten **Primärer Log Analytics-Arbeitsbereich (...)** . Vergewissern Sie sich auf der Seite **Primärer Log Analytics-Arbeitsbereich**, dass Ihr Azure Pass-Abonnement ausgewählt ist, und wählen Sie im Dropdown-Listenfeld **Arbeitsbereiche** den Log Analytics-Arbeitsbereich aus, den Sie für Sentinel verwenden. Klicken Sie anschließend unten auf der Seite auf die Schaltfläche **Auswählen**.

1. Klicken Sie unten auf der Registerkarte **Parameter** auf die Schaltfläche **Weiter**, um zur Registerkarte **Wartung** zu wechseln. Aktivieren Sie auf der Registerkarte **Wartung** das Kontrollkästchen **Wartungstask erstellen**. Dadurch wird in der Dropdownliste **Zu korrigierende Richtlinie** die Option „Azure-Aktivitätsprotokolle zum Streaming an den angegebenen Log Analytics-Arbeitsbereich konfigurieren“ aktiviert. Wählen Sie in der Dropdownliste **Standort der systemseitig zugewiesenen Identität** die Region (z. B. „USA, Osten“) aus, die Sie zuvor für Ihren Log Analytics-Arbeitsbereich ausgewählt haben.

1. Klicken Sie unten auf der Registerkarte **Wartung** auf die Schaltfläche **Weiter**, um zur Registerkarte **Nichtkonformitätsmeldung** zu wechseln. Geben Sie bei Bedarf eine Nichtkonformitätsmeldung ein (dies ist optional), und klicken Sie unten auf der Registerkarte **Nichtkonformitätsmeldung** auf die Schaltfläche **Überprüfen + erstellen**.

1. Klicken Sie auf die Schaltfläche **Erstellen** . Sie sollten drei Meldungen zum Status „erfolgreich“ beobachten: **„Richtlinienzuweisung erfolgreich erstellt“, „Rollenzuweisungen erfolgreich erstellt“ und „Wartungstask erfolgreich erstellt“** .

    >**Hinweis**: Sie können das Glockensymbol „Benachrichtigungen“ überprüfen, um die drei erfolgreichen Aufgaben zu überprüfen.

1. Vergewissern Sie sich, dass im Bereich **Azure-Aktivität** der Graph **Empfangene Daten** angezeigt wird (möglicherweise müssen Sie die Browserseite aktualisieren).  

    >**Hinweis**: Es kann länger als 15 Minuten dauern, bis der Status „Verbunden“ lautet und der Graph „Empfangene Daten“ anzeigt.

#### <a name="task-3-create-a-rule-that-uses-the-azure-activity-data-connector"></a>Aufgabe 3: Erstellen einer Regel, die den Azure-Aktivitätsdatenconnector verwendet 

In dieser Aufgabe überprüfen und erstellen Sie eine Regel, die den Azure-Aktivitätsdatenconnector verwendet. 

1. Klicken Sie auf dem Blatt **Azure Sentinel \| Konfiguration** auf **Analysen**. 

1. Klicken Sie auf dem Blatt **Azure Sentinel \| Analysen** auf die Registerkarte **Regelvorlagen**. 

    >**Hinweis**: Überprüfen Sie die Typen von Regeln, die Sie erstellen können. Jede Regel ist einer bestimmten Datenquelle zugeordnet.

1. Geben Sie in der Liste der Regelvorlagen in das Suchleistenformular den Begriff **Suspicious** (Verdächtig) ein, und klicken Sie auf den der Datenquelle **Azure-Aktivität** zugeordneten Eintrag **Suspicious number of resource creation or deployment** (Verdächtige Anzahl von Ressourcenerstellungen oder -bereitstellungen). Klicken Sie dann in dem Bereich, in dem die Eigenschaften der Regelvorlage angezeigt werden, auf **Regel erstellen** (scrollen Sie bei Bedarf auf der Seite nach rechts).

    >**Hinweis**: Diese Regel ist für den mittleren Schweregrad vorgesehen. 

1. Übernehmen Sie auf der Registerkarte **Allgemein** des Blatts **Analyseregel-Assistent – Neue Regel aus Vorlage erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Regellogik festlegen >** .

1. Übernehmen Sie auf der Registerkarte **Regellogik festlegen** des Blatts **Analyseregel-Assistent – Neue Regel aus Vorlage erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Incidenteinstellungen >** .

1. Übernehmen Sie auf der Registerkarte **Incidenteinstellungen** des Blatts **Analyseregel-Assistent – Neue Regel aus Vorlage erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Automatisierte Antwort >** . 

    >**Hinweis**: Hier können Sie einer Regel ein Playbook hinzufügen, das als Logik-App implementiert wird, um die Behebung eines Problems zu automatisieren.

1. Übernehmen Sie auf der Registerkarte **Automatisierte Antwort** des Blatts **Analyseregel-Assistent – Neue Regel aus Vorlage erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Überprüfen >** . 

1. Klicken Sie auf der Registerkarte **Überprüfen und erstellen** des Blatts **Analyseregel-Assistent – Neue Regel aus Vorlage erstellen** auf **Erstellen**.

    >**Hinweis**: Sie haben jetzt eine aktive Regel.

#### <a name="task-4-create-a-playbook"></a>Aufgabe 4: Erstellen eines Playbooks

In dieser Aufgabe erstellen Sie ein Playbook. Ein Sicherheitsplaybook ist eine Sammlung von Aufgaben, die von Azure Sentinel als Reaktion auf eine Warnung aufgerufen werden können. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Text **Benutzerdefinierte Vorlage bereitstellen** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf die Option **Eigene Vorlage im Editor erstellen**.

1. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Datei laden**, navigieren Sie zur Datei **\\Allfiles\\Labs\\15\\changeincidentseverity.json**, und klicken Sie auf **Öffnen**.

    >**Hinweis**: Beispielplaybooks finden Sie unter [https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks).

1. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Speichern**.

1. Sorgen Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** dafür, dass die folgenden Einstellungen konfiguriert werden (übernehmen Sie für alle anderen Einstellungen die Standardwerte):

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab nutzen|
    |Resource group|**AZ500LAB131415**|
    |Standort|**(USA) USA, Osten**|
    |Name des Playbooks|**Change-Incident-Severity** (Schweregrad des Incidents ändern)|
    |Benutzername|Ihre E-Mail-Adresse|

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Ressourcengruppen** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Ressourcengruppen** in der Liste der Ressourcengruppen auf den Eintrag **AZ500LAB131415**.

1. Klicken Sie auf dem Ressourcengruppenblatt **AZ500LAB131415** in der Ressourcenliste auf den Eintrag für die neu erstellte Logik-App **Change-Incident-Severity**.

1. Klicken Sie auf dem Blatt **Change-Incident-Severity** auf **Bearbeiten**.

    >**Hinweis**: Auf dem Blatt **Logic Apps-Designer** wird für jede der vier Verbindungen eine Warnung angezeigt. Dies bedeutet, dass jede Verbindung überprüft und konfiguriert werden muss.

1. Klicken Sie auf dem Blatt **Logic Apps-Designer** auf den ersten Schritt **Verbindungen**.

1. Klicken Sie auf **Neu hinzufügen**, stellen Sie sicher, dass der Eintrag in der Dropdownliste **Mandant** Ihren Azure AD-Mandantennamen enthält, und klicken Sie auf **Anmelden**.

1. Melden Sie sich bei der entsprechenden Aufforderung mit dem Benutzerkonto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab nutzen.

1. Klicken Sie auf den zweiten Schritt **Verbindungen**, und wählen Sie in der Liste der Verbindungen den zweiten Eintrag für die Verbindung aus, die Sie im vorherigen Schritt erstellt haben.

1. Wiederholen Sie die vorherigen Schritte für die beiden verbleibenden **Verbindungen**-Schritte.

    >**Hinweis**: Stellen Sie sicher, dass bei keinem der Schritte Warnungen angezeigt werden.

1. Klicken Sie auf dem Blatt **Logic Apps-Designer** auf **Speichern**, um Ihre Änderungen zu speichern.

#### <a name="task-5-create-a-custom-alert-and-configure-a-playbook-as-an-automated-response"></a>Aufgabe 5: Erstellen einer benutzerdefinierten Warnung und Konfigurieren eines Playbooks als automatisierte Antwort

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Azure Sentinel \| Übersicht**.

1. Klicken Sie auf dem Blatt **Azure Sentinel \| Übersicht** im Abschnitt **Konfiguration** auf **Analysen**.

1. Klicken Sie auf dem Blatt **Azure Sentinel \| Analysen** auf **+ Erstellen** und dann im Dropdownmenü auf **Geplante Abfrageregel**. 

1. Geben Sie auf der Registerkarte **Allgemein** des Blatts **Analyseregel-Assistent – Neue Regel erstellen** die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte):

    |Einstellung|Wert|
    |---|---|
    |Name|**Playbook-Demo**|
    |Taktik|**Erstzugriff**|

1. Klicken Sie auf **Weiter: Regellogik festlegen >** .

1. Fügen Sie auf der Registerkarte **Regellogik festlegen** des Blatts **Analyseregel-Assistent – Neue Regel erstellen** im Textfeld **Regelabfrage** die folgende Regelabfrage ein. 

    ```
    AzureActivity
     | where ResourceProviderValue =~ "Microsoft.Security" 
     | where OperationNameValue =~ "Microsoft.Security/locations/jitNetworkAccessPolicies/delete" 
    ```

    >**Hinweis**: Diese Regel identifiziert das Entfernen von Just-in-Time-VM-Zugriffsrichtlinien.

    >**Hinweis**: Wenn ein Analysefehler angezeigt wird, hat IntelliSense Ihrer Abfrage möglicherweise Werte hinzugefügt. Vergewissern Sie sich, dass die Abfrage übereinstimmt. Fügen Sie andernfalls die Abfrage in Editor und dann aus Editor in die Regelabfrage ein. 


1. Legen Sie auf der Registerkarte **Regellogik festlegen** des Blatts **Analyseregel-Assistent – Neue Regel erstellen** im Abschnitt **Abfrageplanung** den Wert für **Abfrage ausführen alle** auf **5 Minuten** fest.

1. Übernehmen Sie auf der Registerkarte **Regellogik festlegen** des Blatts **Analyseregel-Assistent – Neue Regel erstellen** die Standardwerte der verbleibenden Einstellungen, und klicken Sie auf **Weiter: Incidenteinstellungen >** .

1. Übernehmen Sie auf der Registerkarte **Incidenteinstellungen** des Blatts **Analyseregel-Assistent – Neue Regel erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Automatisierte Antwort >** . 

1. Aktivieren Sie auf der Registerkarte **Automatisierte Antwort** des Blatts **Analyseregel-Assistent – Neue Regel erstellen** in der Dropdownliste **Warnungsautomatisierung** das Kontrollkästchen neben dem Eintrag **Change-Incident-Severity**, und klicken Sie auf **Weiter: Überprüfen >** . 

1. Klicken Sie auf der Registerkarte **Überprüfen und erstellen** des Blatts **Analyseregel-Assistent – Neue Regel erstellen** auf **Erstellen**.

    >**Hinweis**: Sie haben jetzt die neue aktive Regel **Playbook-Demo**. Wenn ein von der Regellogik des identifiziertes Ereignis eintritt, führt dies zu einer Warnung mit mittlerem Schweregrad, die einen entsprechenden Incident generiert.

#### <a name="task-6-invoke-an-incident-and-review-the-associated-actions"></a>Aufgabe 6: Aufrufen eines Incidents und Überprüfen der zugeordneten Aktionen

1. Navigieren Sie im Azure-Portal zum Blatt **Security Center \| Übersicht**.

    >**Hinweis**: Überprüfen Sie Ihre Sicherheitsbewertung. Jetzt sollte sie aktualisiert worden sein. 

1. Klicken Sie auf dem Blatt **Security Center \| Azure Defender** auf den Abschnitt **Just-in-Time-VM-Zugriff**.

1. Klicken Sie auf dem Blatt **Security Center \| Just-in-Time-VM-Zugriff** auf der rechten Seite der Zeile, die auf den virtuellen Computer **myVM** verweist, auf die Schaltfläche mit den Auslassungspunkten ( **...** ), anschließend auf **Entfernen** und dann auf **Ja**.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Aktivitätsprotokoll** ein, und drücken Sie die **EINGABETASTE**.

1. Navigieren Sie zum Blatt **Aktivitätsprotokoll**, und notieren Sie sich den Eintrag **JIT-Netzwerkzugriffsrichtlinien löschen**. 

    >**Hinweis**: Es kann eine Minute dauern, bis dieser Eintrag angezeigt wird. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Azure Sentinel \| Übersicht**.

1. Überprüfen Sie auf dem Blatt **Azure Sentinel \| Übersicht** das Dashboard, und vergewissern Sie sich, dass dort eine Warnung angezeigt wird, die dem Löschen der Just-in-Time-VM-Zugriffsrichtlinie entspricht.

    >**Hinweis**: Es kann bis zu 5 Minuten dauern, bis Warnungen auf dem Blatt **Azure Sentinel \| Übersicht** angezeigt werden. Wenn zu diesem Zeitpunkt keine Warnung angezeigt wird, führen Sie die Abfrageregel aus, auf die in der vorherigen Aufgabe verwiesen wurde. Überprüfen Sie so, ob die Löschaktivität der Just-In-Time-Zugriffsrichtlinie an den Log Analytics-Arbeitsbereich weitergegeben wurde, der Ihrer Azure Sentinel-Instanz zugeordnet ist. Wenn dies nicht zutrifft, erstellen Sie die Just-in-Time-VM-Zugriffsrichtlinie neu, und löschen Sie sie wieder.

1. Klicken Sie auf dem Blatt **Azure Sentinel \| Übersicht** im Abschnitt **Bedrohungsverwaltung** auf **Incidents**.

1. Vergewissern Sie sich, dass auf dem Blatt ein Incident mit mittlerem oder hohem Schweregrad angezeigt wird.

    >**Hinweis**: Es kann bis zu 5 Minuten dauern, bis der Incident auf dem Blatt **Azure Sentinel \| Incidents** angezeigt wird. 

    >**Hinweis**: Überprüfen Sie das Blatt **Azure Sentinel\| Playbooks**. Dort finden Sie die Anzahl der erfolgreichen und fehlgeschlagenen Ausführungen.

    >**Hinweis**: Sie haben die Möglichkeit, einem Incident einen anderen Schweregrad und Status zuzuweisen.

> Ergebnisse: Sie haben einen Azure Sentinel-Arbeitsbereich erstellt, ihn mit Azure-Aktivitätsprotokollen verbunden, ein Playbook und benutzerdefinierte Warnungen erstellt, die als Reaktion auf das Entfernen von Just-in-Time-VM-Zugriffsrichtlinien ausgelöst werden, und schließlich überprüft, ob die Konfiguration gültig ist.

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. 

1. Öffnen Sie im Azure-Portal Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Klicken Sie bei der entsprechenden Aufforderung auf **PowerShell** und dann auf **Speicher erstellen**.

1. Stellen Sie sicher, dass oben links im Bereich „Cloud Shell“ im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

1. Führen Sie in der PowerShell-Sitzung im Bereich „Cloud Shell“ den folgenden Code zum Entfernen der Ressourcengruppe aus, die Sie in diesem Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB131415" -Force -AsJob
    ```
1. Schließen Sie den **Cloud Shell**-Bereich.
