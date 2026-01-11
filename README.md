# 🌡️ Cronotermostato Node-RED per openHAB

Un cronotermostato settimanale completo con interfaccia Dashboard 2.x (Vue.js) per Node-RED, progettato per integrarsi con openHAB.

![Node-RED](https://img.shields.io/badge/Node--RED-4.x-red?logo=nodered)
![Dashboard](https://img.shields.io/badge/Dashboard-2.x-blue)
![openHAB](https://img.shields.io/badge/openHAB-4.x-orange)
![License](https://img.shields.io/badge/License-MIT-green)

## 📸 Screenshot

```
┌─────────────────────────────────────────────────────────┐
│  🕐  Dom 12/1  14:35                            [OGGI]  │
├─────────────────────────────────────────────────────────┤
│                    PROGRAMMAZIONE                       │
│                      DOMENICA                           │
│               [◀]    [OGGI]    [▶]                      │
├─────────────────────────────────────────────────────────┤
│ 18° 18° 18° 20° 20° 21° 21° 21° 21° 21° 19° [19°]      │
│ ▄▄  ▄▄  ▄▄  ██  ██  ██  ██  ██  ██  ██  ▄▄   ▄▄       │
│  0   1   2   3   4   5   6   7   8   9  10   11        │
├─────────────────────────────────────────────────────────┤
│ 19° 21° 21° 21° 21° 21° 21° 21° 21° 21° 20° 20°        │
│ ▄▄  ██  ██  ██  ██  ██  ██  ██  ██  ██  ██  ██         │
│ 12  13  14  15  16  17  18  19  20  21  22  23         │
├─────────────────────────────────────────────────────────┤
│  21°C                                    Ore 11:00      │
├─────────────────────────────────────────────────────────┤
│  [▼ Giù]  [▲ Su]  [⧉ Copia]  [💾 Salva]  [✕ Annulla]  │
├─────────────────────────────────────────────────────────┤
│ ┌─────────────┐  ┌────────────────────────────────────┐ │
│ │   GAUGE     │  │  🔥 Caldaia           ACCESA       │ │
│ │             │  ├────────────────────────────────────┤ │
│ │   20.4°C    │  │  🎯 Setpoint          21°C        │ │
│ │ Temperatura │  ├────────────────────────────────────┤ │
│ └─────────────┘  │  🅰️ Modalità          AUTO        │ │
│                  └────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

## ✨ Funzionalità

### Programmazione
- **Programmazione settimanale**: 7 giorni × 24 ore = 168 slot programmabili
- **Range temperatura**: 14°C - 26°C con step di 0.5°C
- **Copia giornata**: copia l'intera programmazione di un giorno nel successivo
- **Persistenza**: salvataggio programmazione su file JSON

### Modalità operative
- 🅰️ **AUTO**: segue la programmazione settimanale
- ⭕ **OFF**: riscaldamento disattivato
- 🔧 **MANUALE**: temperatura fissa impostabile

### Interfaccia
- **Dashboard 2.x** con Vue.js e tema scuro
- **Gauge temperatura** con ago animato e segmenti colorati
- **Icona caldaia animata**: fiamma accesa/spenta con effetto glow
- **Barre orarie colorate** in base alla temperatura programmata
- **Evidenziazione ora corrente** con bordo rosso
- **Badge "OGGI"** quando si visualizza il giorno corrente

### Automazione
- **Visualizzazione giorno corrente** all'avvio del flow
- **Timeout automatico**: ritorna al giorno corrente dopo 30 secondi di inattività
- **Isteresi configurabile**: controllo preciso con soglie (+0.1°C / -0.2°C)
- **Anti-rimbalzo**: filtro RBE per evitare comandi ripetuti

## 📋 Prerequisiti

### Software
- [Node-RED](https://nodered.org/) 4.x o superiore
- [openHAB](https://www.openhab.org/) 4.x o superiore

### Nodi Node-RED richiesti

```bash
npm install @flowfuse/node-red-dashboard
npm install node-red-contrib-ramp-thermostat
npm install @essenius/node-red-openhab4
```

Oppure installa tramite la Palette di Node-RED:
- `@flowfuse/node-red-dashboard` (Dashboard 2.x)
- `node-red-contrib-ramp-thermostat`
- `@essenius/node-red-openhab4`

## 🚀 Installazione

### 1. Clona il repository

```bash
git clone https://github.com/tuousername/cronotermostato-nodered.git
cd cronotermostato-nodered
```

### 2. Importa il flow in Node-RED

1. Apri Node-RED (`http://localhost:1880`)
2. Menu ☰ → **Import**
3. Seleziona il file `cronotermostato_v6.json`
4. Click **Import**

### 3. Configura gli item openHAB

Modifica i seguenti nodi per adattarli ai tuoi item openHAB:

| Nodo | Item di default | Descrizione |
|------|-----------------|-------------|
| `SonoOffT` | `SonoOffT` | Sensore temperatura ambiente |
| `Modo` | `Riscaldamento_Modo` | Selettore modalità (1=AUTO, 2=OFF, 3=MAN) |
| `Caldaia` | `FGS221_002_SwitchBinary2` | Relè caldaia |
| `SP Auto` | `Riscaldamento_Auto_SP` | Setpoint automatico (feedback) |

### 4. Configura il file di salvataggio

Il percorso di default è:
```
/etc/openhab/nodered/data/scheduler_new.log
```

Per modificarlo, aggiorna i nodi `Salva` e `Carica`.

### 5. Deploy

Click sul pulsante **Deploy** in Node-RED.

### 6. Accedi alla Dashboard

```
http://<IP-SERVER>:1880/dashboard/termostato
```

## 🎮 Utilizzo

### Navigazione giorni
- **◀**: giorno precedente
- **▶**: giorno successivo
- **OGGI**: torna al giorno corrente

### Programmazione temperatura
1. Clicca su uno slot orario (0-23)
2. Usa **▲ Su** / **▼ Giù** per regolare la temperatura
3. Lo slot selezionato è evidenziato in verde
4. L'ora corrente è evidenziata con bordo rosso

### Copia programmazione
- **⧉ Copia**: copia l'intera programmazione del giorno corrente nel giorno successivo e passa a visualizzarlo

### Salvataggio
- **💾 Salva**: salva la programmazione su file (persistente al riavvio)
- **✕ Annulla**: ricarica l'ultima programmazione salvata e torna al giorno corrente

### Timeout automatico
Dopo 30 secondi di inattività, se stai visualizzando un giorno diverso da oggi, la vista torna automaticamente al giorno corrente.

## ⚙️ Configurazione openHAB

### Item richiesti

Aggiungi questi item al tuo file `.items`:

```java
// Temperatura ambiente (dal tuo sensore)
Number SonoOffT "Temperatura [%.1f °C]" <temperature>

// Modalità riscaldamento
Number Riscaldamento_Modo "Modo [MAP(riscaldamento.map):%s]" <heating>

// Setpoint automatico (feedback dal cronotermostato)  
Number Riscaldamento_Auto_SP "Setpoint Auto [%.1f °C]" <temperature>

// Setpoint manuale
Number Riscaldamento_Manual_SP "Setpoint Manuale [%.1f °C]" <temperature>

// Relè caldaia
Switch FGS221_002_SwitchBinary2 "Caldaia" <fire>
```

### Mappa modalità (riscaldamento.map)

```
1=AUTO
2=OFF
3=MANUALE
NULL=N/D
-=N/D
```

### Sitemap di esempio

```java
sitemap riscaldamento label="Riscaldamento" {
    
    Frame label="Stato" {
        Text item=SonoOffT icon="temperature"
        Text item=Riscaldamento_Auto_SP icon="temperature" visibility=[Riscaldamento_Modo==1]
        Text item=FGS221_002_SwitchBinary2 icon="fire"
    }
    
    Frame label="Controllo" {
        Selection item=Riscaldamento_Modo mappings=[1="AUTO", 2="OFF", 3="MANUALE"] icon="heating"
        Setpoint item=Riscaldamento_Manual_SP minValue=14 maxValue=26 step=0.5 visibility=[Riscaldamento_Modo==3] icon="temperature"
    }
    
    Frame label="Programmazione" {
        Webview url="/nodered/dashboard/termostato" height=20
    }
}
```

## 🔧 Personalizzazione

### Modificare range temperatura

Nel nodo `Logica Cronotermostato`, modifica le costanti:

```javascript
const TEMP_MIN = 14;    // Temperatura minima
const TEMP_MAX = 26;    // Temperatura massima
const TEMP_STEP = 0.5;  // Step di regolazione
```

### Modificare timeout ritorno a oggi

Nel nodo `Logica Cronotermostato`, nella sezione **On Start** (inizializzazione):

```javascript
flow.set('timeoutSeconds', 30);  // Cambia questo valore (in secondi)
```

### Modificare isteresi

Nel nodo `Calcolo Termostato`:

```javascript
msg3.payload = 0.1;  // Isteresi positiva
msg4.payload = 0.2;  // Isteresi negativa
```

### Modificare polling

Il nodo `inject` "30s" controlla la frequenza di aggiornamento del termostato. Modifica il campo `repeat` per cambiare l'intervallo.

Il nodo `inject` "Ogni 10s" controlla l'aggiornamento dell'ora e il check del timeout.

### Personalizzare tema

Modifica il nodo `ui-theme` "Dark" per cambiare i colori:

```json
{
  "surface": "#1a1a2e",
  "primary": "#4ecdc4",
  "bgPage": "#0f0f1a",
  "groupBg": "#16213e",
  "groupOutline": "#1f2b4a"
}
```

### Personalizzare colori barre temperatura

Nel template Vue.js, funzione `barCss()`:

```javascript
if (t <= 16) c = '#1E88E5';      // Blu (freddo)
else if (t <= 18) c = '#00ACC1'; // Ciano
else if (t <= 20) c = '#388E3C'; // Verde
else if (t <= 22) c = '#8BC34A'; // Verde chiaro
else if (t <= 24) c = '#FFC107'; // Giallo
else c = '#FF5722';              // Arancione (caldo)
```

## 📁 Struttura file

```
cronotermostato-nodered/
├── README.md
├── LICENSE
├── cronotermostato_v6.json    # Flow Node-RED principale
└── examples/
    └── openhab/
        ├── items/
        │   └── riscaldamento.items
        ├── sitemaps/
        │   └── riscaldamento.sitemap
        └── transform/
            └── riscaldamento.map
```

## 🐛 Troubleshooting

### La dashboard non si carica
- Verifica che `@flowfuse/node-red-dashboard` sia installato
- Controlla la console del browser (F12) per errori JavaScript
- Verifica che il path sia corretto: `/dashboard/termostato`

### Temperatura non rilevata
- Verifica che l'item openHAB sia corretto
- Controlla lo status del nodo `Salva Temp` (deve mostrare la temperatura)
- Verifica la connessione al controller openHAB

### Caldaia non si accende
- Verifica la connessione openHAB (nodo controller)
- Controlla che l'item della caldaia sia corretto
- Verifica lo status del nodo `Output Caldaia`
- Controlla che la modalità non sia "OFF"

### Programmazione non salvata
- Verifica i permessi sulla cartella `/etc/openhab/nodered/data/`
- Crea la cartella se non esiste: `mkdir -p /etc/openhab/nodered/data`
- Controlla lo status del nodo `Salva`

### Gauge non si aggiorna
- Il gauge si aggiorna ogni 30 secondi (polling termostato)
- Verifica che il sensore temperatura invii dati
- Controlla il nodo `Aggiorna Status Panel`

### Timeout non funziona
- Verifica che il nodo `inject` "Ogni 10s" sia attivo
- Controlla il valore di `timeoutSeconds` nel nodo logica

## 📄 Licenza

Questo progetto è rilasciato sotto licenza MIT. Vedi il file [LICENSE](LICENSE) per i dettagli.

## 🤝 Contributi

Contributi, issue e feature request sono benvenuti!

1. Fork del repository
2. Crea un branch per la feature (`git checkout -b feature/nuova-funzione`)
3. Commit delle modifiche (`git commit -m 'Aggiunge nuova funzione'`)
4. Push sul branch (`git push origin feature/nuova-funzione`)
5. Apri una Pull Request

## 📝 Changelog

### v6 (Corrente)
- Nuovo pannello stato con gauge SVG e icone animate
- Layout a due colonne per gauge e stati
- Icona caldaia animata (fiamma accesa/spenta)
- Temperatura forzata a 1 decimale
- Rimossi widget Dashboard separati (tutto integrato in Vue.js)
- Layout più compatto e moderno

### v5
- Aggiunta barra data/ora sempre visibile
- Visualizzazione giorno corrente all'avvio
- Timeout automatico ritorno a oggi
- Pulsante "OGGI" per tornare al giorno corrente
- Evidenziazione ora corrente

### v4
- Fix loop navigazione giorni
- Copia intera giornata invece di singola ora
- Pulsanti ◀ ▶ separati

### v3
- Prima versione Dashboard 2.x con Vue.js
- Tema scuro
- Barre temperatura colorate

## 👨‍💻 Autore

Creato con ❤️ per la community openHAB e Node-RED.

## 🙏 Ringraziamenti

- [Node-RED](https://nodered.org/)
- [FlowFuse Dashboard](https://dashboard.flowfuse.com/)
- [openHAB](https://www.openhab.org/)
- [node-red-contrib-ramp-thermostat](https://flows.nodered.org/node/node-red-contrib-ramp-thermostat)
