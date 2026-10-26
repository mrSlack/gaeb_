# GAEB Diff & Exchange — Technisches Lastenheft für das MVP

**Version:** 0.2, 26.10.2026  
**Produkt:** Eigenständige Webplattform zum Austausch von GAEB-Dokumenten und zum Vergleich verschiedener Fassungen von Leistungsverzeichnissen (LV), Angeboten und Auftrags-LV.  
**Primäre Zielgruppe:** Bauunternehmen, Nachunternehmer und Kalkulatoren, die mit Auftraggebern sowie AVA-Systemen wie RIB iTWO zusammenarbeiten.  
**Status:** Konzeptionelles Lastenheft. Vor der produktiven Freigabe sind reale Testdateien sowie Import-/Exportvorgänge mit den vorgesehenen Zielsystemen zu überprüfen.

## 1. Ziele und Abgrenzung

### 1.1. Ziele

- Import der GAEB-DA-XML-3.3-Austauschphasen X83, X84 und X86. Bei X84 ist die Zuordnung zu einem zugrunde liegenden LV erforderlich; ohne diese Zuordnung wird der eingeschränkte Vergleichsumfang ausdrücklich angezeigt.
- Erstellung eines Angebots auf Grundlage einer X83-Datei, Eingabe von Einheitspreisen (EP) sowie korrekter Export als X84.
- Vergleich zweier Leistungsverzeichnisse sowie Drei-Wege-Vergleich aus `ursprünglichem X83` + `eigenem X84` + `neuem X83/X86`.
- Verständliche Visualisierung von Änderungen an LV-Struktur, Positionen, Mengen, Texten, Mengeneinheiten, Preisen, Zu-/Abschlägen und deren wirtschaftlichen Auswirkungen.
- Lückenlose Versionierung ohne Überschreiben importierter Originaldateien, Erzeugung von Änderungsberichten und Dokumentation der fachlichen Prüfung.
- Übernahme eigener Einheitspreise in eine neue LV-Fassung mit ausdrücklicher Bestätigung bei risikobehafteten Änderungen.

### 1.2. Nicht Bestandteil des ersten Releases

- Vollständige Rechnungsbearbeitung mit X89/X89B und XRechnung.
- Aufmaß-/Mengenermittlung über X31, vollständiges Nachtragsmanagement, Zahlungsabwicklung und ERP-Anbindung.
- GAEB DA XML 3.2, ältere D-/P-Formate, die Beta-Version GAEB 3.4, direkte API-Anbindung an RIB iTWO sowie eine native mobile App.
- Automatische rechtliche Bewertung von Nachtragsansprüchen oder eigenständige Änderung vertraglicher Vereinbarungen.

### 1.3. Technische Standards

- **GAEB DA XML 3.3** (Stand 2023-01). Für den LV-Datenaustausch X83/X84/X86 ist der zugehörige Schema-Stand 2021-05 maßgeblich; die Änderung 2023-01 betrifft X31.
- **GAEB DA XML 3.4** (Stand 2026-03) liegt als Beta vor und dient nicht als Ausgangsbasis des MVP.
- **BVBS-Prüfkriterien und Testdateien für GAEB XML 3.3** sowie zusätzliche Round-Trip-Tests mit der konkret eingesetzten RIB-iTWO-Version.

Offizielle Referenzen:

- https://www.gaeb.de/de/service/downloads/gaeb-datenaustausch/
- https://www.gaeb.de/de/produkte/gaeb-datenaustausch/versionen/gaeb-da-xml-version-3-3-stand-2023-01/
- https://www.bvbs.de/wp-content/uploads/2024/05/Pruefkriterien-GAEB-DA-XML-3.3-Bauausfuehrung-V-04-04-2024.pdf

## 2. Zentrale Anwendungsfälle (User Stories)

**US-01 — GAEB-Import:** Ein Benutzer lädt eine X83-Datei hoch. Die Plattform erkennt Austauschphase und Version anhand des Dateiinhalts, validiert XML und XSD, erstellt ein Projekt samt unveränderlichem LV-Snapshot und zeigt die Gliederung sowie die Positionen an.

**US-02 — Angebotserstellung:** Aus einer importierten X83-Datei wird ein Preisspiegel bzw. ein eigener Preisdatensatz für das Angebot erstellt. Der Benutzer erfasst EP und erforderliche Bietertextergänzungen, kontrolliert die Summen und exportiert eine X84-Datei. OZ-Maske, Gliederung und Positionsmerkmale dürfen dadurch nicht unzulässig verändert werden.

**US-03 — Neue LV-Fassung:** Der Auftraggeber stellt X83 Version 2 bereit. Der Benutzer ordnet die Datei dem bestehenden Projekt zu; die Plattform erzeugt einen neuen LV-Snapshot, ohne Version 1 zu löschen.

**US-04 — Zwei-Wege-Vergleich LV ↔ LV:** X83 Version 1 und Version 2 werden verglichen. Die Ansicht zeigt hinzugefügte, entfernte, verschobene, umnummerierte und inhaltlich veränderte Positionen sowie Unterschiede bei Menge, Einheit, Kurztext, Langtext, Vorbemerkungen, Positionsmerkmalen und Anlagen.

**US-05 — Drei-Wege-Vergleich:** Ursprüngliches X83 Version 1 + eigenes X84 + neues X83 Version 2 bzw. X86 des Auftraggebers. Die Plattform trennt (a) Änderungen am Leistungsumfang, (b) Veränderungen von Einheitspreisen, soweit neue Preise tatsächlich übermittelt wurden, und (c) eine gesondert gekennzeichnete Prognose der neuen LV-Summe auf Basis bisheriger EP.

**US-06 — Preisübernahme:** Bei eindeutig zugeordneten, inhaltlich geeigneten Positionen können frühere EP übernommen werden. Haben sich Leistungstext, Einheit, Positionstyp oder die Zuordnungssicherheit verändert, ist eine ausdrückliche Bestätigung erforderlich. Neue Positionen erhalten zunächst keinen EP. Es wird eine neue Angebotsversion erzeugt; das ursprüngliche Angebot bleibt unverändert.

**US-07 — Änderungsprüfung:** Ein Sachbearbeiter kennzeichnet Änderungen als `ungeprüft`, `zur Kenntnis genommen` oder `Klärung erforderlich` und kann Kommentare hinterlegen. Er filtert nach Änderungstyp und Relevanz und exportiert einen Prüfbericht als PDF bzw. XLSX.

**US-08 — Unvollständige Unterlagen:** Eine X84-Datei ohne zugehöriges X83 darf gespeichert werden. Ein vollständiger Struktur- und Inhaltsvergleich wird jedoch erst angeboten, wenn das korrespondierende Basis-LV zugeordnet wurde.

## 3. Funktionale Anforderungen

| ID | Anforderung | Priorität |
|---|---|---|
| FR-01 | Registrierung, Organisationen, Benutzer und Rollen: Administrator, Bearbeiter, Leser | P0 |
| FR-02 | Projektverwaltung, Upload-Historie, unveränderte Originaldateien und Metadaten | P0 |
| FR-03 | Import von GAEB XML 3.3 X83 inklusive XSD- und fachlicher Validierung | P0 |
| FR-04 | LV-Hierarchie, OZ-Maske, Normal- und Sonderpositionen, Langtexte und Anlagen | P0 |
| FR-05 | Erfassung von EP, Bieterangaben/Bietertextergänzungen und Export als X84 3.3 | P0 |
| FR-06 | Import von X84 mit Zuordnung zum Basis-X83 und Import von X86 als Auftraggeber-/Vertragsdokument | P0 |
| FR-07 | Unveränderliche LV-Snapshots mit dokumentierter Herkunft und Versionskette | P0 |
| FR-08 | Positionszuordnung mit Konfidenzwert, Protokoll und manueller Klärung mehrdeutiger Fälle | P0 |
| FR-09 | Semantischer Feldvergleich sowie Text-Diff auf Absatz- und Wortebene | P0 |
| FR-10 | Zwei-Wege-Vergleich LV ↔ LV, Angebot ↔ Auftrag sowie Drei-Wege-Vergleich | P0 |
| FR-11 | Auswertung von ΔMenge, ΔEP, ΔGP, Prognose zu eigenen EP; Kennzeichnung unbekannter Beträge | P0 |
| FR-12 | Filter, Sortierung, Gegenüberstellung, Prüfstatus sowie Berichtsexport | P0 |
| FR-13 | Übernahme von EP in eine neue LV-Fassung und Erstellung eines neuen X84-Angebots | P0 |
| FR-14 | Audit-Log, projektspezifische Berechtigungen, Verschlüsselung und Datensicherung | P0 |
| FR-15 | Einladungen an Nachunternehmer und kollaborative Angebotsabgabe | P1 |
| FR-16 | X31-Import, X89/X89B, XRechnung und vollständige Rechnungsfunktionen | P1 / Folgeversion |
| FR-17 | GAEB 3.2 und weitere ältere Dateiformate | P2 |

## 4. Benutzeroberfläche: „Vergleich“

1. **Kopfbereich:** Projekt, Dateinamen, Upload-Zeitpunkte, Prüfstatus und Vergleichsmodus (`LV ↔ LV`, `Angebot ↔ Auftrag`, `3-Wege`).
2. **Zusammenfassung:** Anzahl hinzugefügter, entfernter, veränderter, umnummerierter und nicht eindeutig zugeordneter Positionen; Auswirkungen auf Mengen, EP, GP und Anzahl ungeklärter Abweichungen.
3. **Filterleiste:** Änderungstyp, LV-Abschnitt, OZ-Bereich, monetäre Relevanz, Freigabe-/Prüfstatus, nur Textänderungen.
4. **Zentrale Tabelle:** Fixierte Spalten `OZ` und `Kurztext`; für jede Position `vorher`, `nachher` und `Differenz` für Menge, ME, EP und GP nebeneinander.
5. **LV-Baum links:** Gliederung mit Anzahl der Änderungen je Abschnitt.
6. **Detailbereich rechts:** Langtext-Diff nach Absätzen/Wörtern, Anlagen, Datenherkunft, Berechnungsformel und Kommentare.
7. **Aktionen:** „Zuordnung bestätigen“, „Nicht zuordnen“, „EP übernehmen“, „Rückfrage markieren“, „Änderungsbericht exportieren“.
8. **Statusanzeige:** Hinzugefügt, entfernt, geändert, unverändert, Zuordnung unsicher. Jede Farbe muss durch ein Textlabel oder Symbol ergänzt werden.
9. **Originaldateien:** Direkter Download für eine unabhängige Prüfung außerhalb der Plattform.

**Grundsatz:** Die visuelle Diff-Ansicht und die PDF-/XLSX-Berichte sind plattformeigene Darstellungen und keine zusätzlichen GAEB-Austauschphasen.

## 5. Regeln für den Vergleich (Diff Engine)

### 5.1. Datenaufbereitung und Normalisierung

- XML-Dateien werden sicher verarbeitet: DTD und XXE sind deaktiviert; Anlagen und formatierte Langtexte werden kontrolliert verarbeitet.
- Die GAEB-Austauschphase wird anhand der Dokumentstruktur ermittelt, nicht allein anhand der Dateiendung.
- Daten werden logisch in einen **strukturellen LV-Snapshot** und separate **Preisdatensätze (Price Sheets)** aufgeteilt.
- Preise aus X84 werden dem zugrunde liegenden LV zugeordnet. Enthält ein neues X83 keine EP, bleiben diese Werte `null`; ein fehlender Preis wird niemals automatisch zu `0`.
- Ursprungsdatei, nicht abgebildete Felder und Anlagen bleiben für Nachvollziehbarkeit sowie spätere Parser-Erweiterungen verfügbar.
- Die semantische Normalisierung darf keine rechtlich oder fachlich relevanten Textbestandteile und Strukturen verlieren.

### 5.2. Zuordnung von Positionen (Matching)

Reihenfolge bei der Kandidatensuche:

1. **GUID**, falls vorhanden und die gemeinsame Dokumentherkunft plausibel bestätigt ist.
2. **OZ plus LV-Hierarchie und Positionstyp**.
3. **Textähnlichkeit, Mengeneinheit, benachbarte Positionen und Abschnittskontext** als zusätzliche Merkmale.
4. **Manuelle Prüfung**, wenn Positionen geteilt, zusammengeführt, mehrfach nummeriert oder nicht sicher zuordenbar sind.

Automatisches Matching ist nur bei hoher Zuordnungssicherheit zulässig. Eine unveränderte OZ mit erheblich verändertem Inhalt wird als verdächtig markiert. Bei mehreren möglichen Kandidaten darf die Software keine willkürliche Zuordnung wählen.

### 5.3. Zu vergleichende Felder

Soweit in der jeweiligen GAEB-Austauschphase vorhanden:

`oz`, `parent/section`, `order`, `node_type`, `position_type`, `short_text`, `long_text`, `quantity`, `unit`, `unit_price`, `line_total`, `percentage_markup`, `discount`, `bidder_completions`, `attachments`, `notes`, `alternative/basic/dependent links`, `currency`, `lv_headers/preliminaries`.

Die Fachlogik muss die folgenden Änderungen auseinanderhalten: neue Position, entfernte Position, geänderte Menge, geänderter EP, geänderte ME, Textänderung, andere Positionsart, Umnummerierung, Verschiebung und Änderung übergeordneter Vorbemerkungen.

### 5.4. Finanzielle Auswertung

Für eine reguläre Position gilt unter Beachtung der festgelegten Rundungsregeln:

`GP = Menge × EP`

Die Gesamtdifferenz wird rechnerisch in zwei Beiträge aufgeteilt:

- **Mengeneffekt:** `(M2 − M1) × EP1`
- **Preiseffekt:** `M2 × (EP2 − EP1)`
- **Gesamteffekt:** Summe aus Mengen- und Preiseffekt, vor Rundung.

**Beispiel:** Ursprünglich `100 m² × 45,00 EUR = 4.500,00 EUR`; neu `120 m² × 42,00 EUR = 5.040,00 EUR`. Die Differenz beträgt **+540,00 EUR**, zusammengesetzt aus **+900,00 EUR Mengeneffekt** und **−360,00 EUR Preiseffekt**.

Für besondere Positionsarten (etwa Bedarfspositionen, Alternativpositionen, Grundpositionen, Zuschlagspositionen oder Pauschalpositionen) gelten spezifische Berechnungs- und Summierungsregeln. Wenn eine erforderliche Strategie noch nicht implementiert ist, wird „Berechnung fachlich zu prüfen“ angezeigt. Solche Werte dürfen keine vermeintlich verlässliche Gesamtsumme verfälschen.

### 5.5. Strikte Trennung finanzieller Werte

- **Original Bid / Ursprüngliches Angebot:** Tatsächliche Beträge der ursprünglich abgegebenen X84.
- **Repriced New Scope / Prognose nach bisherigen EP:** Neu ermittelter LV-Umfang, bewertet mit bestätigten bisherigen eigenen EP; ausdrücklich nur ein Kalkulationsszenario.
- **Customer Price / Auftraggeberpreis:** Tatsächlich im neuen X86 oder einem anderen geeigneten Dokument vorhandene Auftraggeberpreise.
- **Unknown / Nicht bekannt:** Die Angabe fehlt. Unbekannt ist nicht gleich null.

Die Plattform darf prognostizierte eigene Preise niemals als übermittelte bzw. akzeptierte Auftraggeberpreise darstellen.

### 5.6. Preisübernahme und Änderungsregeln

- Eine Änderung eines Merkmals bedeutet nicht, dass weitere Merkmale stillschweigend akzeptiert wurden.
- Bei geändertem Langtext, Positionstyp oder Mengeneinheit ist eine ausdrückliche Freigabe vor Übernahme des EP erforderlich.
- Das neue Angebot entsteht aus dem **neuen Basis-X83**. Das ursprüngliche X83 darf nicht durch unzulässige strukturelle Änderungen innerhalb einer X84 ersetzt werden.
- Originale, frühere Angebote und bereits abgeschlossene Prüfergebnisse werden nicht überschrieben.

## 6. Datenmodell und Beziehungen

Die erste Implementierungsskizze liegt in der separaten Datei `gaeb_diff_initial_schema.sql`. Die konzeptionellen Entitäten lauten:

| Tabelle | Bedeutung und wichtige Attribute |
|---|---|
| `organizations`, `users`, `organization_memberships` | Mandantentrennung, Benutzer und Rollen |
| `projects` | Bauvorhaben, Auftraggeber und Projektkontext |
| `gaeb_documents` | Original-Upload, SHA-256, GAEB-Phase/-Version/-Stand, Storage-Key, Validierungsbericht |
| `lv_snapshots` | Unveränderliche LV-Fassung, Quelldokument und Vorgängerversion |
| `lv_nodes` | Titel, Abschnitte und Positionen mit Parent-Beziehung, OZ, GUID, Texten, Menge, ME und Positionsart |
| `price_sheets` | Preisstände: eigenes Angebot, übermittelte Auftraggeberpreise, modellierte Prognose |
| `price_entries` | EP, GP, Rabatte, Bietertextergänzungen und weitere Preisinformationen je Position |
| `comparisons` | Vergleichsmodus, Basisfassung, linkes/rechtes LV, zugeordnete Preisstände, Regelversion |
| `comparison_matches` | Positionspaare, Zuordnungsmethode, Konfidenz und Freigabestatus |
| `comparison_changes` | Feldänderungen, alte/neue Werte, Änderungstyp, finanzieller Effekt und Prüfbedarf |
| `review_comments`, `audit_events` | Entscheidungen, Kommentare und revisionsfähige Benutzeraktionen |

**Erforderliche Indizes:** `(snapshot_id, oz)`, `(snapshot_id, source_guid)`, `(comparison_id, match_status)`, `(comparison_id, change_type)` und SHA-256 je Dokument.

Jede fachliche Abfrage und Schreiboperation muss mandanten- und projektbezogen autorisiert sein (`organization_id`, `project_id`). Für unveränderliche Snapshots sind Fremdschlüssel, eindeutige Versionsbezüge und Schutz vor nachträglichen Änderungen vorzusehen.

## 7. Softwarearchitektur

**Frontend:** React, TypeScript, TanStack Table mit virtualisierten Zeilen, Anzeige von Rich-Text-Unterschieden, OIDC-Authentifizierung.  
**Backend:** FastAPI, Pydantic, PostgreSQL, SQLAlchemy und Alembic für Datenbankmigrationen.  
**GAEB-Komponente:** Versionierte XSD-Dateien, `lxml` mit deaktiviertem DTD/XXE, phasenspezifische Import-/Exportadapter, deterministisches internes LV-Modell.  
**Diff-Komponente:** Reproduzierbare Vergleichsregeln, Zuordnungsvorschläge, Prüfung unklarer Treffer und getrennte Berechnungsstrategien für Positionsarten.  
**Job-Verarbeitung:** Redis mit RQ oder Celery für serverseitige Import- und Vergleichsaufträge.  
**Dateiablage:** S3-kompatibler Objektspeicher, unveränderliche Originale, Backups.  
**Betrieb:** Hosting in der EU bzw. Deutschland, Container, TLS, Datensicherung sowie Protokollierung und Monitoring.

**Reproduzierbarkeit:** Für jeden Import und Vergleich sind `parser_version`, `schema_version` und `diff_rules_version` zu speichern. Eine Änderung des Vergleichsalgorithmus erzeugt einen neuen Comparison-Run; frühere Ergebnisse bleiben erhalten.

## 8. REST API v1 (Entwurf)

| Methode | Endpunkt | Funktion |
|---|---|---|
| POST | `/api/v1/projects` | Projekt anlegen |
| POST | `/api/v1/projects/{id}/documents` | GAEB-Datei hochladen |
| GET | `/api/v1/projects/{id}/documents` | Dateien und Verarbeitungsstatus abrufen |
| GET | `/api/v1/documents/{id}/validation` | XML-/XSD-Fehler und fachliche Warnungen anzeigen |
| GET | `/api/v1/snapshots/{id}/nodes` | LV-Baum und Positionen paginiert abrufen |
| POST | `/api/v1/price-sheets` | Preisdatensatz anlegen |
| PATCH | `/api/v1/price-sheets/{id}/entries/{nodeId}` | EP und Angebotsdaten aktualisieren |
| POST | `/api/v1/comparisons` | Zwei- oder Drei-Wege-Vergleich starten |
| GET | `/api/v1/comparisons/{id}` | Zusammenfassung, Status und Berechnungsergebnisse abrufen |
| GET | `/api/v1/comparisons/{id}/changes` | Änderungen filtern, sortieren und paginieren |
| PATCH | `/api/v1/comparisons/{id}/matches/{id}` | Positionszuordnung bestätigen oder verwerfen |
| POST | `/api/v1/comparisons/{id}/carry-forward` | Neue Preisfassung mit bestätigter EP-Übernahme erstellen |
| POST | `/api/v1/price-sheets/{id}/exports/x84` | GAEB-X84-Datei exportieren |
| GET | `/api/v1/comparisons/{id}/report?format=xlsx` | Prüfbericht als XLSX/PDF abrufen |

Beispiel für den Request eines Drei-Wege-Vergleichs:

```json
{
  "mode": "THREE_WAY",
  "base_snapshot_id": "uuid-x83-v1",
  "left_snapshot_id": "uuid-x83-v1",
  "left_price_sheet_id": "uuid-own-x84",
  "right_snapshot_id": "uuid-new-x83-or-x86",
  "right_price_sheet_id": null,
  "projection": "REPRICE_WITH_CONFIRMED_LEFT_EP"
}
```

`right_price_sheet_id = null` bedeutet, dass keine neuen Auftraggeberpreise vorliegen. Die Auswertung darf in diesem Fall nur eine als solche markierte Prognose darstellen.

## 9. Nichtfunktionale Anforderungen

- **Umfang:** Bis zu 10.000 Positionen je LV als Zielprofil des MVP. Große Dateien werden als serverseitige Jobs verarbeitet; der Bearbeitungsfortschritt ist sichtbar.
- **Performance-Zielwerte:** Auf einer ausreichend dimensionierten Testumgebung mit 10.000 regulären Positionen: Import höchstens 30 Sekunden, Vergleich höchstens 20 Sekunden. Diese Werte sind durch Lasttests zu validieren und gelten nicht ohne Messung als zugesicherte Leistung.
- **Frontend:** Virtualisierte Tabellen, serverseitige Filter und Sortierung. Tausende umfangreiche Langtext-Blöcke dürfen nicht gleichzeitig in den Browser geladen werden.
- **Datenschutz und Zugriff:** Konfigurierbare 2FA-/SSO-Unterstützung, RBAC, Audit-Log, Verschlüsselung bei Übertragung und Speicherung, EU-Datenspeicherung sowie getestete Sicherungs- und Wiederherstellungsverfahren.
- **Dateisicherheit:** Größenlimits, Ressourcenbegrenzung des Parsers, Prüfung von MIME-Typ und GAEB-Phase, sichere XML-Verarbeitung, Virenprüfung sowie Schutz vor ZIP-/Dekompressionsbomben.
- **Nachvollziehbarkeit:** Original- und Exportdateien aufbewahren, kein stilles Überschreiben, SHA-256-basierte Erkennung wiederholter Uploads mit definiertem Idempotenzverhalten.
- **Fachliche Korrektheit:** Ein unbekannter Einheitspreis darf ohne ausdrückliche Benutzereingabe nie zu `0` werden.
- **Bedienbarkeit:** Benutzeroberfläche mindestens in Deutsch, optional Russisch; GAEB-Fachbegriffe bleiben eindeutig. Tastaturbedienbarkeit und ergänzende Textkennzeichnungen für Farbzustände sind erforderlich.

## 10. Abnahmekriterien und Tests

**AT-01 — Import:** Eine gültige X83 wird mit OZ-Maske, Überschriften, Positionen, Mengen/ME, Kurz-/Langtexten und Anlagen eingelesen. Ungültiges XML erzeugt eine verständliche Diagnose; die Originaldatei bleibt gesichert.

**AT-02 — BVBS-Sonderfälle:** Geeignete Testdateien prüfen insbesondere Normal-, Bedarfs-, Grund- und Alternativpositionen, OZ-Strukturen, Grafiken im Langtext, Grenzfälle bei Mengen, Zuschläge sowie Bietertextergänzungen. Nicht unterstützte Fälle sind explizit zurückzuweisen oder zu kennzeichnen, niemals stillschweigend zu verändern.

**AT-03 — X84-Round-Trip:** Die erzeugte X84 besteht die Prüfung im GAEBXML Checker und ist in die vorgesehene RIB-iTWO-Version importierbar. OZ-Maske, Hierarchie, Währung, Preise, Ergänzungen und unterstützte Sonderpositionen bleiben korrekt. Ein erfolgreicher Test ersetzt keine offizielle Zertifizierung.

**AT-04 — Mengenänderung:** V1 `100 m²`, V2 `120 m²`, EP `45,00 EUR`: Ergebnis ΔMenge `+20 m²` und prognostizierte Kostendifferenz `+900,00 EUR`.

**AT-05 — Mengen- und Preisänderung:** V1 `100 m² × 45,00 EUR`, V2 `120 m² × 42,00 EUR`: Gesamtdifferenz `+540,00 EUR`, davon Mengeneffekt `+900,00 EUR` und Preiseffekt `−360,00 EUR`.

**AT-06 — Reine Textänderung:** Eine Veränderung des Langtexts ohne Mengen- oder Preisänderung ist sichtbar. Es wird kein erfundener monetärer Effekt ausgegeben.

**AT-07 — Hinzugefügt/entfernt:** Neue und gelöschte Positionen werden gesondert gekennzeichnet. Die automatische Übernahme alter EP auf neue, nicht zugeordnete Positionen ist unzulässig.

**AT-08 — Umnummerierung:** Eine neue OZ bei sonst identischem Inhalt erzeugt den Kandidaten „umnummeriert“. Eine wiederverwendete OZ mit neuem Leistungsinhalt erfordert eine Prüfung. Es entstehen keine willkürlichen Doppelzuordnungen.

**AT-09 — Fehlende Auftraggeberpreise:** Ein neues X83 ohne EP führt nicht zu fiktiven „Auftraggeberpreisen“. Zulässig ist lediglich eine gesondert markierte Prognose auf Basis eigener EP.

**AT-10 — Versionssicherheit:** Nach der EP-Übernahme bleibt Angebot V1 unverändert; eine neue X84 wird auf Grundlage von X83 Version 2 erzeugt.

**AT-11 — Mandantentrennung:** Benutzer unterschiedlicher Organisationen können auch durch Manipulation von URLs oder IDs nicht auf fremde Dokumente, Kommentare oder Berechnungen zugreifen.

**AT-12 — Prüfbericht:** Der Änderungsbericht nennt Vergleichsbasis, Eingangsdateien und deren Hashwerte, Zeitstempel, Zuordnungsregeln, ungeklärte Fälle und Hinweise auf prognostizierte Beträge.

## 11. Entwicklungsphasen, Team und Terminrahmen

Unter der Annahme von **zwei Vollzeitentwicklern** (Backend/GAEB und Frontend), einem zeitweise verfügbaren GAEB-Fachexperten sowie QA-Kapazität beträgt die grobe Planungsannahme **12–16 Wochen bis zu einem pilotfähigen MVP**. Voraussetzung sind echte Beispieldateien und eine beherrschbare Komplexität beim GAEB-Schema-Mapping. Nach einem technischen Prototyp ist die Planung neu zu bewerten.

| Sprint | Inhalt | Definition of Done |
|---|---|---|
| 1 (Woche 1–2) | Reale Beispieldateien, XSD, Architektur, Datenmodell, X83-Import | X83 im Browser lesbar, Original unverändert gespeichert |
| 2 (Woche 3–4) | Positionen/Sondertypen, Preisverwaltung, Kalkulationsoberfläche | EP bearbeitbar, fachlich korrekte Berechnungen |
| 3 (Woche 5–6) | X84-Export, BVBS-Prüfungen, Rückimport | Round-Trip X83 → X84 → iTWO erfolgreich |
| 4 (Woche 7–8) | X86, Snapshots, Position Matching und Feld-Diff | Vergleich V1 ↔ V2 und X84 ↔ X86 möglich |
| 5 (Woche 9–10) | Drei-Wege-Diff, finanzielle Auswirkungen, UX | Echte Preise von Prognosen eindeutig unterscheidbar |
| 6 (Woche 11–12) | EP-Übernahme, Berichte, Kommentare | Neues X84 V2 und nachvollziehbares Änderungsprotokoll |
| 7 (Woche 13–16) | Kompatibilität, Sicherheit, QA, Pilotbetrieb | Abnahmetests mit realen Dateien erfolgreich |

Ein eingeschränkter interner Proof of Concept kann gegebenenfalls innerhalb von 3–4 Wochen realisiert werden. Er ist jedoch **noch kein nachgewiesen GAEB-kompatibles Produkt**, solange der X84-Import in die Zielversion von RIB iTWO nicht getestet wurde.

## 12. Risiken und offene Punkte

- Die tatsächliche Kompatibilität hängt von der eingesetzten RIB-iTWO-Version, den verwendeten Positionsarten und repräsentativen Beispieldateien ab.
- Eine X84 enthält möglicherweise nicht sämtliche Texte und Mengen; die Zuordnung zum ursprünglichen X83 ist daher essenziell.
- Komplexe Zuschläge, Bedarfs-/Alternativpositionen und Bietertextergänzungen benötigen eigene Round-Trip-Tests.
- GAEB XML 3.2 und 3.3 unterscheiden sich syntaktisch. Die Unterstützung älterer Versionen darf nicht auf den Austausch einer Versionsnummer reduziert werden.
- Bei aufgeteilten oder zusammengeführten Positionen und strukturellen Umbauten ist eine eindeutige automatische Zuordnung teilweise unmöglich; dafür ist ein manueller Prüfworkflow erforderlich.
- Ein PDF-Änderungsbericht ersetzt keine wirksam vereinbarten oder unterzeichneten Vertragsunterlagen.

## 13. Nächster technischer Schritt

Es werden **drei echte Dateien aus demselben Bauprojekt** benötigt:

1. Das ursprünglich vom Auftraggeber erhaltene **X83**.
2. Das darauf basierende **eigene X84-Angebot**.
3. Ein **geändertes X83** bzw. das vom Auftraggeber zurückgesendete **X86**.

Die Dateien können hinsichtlich Personen, Firmennamen und weiterer vertraulicher Angaben anonymisiert werden. Entscheidend ist, dass LV-Struktur, Positionsbezüge und typische fachliche Änderungen erhalten bleiben. Anhand dieser drei Dateien soll ein Import- und Diff-Prototyp die Zuordnungsgenauigkeit und den praktischen Nutzen des Produkts nachweisen.