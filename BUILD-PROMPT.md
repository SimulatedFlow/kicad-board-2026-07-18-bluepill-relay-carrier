```json
{
  "board_name": "BluePill-Relay-Carrier",
  "one_liner": "Robuste 2-Kanal-Relaisträgerplatine für die STM32 Bluepill mit Sensor-Schnittstelle und galvanischer Absicherung.",
  "market_gap": "Existierende Trägerplatinen sind oft überladene Alleskönner oder billige Steckbrett-Kopien. Dieses Board bietet Bastlern eine saubere, extrem robuste und handlötbare Hutschienen-Plattform für zwei Relais ohne Kabelsalat.",
  "confidence": "high",
  "price_eur": 14.90,
  "target_enclosure": "80x70mm DIN-Hutschienen-Gehäuse (z. B. CamdenBoss CNMB/3)",
  "injection_notes": "keine"
}
```

## BUILD-PROMPT

### Name & Ziel
**BluePill-Relay-Carrier** – Eine handlötbare Trägerplatine (Carrier Board) für das beliebte STM32F103-„Bluepill"-Entwicklungsboard, ausgelegt auf genau eine Kernfunktion: Sicheres, robustes Schalten von zwei Relais und das Auslesen eines externen Sensors im industrienahen oder Hausautomations-Bereich.

---

### 1. HARTE DFM-Vorgaben (Handlöt-Tauglichkeit)
- **Komplexitätsgrenze:** Maximal 19 Bauteile gesamt – extrem luftiges Layout für fehlerfreies Routing und einfaches Löten.
- **Technologie:** Reine THT-Bauweise (Through-Hole Technology). Keine SMD-Bauteile, um den Einstieg für Bastler so einfach wie möglich zu machen.
- **Leiterbahnen & Abstände:** 
  - Standard-Signalleitungen: Breite $\ge 0.4\,\text{mm}$, Clearance $\ge 0.3\,\text{mm}$.
  - Stromversorgung (5V/GND): Breite $\ge 1.0\,\text{mm}$.
  - Relais-Lastpfade: Breite $\ge 1.5\,\text{mm}$ (ausgelegt für bis zu 5A Laststrom).
- **Lagen-Stackup (4 Lagen):**
  - `F.Cu` (Top-Signal & High-Voltage Lastpfade)
  - `In1.Cu` (Durchgehende GND-Plane für hervorragende EMV und Rauschunterdrückung)
  - `In2.Cu` (Durchgehende 5V-Versorgungs-Plane)
  - `B.Cu` (Bottom-Signal & zusätzliche Relais-Leiterbahnen)

---

### 2. Mechanische Spezifikationen
- **Platinengröße:** Exakt $80.0\,\text{mm} \times 70.0\,\text{mm}$ (rechteckig, perfekt passend für Standard-Hutschienenhalterungen).
- **Befestigungsbohrungen:** 4× M3-Bohrungen (Durchmesser $3.2\,\text{mm}$) an den Koordinaten:
  - Loch 1: $(5.0\,\text{mm}, 5.0\,\text{mm})$
  - Loch 2: $(75.0\,\text{mm}, 5.0\,\text{mm})$
  - Loch 3: $(75.0\,\text{mm}, 65.0\,\text{mm})$
  - Loch 4: $(5.0\,\text{mm}, 65.0\,\text{mm})$
  - **Keepout:** $6.0\,\text{mm}$ Kreisdurchmesser um jedes Loch herum (keine Leiterbahnen oder Kupferflächen).

---

### 3. Stückliste (BOM) & KiCad-Footprints (19 Bauteile)

| RefDerig | Bauteil | KiCad-Symbol | KiCad-Footprint | Beschreibung |
| :--- | :--- | :--- | :--- | :--- |
| **U1, U2** | 2x 20-Pin Buchsenleiste | `Connector:Conn_01x20_Socket` | `Connector_PinSocket_2.54mm:PinSocket_1x20_P2.54mm_Vertical` | Sockel für das Bluepill-Board (Abstand der beiden Reihen zueinander: exakt **0.9″ / 22.86 mm**) |
| **RY1, RY2** | 2x Print-Relais (5V) | `Relay:Relay_SPDT` | `Relay_THT:Relay_SPDT_Sanyou_SRD_Series_Form_C` | Standard-Hobby-Relais (z. B. Songle SRD-05VDC-SL-C) |
| **Q1, Q2** | 2x NPN-Transistor | `Device:Q_NPN_EBC` | `Package_TO_SOT_THT:TO-92_Inline` | BC547 oder 2N3904 zur Ansteuerung der Relaisspulen |
| **D1, D2** | 2x Freilaufdiode | `Device:D` | `Diode_THT:D_DO-41_SOD81_P10.16mm_Horizontal` | 1N4007 Diode (Schutz vor Induktionsspannung) |
| **D3** | 1x Schottky-Diode | `Device:D_Schottky` | `Diode_THT:D_DO-41_SOD81_P10.16mm_Horizontal` | 1N5819 (Verpolungsschutz für 5V-Eingang) |
| **R1, R2** | 2x Widerstand 1kΩ | `Device:R` | `Resistor_THT:R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal` | Basis-Vorwiderstände für Q1 und Q2 |
| **R3, R4** | 2x Widerstand 330Ω | `Device:R` | `Resistor_THT:R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal` | Vorwiderstände für die Status-LEDs |
| **D4, D5** | 2x LED (3mm oder 5mm) | `Device:LED` | `LED_THT:LED_D3.0mm` | Rote/Grüne THT-LEDs zur Visualisierung des Relais-Schaltzustands |
| **C1** | 1x Elko $100\,\mu\text{F}$ | `Device:C_Polarized` | `CP_Radial_D6.3mm_P2.50mm` | Bulk-Kondensator zur Glättung der 5V-Schiene |
| **C2** | 1x Keramikkondensator $100\,\text{nF}$| `Device:C` | `C_Disc_D5.0mm_W2.5mm_P5.00mm` | Entkopplungskondensator |
| **J1** | 1x Schraubklemme 2-polig | `Connector:Screw_Terminal_01x02`| `Connector_TerminalBlock:TerminalBlock_Phoenix_PT-1.5_2-G-5.08_1x02_P5.08mm_Horizontal` | Stromeingang 5VDC (an der linken Boardkante platziert) |
| **J2, J3** | 2x Schraubklemme 3-polig | `Connector:Screw_Terminal_01x03`| `Connector_TerminalBlock:TerminalBlock_Phoenix_PT-1.5_3-G-5.08_1x03_P5.08mm_Horizontal` | Relaisausgänge (COM, NO, NC) (an der rechten Boardkante platziert) |
| **J4** | 1x Schraubklemme 4-polig | `Connector:Screw_Terminal_01x04`| `Connector_TerminalBlock:TerminalBlock_Phoenix_PT-1.5_4-G-5.08_1x04_P5.08mm_Horizontal` | Sensor-Schnittstelle (5V, GND, I2C: SDA/SCL oder GPIO) (an der unteren Kante) |

---

### 4. Elektrische Verbindungen & Logik (Netzliste)
- **Power-Input:**
  - `J1` Pin 1 (VCC_IN) $\rightarrow$ Anode von `D3` (Schottky Verpolungsschutz).
  - Kathode von `D3` $\rightarrow$ Net `+5V` (speist Relaisspulen, C1, C2 und den 5V-Pin der Bluepill).
  - `J1` Pin 2 $\rightarrow$ Net `GND` (GND-Ebene).
- **Bluepill-Integration:**
  - Der Bluepill-Sockel (`U1`/`U2`) wird so platziert, dass der 5V-Pin mit `+5V` verbunden ist und die GND-Pins auf das `GND`-Netz gelegt werden. Der interne Regler der Bluepill erzeugt die $3.3\,\text{V}$ für den STM32-Chip.
- **Relaiskanal 1:**
  - Steuerpin von Bluepill (z. B. `PA0`) $\rightarrow$ `R1` ($1\,\text{k}\Omega$) $\rightarrow$ Basis von `Q1` (NPN).
  - Emitter von `Q1` $\rightarrow$ `GND`.
  - Collector von `Q1` $\rightarrow$ Net `RY1_Drive`.
  - Relaisspule `RY1` liegt zwischen `+5V` und `RY1_Drive`.
  - Freilaufdiode `D1` liegt antiparallel zur Spule (Kathode an `+5V`, Anode an `RY1_Drive`).
  - Status-LED `D4` mit Vorwiderstand `R3` ($330\,\Omega$) liegt ebenfalls parallel zur Spule (Anode an `+5V`, Kathode über `R3` an `RY1_Drive`).
  - Relaiskontakt-Pins von `RY1` gehen direkt zu `J2` (Pin 1: COM, Pin 2: NO, Pin 3: NC).
- **Relaiskanal 2:**
  - Steuerpin von Bluepill (z. B. `PA1`) $\rightarrow$ `R2` ($1\,\text{k}\Omega$) $\rightarrow$ Basis von `Q2` (NPN).
  - Emitter von `Q2` $\rightarrow$ `GND`.
  - Collector von `Q2` $\rightarrow$ Net `RY2_Drive`.
  - Relaisspule `RY2` liegt zwischen `+5V` und `RY2_Drive`.
  - Freilaufdiode `D2` antiparallel zur Spule (Kathode an `+5V`, Anode an `RY2_Drive`).
  - Status-LED `D5` mit Vorwiderstand `R4` ($330\,\Omega$) parallel zur Spule (Anode an `+5V`, Kathode über `R4` an `RY2_Drive`).
  - Relaiskontakt-Pins von `RY2` gehen direkt zu `J3` (Pin 1: COM, Pin 2: NO, Pin 3: NC).
- **Sensor-Schnittstelle:**
  - `J4` Pin 1 $\rightarrow$ `+5V` / `+3.3V` (wählbar oder fest verdrahtet).
  - `J4` Pin 2 $\rightarrow$ `GND`.
  - `J4` Pin 3 $\rightarrow$ Bluepill `PB6` (I2C1_SCL oder digitaler Pin).
  - `J4` Pin 4 $\rightarrow$ Bluepill `PB7` (I2C1_SDA oder digitaler Pin).

---

### 5. Layout- & Routingvorgaben für den Bau-Agenten
1. **Projekt anlegen:** Erzeuge ein neues KiCad-Projekt im Arbeitsverzeichnis.
2. **Schaltplan zeichnen:** Setze obiges Schaltungsdesign um. Verwende globale Labels für saubere Strukturierung.
3. **Leiterplatten-Umriss:** Ziehe ein Rechteck mit den Maßen $80.0\,\text{mm} \times 70.0\,\text{mm}$ auf dem Layer `Edge.Cuts`. Platziere die vier M3-Bohrungen wie spezifiziert.
4. **Platzierung:**
   - Setze die Bluepill-Sockel `U1, U2` mittig auf das Board.
   - Platziere `J1` (Power) an der linken Kante, `J4` (Sensor) an der Unterkante.
   - Setze `RY1, RY2` sowie deren Schraubklemmen `J2, J3` an die rechte Kante, sodass die Hochspannungsleitungen maximal kurz sind.
   - Die Treiberstufen (Transistoren, Dioden, Vorwiderstände) nahe bei den Relais platzieren.
   - Die Status-LEDs `D4, D5` gut sichtbar am oberen Rand positionieren.
5. **Kupferflächen & Planes:** Erzeuge durchgehende GND-Zonen auf `In1.Cu` sowie `B.Cu` und eine 5V-Zone auf `In2.Cu`.
6. **Isolationsschlitz (Kriechstrecke):** Zeichne im Edge.Cuts-Layer eine nicht-metallisierte Ausfräsung (Slot, Breite $1.5\,\text{mm}$) unter den Relaiskontakten hindurch, um den Laststromkreis (230V-tauglich) galvanisch und physisch vom Niederspannungs-Steuerstromkreis (5V/3.3V) der Bluepill zu trennen.
7. **Routing:** Starte das Freerouting-Plugin für optimales, kreuzungsfreies 4-Layer-Autorouting unter Einhaltung der Leiterbahnbreiten.
8. **Prüfung & Export:** Führe den Design Rule Check (DRC) fehlerfrei aus und exportiere alle Gerberdaten, Bohrdateien sowie Bestückungsdaten sauber in das Verzeichnis `./gerbers`.

---

*Hinweis für den Bau-Agenten: Bitte führe diese Schritte strukturiert aus und gib am Ende ein ehrliches, kurzes Feedback darüber, was beim automatisierten Generieren und Routen der Platine geklappt hat.*
