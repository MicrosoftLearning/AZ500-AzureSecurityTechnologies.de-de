---
lab:
  title: '11: Microsoft Sentinel'
  module: Module 04 - Configure and manage security monitoring and automation solutions
---

# Lab 11: Microsoft Sentinel
# Lab-Handbuch für Kursteilnehmende

## Labszenario

**Hinweis**: **Azure Sentinel** wurde in **Microsoft Sentinel** umbenannt. 

Sie wurden gebeten, einen Proof of Concept für eine Microsoft Sentinel-basierte Bedrohungserkennung und die Reaktion auf Bedrohungen zu erstellen. Insbesondere geht es Ihnen um Folgendes:

- Daten aus Azure-Aktivitäten und Microsoft Defender für Cloud sammeln
- Integrierte und benutzerdefinierte Warnungen hinzufügen 
- Überprüfen, wie eine Reaktion auf einen Incident mithilfe von Playbooks automatisiert werden kann

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Implementieren von Microsoft Sentinel

## Microsoft Sentinel-Diagramm

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/509aa70d-de11-4470-a289-877fbfecbc00)

## Anweisungen

## Labdateien:

- **\\Allfiles\\Labs\\15\\changeincidentseverity.json**

### Übung 1: Implementieren von Microsoft Sentinel

### Geschätzte Zeit: 30 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Einführen von Microsoft Sentinel
- Aufgabe 2: Verbinden von Azure-Aktivität mit Sentinel
- Aufgabe 3: Erstellen einer Regel, die den Azure-Aktivitätsdatenconnector verwendet 
- Aufgabe 4: Erstellen eines Playbooks
- Aufgabe 5: Erstellen einer benutzerdefinierten Warnung und Konfigurieren des Playbooks als automatisierte Antwort
- Aufgabe 6: Aufrufen eines Incidents und Überprüfen der zugeordneten Aktionen

#### Aufgabe 1: Einführen von Microsoft Sentinel

In dieser Aufgabe führen Sie das Onboarding für Microsoft Sentinel durch und verbinden den Dienst mit dem Log Analytics-Arbeitsbereich. 

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, dem in dem Azure-Abonnement, das Sie für dieses Lab verwenden, die Rolle „Besitzer“ oder „Mitwirkender“ zugewiesen ist.

2. Geben Sie im Azure-Portal oben auf der Azure-Portal-Seite im Textfeld **Ressourcen, Dienste und Dokumente durchsuchen** den Begriff **Microsoft Sentinel** ein, und drücken Sie die **EINGABETASTE**.

    >**Hinweis:** Wenn Sie zum ersten Mal eine Aktion mit Microsoft Sentinel im Azure-Dashboard ausführen möchten, führen Sie die folgenden Schritte durch: Geben Sie im Azure-Portal oben auf der Azure-Portal-Seite im Textfeld **Ressourcen, Dienste und Dokumente durchsuchen** den Begriff **Microsoft Sentinel** ein, und drücken Sie die **EINGABETASTE**. Wählen Sie in der Ansicht **Dienste** die Option **Microsoft Sentinel** aus.
  
3. Klicken Sie auf dem Blatt **Microsoft Sentinel** auf **+ Erstellen**.

4. Wählen Sie auf dem Blatt **Add Microsoft Sentinel to a workspace** (Microsoft Sentinel zu einem Arbeitsbereich hinzufügen) den Log Analytics-Arbeitsbereich aus, den Sie im Lab „Azure Monitor“ erstellt haben, und klicken Sie auf **Hinzufügen**.

    >**Hinweis**: Microsoft Sentinel hat sehr spezifische Anforderungen an Arbeitsbereiche. Beispielsweise können Arbeitsbereiche, die von Microsoft Defender für Cloud erstellt wurden, nicht verwendet werden. Weitere Informationen finden Sie unter [Schnellstart: Durchführen des Onboardings für Microsoft Sentinel](https://docs.microsoft.com/en-us/microsoft/sentinel/quickstart-onboard).
    
#### Aufgabe 2: Konfigurieren von Microsoft Sentinel zur die Verwendung des Azure-Aktivitätsdatenconnectors 

In dieser Aufgabe konfigurieren Sie Sentinel für die Verwendung des Azure-Aktivitätsdatenconnectors.  

1. Klicken Sie im Azure-Portal auf dem Blatt **Microsoft Sentinel \| Übersicht** im Abschnitt **Inhaltsverwaltung** auf **Inhaltshub**.

2. Überprüfen Sie auf dem Blatt **Microsoft Sentinel \| Inhaltshub** die Liste der verfügbaren Inhalte.

3. Geben Sie **Azure** in die Suchleiste ein, und wählen Sie den Eintrag **Azure-Aktivität** aus. Lesen Sie die Beschreibung ganz rechts, und klicken Sie dann auf **Installieren**.

4. Warten Sie, bis die Benachrichtigung **Installation erfolgreich** angezeigt wird. Klicken Sie im linken Navigationspanel im Abschnitt **Konfiguration** auf **Datenconnectors**.

5. Klicken Sie auf dem Blatt **Microsoft Sentinel \| Datenconnectors** auf **Aktualisieren**, und sehen Sie sich die Liste der verfügbaren Connectors an. Wählen Sie den Eintrag für den Connector **Azure-Aktivität** aus (linke Menüleiste kann bei Bedarf mit \<< ausgeblendet werden), sehen Sie sich die Beschreibung und den Status ganz rechts an, und klicken Sie dann auf **Connectorseite öffnen**.

6. Auf dem Blatt **Azure-Aktivität** sollte die Registerkarte **Anweisungen** ausgewählt sein. Notieren Sie sich die **Voraussetzungen**, und scrollen Sie nach unten zu **Konfiguration**. Notieren Sie sich die Informationen, mit denen das Connectorupdate beschrieben wird. Weil Ihr Azure Pass-Abonnement die Legacy-Verbindungsmethode nie verwendet hat, können Sie Schritt 1 überspringen (die Schaltfläche **Alle trennen** ist abgeblendet) und mit Schritt 2 weitermachen.

7. Lesen Sie in Schritt 2 **Verbinden Sie Ihre Abonnements über die Diagnoseeinstellungen „neue Pipeline“** die Anleitungen unter „Starten Sie den Azure Policy-Zuweisungs-Assistenten, und führen Sie diese Schritte aus“. Klicken Sie dann auf **Azure Policy-Zuweisungs-Assistent starten\>** .

8. Klicken Sie auf der Registerkarte **Azure-Aktivitätsprotokolle zum Streaming an den angegebenen Log Analytics-Arbeitsbereich konfigurieren** (Seite „Richtlinie zuweisen“) **Grundlagen** auf die Schaltfläche mit den Auslassungspunkten **Bereich (...)** . Wählen Sie auf der Seite **Bereich** in der Dropdownliste „Abonnement“ Ihr Azure Pass-Abonnement aus, und klicken Sie unten auf der Seite auf die Schaltfläche **Auswählen**.

    >**Hinweis**:Wählen Sie *keine* Ressourcengruppe aus.

9. Klicken Sie am unteren Rand der Registerkarte **Grundlagen** zweimal auf die Schaltfläche **Weiter**, um zur Registerkarte **Parameter** zu wechseln. Klicken Sie auf der Registerkarte **Parameter** auf die Schaltfläche **Primärer Log Analytics-Arbeitsbereich Auslassungspunkte (...)**. Vergewissern Sie sich auf der Seite **Primärer Log Analytics-Arbeitsbereich**, dass Ihr Azure Pass-Abonnement ausgewählt ist, und wählen Sie im Dropdown-Listenfeld **Arbeitsbereiche** den Log Analytics-Arbeitsbereich aus, den Sie für Sentinel verwenden. Klicken Sie anschließend unten auf der Seite auf die Schaltfläche **Auswählen**.

10. Klicken Sie unten auf der Registerkarte **Parameter** auf die Schaltfläche **Weiter**, um zur Registerkarte **Wartung** zu wechseln. Aktivieren Sie auf der Registerkarte **Wartung** das Kontrollkästchen **Wartungstask erstellen**. Dadurch wird in der Dropdownliste **Zu korrigierende Richtlinie** die Option „Azure-Aktivitätsprotokolle zum Streaming an den angegebenen Log Analytics-Arbeitsbereich konfigurieren“ aktiviert. Wählen Sie in der Dropdownliste **Standort der systemseitig zugewiesenen Identität** die Region (z. B. „USA, Osten“) aus, die Sie zuvor für Ihren Log Analytics-Arbeitsbereich ausgewählt haben.

11. Klicken Sie unten auf der Registerkarte **Wartung** auf die Schaltfläche **Weiter**, um zur Registerkarte **Nichtkonformitätsmeldung** zu wechseln. Geben Sie bei Bedarf eine Nichtkonformitätsmeldung ein (dies ist optional), und klicken Sie unten auf der Registerkarte **Nichtkonformitätsmeldung** auf die Schaltfläche **Überprüfen + erstellen**.

12. Klicken Sie auf die Schaltfläche **Erstellen** . Sie sollten drei Meldungen zum Status „erfolgreich“ beobachten: **„Richtlinienzuweisung erfolgreich erstellt“, „Rollenzuweisungen erfolgreich erstellt“ und „Wartungstask erfolgreich erstellt“** .

    >**Hinweis**: Sie können das Glockensymbol „Benachrichtigungen“ überprüfen, um die drei erfolgreichen Aufgaben zu überprüfen.

13. Vergewissern Sie sich, dass im Bereich **Azure-Aktivität** der Graph **Empfangene Daten** angezeigt wird (möglicherweise müssen Sie die Browserseite aktualisieren).  

    >**Hinweis**: Es kann länger als 15 Minuten dauern, bis der Status „Verbunden“ lautet und der Graph „Empfangene Daten“ anzeigt.

#### Aufgabe 3: Erstellen einer Regel, die den Azure-Aktivitätsdatenconnector verwendet 

In dieser Aufgabe überprüfen und erstellen Sie eine Regel, die den Azure-Aktivitätsdatenconnector verwendet. 

1. Klicken Sie auf dem Blatt **Microsoft Sentinel \| Konfiguration** auf **Analysen**. 

2. Klicken Sie auf dem Blatt **Microsoft Sentinel \| Analysen** auf die Registerkarte **Regelvorlagen**. 

    >**Hinweis**: Überprüfen Sie die Typen von Regeln, die Sie erstellen können. Jede Regel ist einer bestimmten Datenquelle zugeordnet.

3. Geben Sie in der Liste der Regelvorlagen in das Suchleistenformular den Begriff **Suspicious** (Verdächtig) ein, und klicken Sie auf den der Datenquelle **Azure-Aktivität** zugeordneten Eintrag **Suspicious number of resource creation or deployment** (Verdächtige Anzahl von Ressourcenerstellungen oder -bereitstellungen). Klicken Sie dann in dem Bereich, in dem die Eigenschaften der Regelvorlage angezeigt werden, auf **Regel erstellen** (scrollen Sie bei Bedarf auf der Seite nach rechts).

    >**Hinweis**: Diese Regel ist für den mittleren Schweregrad vorgesehen. 

4. Übernehmen Sie auf der Registerkarte **Allgemein** des Blatts **Analyseregelassistent – Neue geplante Regel erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Regellogik festlegen**.

5. Übernehmen Sie auf der Registerkarte **Regellogik festlegen** des Blatts **Analyseregelassistent – Neue geplante Regel erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Vorfallseinstellungen (Vorschau) >**.

6. Übernehmen Sie auf der Registerkarte **Vorfallseinstellungen** des Blatts **Analyseregelassistent – Neue geplante Regel erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Automatisierte Antwort >**. 

    >**Hinweis**: Hier können Sie einer Regel ein Playbook hinzufügen, das als Logik-App implementiert wird, um die Behebung eines Problems zu automatisieren.

7. Übernehmen Sie auf der Registerkarte **Automatisierte Antwort** des Blatts **Analyseregelassistent – Neue geplante Regel erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Überprüfen und Erstellen >**. 

8. Klicken Sie auf der Registerkarte **Überprüfen und erstellen** des Blatts **Analyseregel-Assistent – Neue geplante Regel erstellen** auf **Speichern**.

    >**Hinweis**: Sie haben jetzt eine aktive Regel.

#### Aufgabe 4: Erstellen eines Playbooks

In dieser Aufgabe erstellen Sie ein Playbook. Ein Sicherheitsplaybook ist eine Sammlung von Aufgaben, die von Microsoft Sentinel als Reaktion auf eine Warnung aufgerufen werden können. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Text **Benutzerdefinierte Vorlage bereitstellen** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf die Option **Eigene Vorlage im Editor erstellen**.

3. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Datei laden**, navigieren Sie zur Datei **\\Allfiles\\Labs\\15\\changeincidentseverity.json**, und klicken Sie auf **Öffnen**.

    >**Hinweis**: Beispielplaybooks finden Sie unter [https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks).

4. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Speichern**.

5. Stellen Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** sicher, dass die folgenden Einstellungen konfiguriert sind (übernehmen Sie für alle anderen Einstellungen die Standardwerte):

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
    |Resource group|**AZ500LAB131415**|
    |Standort|**(USA) USA, Osten**|
    |Name des Playbooks|**Change-Incident-Severity** (Schweregrad des Incidents ändern)|
    |Benutzername|Ihre E-Mail-Adresse|

6. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist.

7. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Ressourcengruppen** ein, und drücken Sie die **EINGABETASTE**.

8. Klicken Sie auf dem Blatt **Ressourcengruppen** in der Liste der Ressourcengruppen auf den Eintrag **AZ500LAB131415**.

9. Klicken Sie auf dem Ressourcengruppenblatt **AZ500LAB131415** in der Ressourcenliste auf den Eintrag für die neu erstellte Logik-App **Change-Incident-Severity**.

10. Klicken Sie auf dem Blatt **Change-Incident-Severity** auf **Bearbeiten**.

    >**Hinweis:** Auf dem Blatt **Logic Apps-Designer** wird für jede der vier Regeln eine Warnung angezeigt. Dies bedeutet, dass jede Verbindung überprüft und konfiguriert werden muss.

11. Klicken Sie auf dem Blatt **Logic Apps-Designer** auf den ersten **Verbindungsschritt**.

12. Klicken Sie auf **Neu hinzufügen**, stellen Sie sicher, dass der Eintrag in der Dropdownliste **Mandant** Ihren Azure AD-Mandantennamen enthält, und klicken Sie auf **Anmelden**.

13. Melden Sie sich bei der entsprechenden Aufforderung mit dem Benutzerkonto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab nutzen.

14. Klicken Sie auf den zweiten **Verbindungsschritt**, und wählen Sie in der Liste der Verbindungen den zweiten Eintrag für die Verbindung aus, die Sie im vorherigen Schritt erstellt haben.

15. Wiederholen Sie die vorherigen Schritte für die beiden verbleibenden **Verbindungsschritte**.

    >**Hinweis**: Stellen Sie sicher, dass bei keinem der Schritte Warnungen angezeigt werden.

16. Klicken Sie auf dem Blatt **Logic Apps-Designer** auf **Speichern**, um Ihre Änderungen zu speichern.

#### Aufgabe 5: Erstellen einer benutzerdefinierten Warnung und Konfigurieren eines Playbooks als automatisierte Antwort

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Sentinel \| Übersicht**.

2. Klicken Sie auf dem Blatt **Microsoft Sentinel \| Übersicht** im Abschnitt **Konfiguration** auf **Analysen**.

3. Klicken Sie auf dem Blatt **Microsoft Sentinel \| Analysen** auf **+ Erstellen** und dann im Dropdownmenü auf **Geplante Abfrageregel**. 

4. Geben Sie auf der Registerkarte **Allgemein** des Blatts **Analyseregel-Assistent – Neue Regel geplante erstellen** die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte):

    |Einstellung|Wert|
    |---|---|
    |Name|**Playbook-Demo**|
    |Taktik|**Erstzugriff**|

5. Klicken Sie auf **Weiter: Regellogik festlegen >** .

6. Fügen Sie auf der Registerkarte **Regellogik festlegen** des Blatts **Analyseregel-Assistent – Neue geplante Regel erstellen** im Textfeld **Regelabfrage** die folgende Regelabfrage ein. 

    ```
    AzureActivity
     | where ResourceProviderValue =~ "Microsoft.Security" 
     | where OperationNameValue =~ "Microsoft.Security/locations/jitNetworkAccessPolicies/delete" 
    ```

    >**Hinweis:** Diese Regel identifiziert die Entfernung von Just-in-Time-VM-Zugriffsrichtlinien.

    >**Hinweis**: Wenn ein Analysefehler angezeigt wird, hat IntelliSense Ihrer Abfrage möglicherweise Werte hinzugefügt. Vergewissern Sie sich, dass die Abfrage übereinstimmt. Fügen Sie andernfalls die Abfrage in Editor und dann aus Editor in die Regelabfrage ein. 


7. Legen Sie auf der Registerkarte **Regellogik festlegen** des Blatts **Analyseregel-Assistent – Neue geplante Regel erstellen** im Abschnitt **Abfrageplanung** den Wert für **Abfrage ausführen alle** auf **5 Minuten** fest.

8. Übernehmen Sie auf der Registerkarte **Regellogik festlegen** des Blatts **Analyseregelassistent – Neue geplante Regel erstellen** die Standardwerte der verbleibenden Einstellungen, und klicken Sie auf **Weiter: Vorfallseinstellungen >**.

9. Übernehmen Sie auf der Registerkarte **Vorfallseinstellungen** des Blatts **Analyseregelassistent – Neue geplante Regel erstellen** die Standardeinstellungen, und klicken Sie auf **Weiter: Automatisierte Antwort >**. 

10. Klicken Sie auf der Registerkarte **Automatisierte Antwort** des Blatts **Analyseregel-Assistent – Neue geplante Regel erstellen** unter **Automatisierungsregeln**auf **+ Neue hinzufügen**.

11. Geben Sie im Fenster **Neue Automatisierungsregel erstellen** den Namen **Run Change-Severity Playbook** für die **Automatisierungsregel** ein. Klicken Sie dann im Feld **Trigger** auf das Dropdownmenü, und wählen Sie **Wenn eine Warnung erstellt wird** aus.

12. Lesen Sie im Fenster **Neue Automatisierungsregel erstellen** unter **Aktionen** den Hinweis, und klicken Sie dann auf **Playbookberechtigungen verwalten**. Aktivieren Sie im Fenster **Berechtigungen verwalten** das Kontrollkästchen neben der zuvor erstellten Ressourcengruppe **AZ500LAB1314151**, und klicken Sie dann auf **Übernehmen**.

13.  Klicken Sie im Fenster **Neue Automatisierungsregel erstellen** unter **Aktionen** auf das zweite Dropdownmenü, und wählen Sie die Logik-App **Change-Incident-Severity** aus. Klicken Sie im Fenster **Neue Automatisierungsregel erstellen** auf **Übernehmen**.

14. Klicken Sie auf der Registerkarte **Automatisierte Antwort** des Blatts **Analyseregelassistent – Neue geplante Regel erstellen** auf **Weiter: Überprüfen und erstellen >**, und klicken Sie auf **Speichern**

    >**Hinweis**: Sie haben jetzt die neue aktive Regel **Playbook-Demo**. Wenn ein von der Regellogik des identifiziertes Ereignis eintritt, führt dies zu einer Warnung mit mittlerem Schweregrad, die einen entsprechenden Incident generiert.

#### Aufgabe 6: Aufrufen eines Incidents und Überprüfen der zugeordneten Aktionen

1. Navigieren Sie im Azure-Portal zum Blatt **Microsoft Defender für Cloud \| Übersicht**.

    >**Hinweis**: Überprüfen Sie Ihre Sicherheitsbewertung. Jetzt sollte sie aktualisiert worden sein. 

2. Klicken Sie auf dem Blatt **Microsoft Defender for Cloud \| Übersicht** im linken Navigationsbereich unter **Cloudsicherheit** auf **Workloadschutz**.

3. Scrollen Sie auf dem Blatt **Microsoft Defender for Cloud \| Workloadschutz** nach unten, und klicken Sie unter **Erweiterter Schutz** auf die Kachel **Just-In-Time-VM-Zugriff**.

4. Klicken Sie auf dem Blatt **Just-in-Time-VM-Zugriff** auf der rechten Seite der Zeile für die VM **myVM** auf die Schaltfläche mit den **Auslassungspunkten (...)**, anschließend auf **Entfernen** und dann auf **Ja**.

    >**Hinweis:** Wenn die VM nicht in den **Just-in-Time-VMs** aufgeführt ist, navigieren Sie zum Blatt **Virtuelle Maschine**, und klicken Sie auf **Konfiguration**. Klicken Sie unter **Zugriff auf Just-in-Time-VMs** auf die Option **Just-in-Time-VMs aktivieren**. Wiederholen Sie den obigen Schritt, um zurück zu **Microsoft Defender für Cloud** zu navigieren. Aktualisieren Sie die Seite, um die VM anzuzeigen.

5. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Aktivitätsprotokoll** ein, und drücken Sie die **EINGABETASTE**.

6. Navigieren Sie zum Blatt **Aktivitätsprotokoll**, und notieren Sie sich den Eintrag **JIT-Netzwerkzugriffsrichtlinien löschen**. 

    >**Hinweis:** Es kann einige Minuten dauern, bis dies angezeigt wird. **Aktualisieren** Sie die Seite, wenn sie nicht angezeigt werden.

7. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Sentinel \| Übersicht**.

8. Überprüfen Sie auf dem Blatt **Microsoft Sentinel \| Übersicht** das Dashboard, und vergewissern Sie sich, dass dort ein Incident bezüglich des Löschens der Just-in-Time-VM-Zugriffsrichtlinie angezeigt wird.

    >**Hinweis**: Es kann bis zu fünf Minuten dauern, bis Warnungen auf dem Blatt **Microsoft Sentinel \| Übersicht** angezeigt werden. Wenn zu diesem Zeitpunkt keine Warnung angezeigt wird, führen Sie die Abfrageregel aus, auf die in der vorherigen Aufgabe verwiesen wurde. Überprüfen Sie so, ob die Löschaktivität der Just-In-Time-Zugriffsrichtlinie an den Log Analytics-Arbeitsbereich weitergegeben wurde, der Ihrer Microsoft Sentinel-Instanz zugeordnet ist. Wenn dies nicht zutrifft, erstellen Sie die Just-in-Time-VM-Zugriffsrichtlinie neu, und löschen Sie sie wieder.

9. Klicken Sie auf dem Blatt **Microsoft Sentinel \| Übersicht** im Abschnitt **Bedrohungsverwaltung** auf **Incidents**.

10. Vergewissern Sie sich, dass auf dem Blatt ein Incident mit mittlerem oder hohem Schweregrad angezeigt wird.

    >**Hinweis**: Es kann bis zu fünf Minuten dauern, bis der Incident auf dem Blatt **Microsoft Sentinel \| Incidents** angezeigt wird. 

    >**Hinweis**: Überprüfen Sie das Blatt **Microsoft Sentinel \| Playbooks**. Dort finden Sie die Anzahl der erfolgreichen und fehlgeschlagenen Ausführungen.

    >**Hinweis**: Sie haben die Möglichkeit, einem Incident einen anderen Schweregrad und Status zuzuweisen.

> Ergebnisse: Sie haben einen Microsoft Sentinel-Arbeitsbereich erstellt, ihn mit Azure-Aktivitätsprotokollen verbunden, ein Playbook und benutzerdefinierte Warnungen erstellt, die als Reaktion auf das Entfernen von Just-in-Time-VM-Zugriffsrichtlinien ausgelöst werden, und Sie haben überprüft, ob die Konfiguration gültig ist.

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. 

1. Öffnen Sie im Azure-Portal Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Klicken Sie bei der entsprechenden Aufforderung auf **PowerShell** und dann auf **Speicher erstellen**.

2. Stellen Sie sicher, dass oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

3. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppe zu entfernen, die Sie in diesem Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB131415" -Force -AsJob
    ```
4. Schließen Sie den **Cloud Shell**-Bereich.
