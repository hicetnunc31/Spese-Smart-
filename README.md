# Spese Smart — prototipo 0.1

## Funzioni incluse
- Registrazione manuale di entrate e uscite
- Tentativo di riconoscimento vocale in italiano dove il browser lo supporta
- Fallback alla dettatura della tastiera iOS
- Categorie e metodi di pagamento
- Budget mensile complessivo e per categoria
- Dashboard mensile, storico e riepilogo per categoria
- Modifica e cancellazione dei movimenti
- Esportazione backup JSON e CSV; ripristino JSON
- Cache offline tramite service worker dopo il primo accesso

## Installazione su iPhone
Questa cartella deve essere pubblicata su un hosting HTTPS (ad esempio un proprio spazio web statico).
1. Apri l'indirizzo in Safari su iPhone.
2. Tocca Condividi.
3. Scegli "Aggiungi alla schermata Home".
4. Avvia Spese Smart dall'icona creata.

## Limiti importanti del prototipo
- I dati sono memorizzati localmente nel browser/dispositivo. Non c'è sincronizzazione cloud automatica.
- Esporta periodicamente il backup JSON, soprattutto prima di cancellare i dati del browser o cambiare iPhone.
- Il riconoscimento vocale diretto dipende dal supporto del browser. In assenza, usa il microfono della tastiera iOS nel campo descrizione e poi verifica i campi prima di salvare.
- Il prototipo non si collega a banche o carte e non importa automaticamente transazioni.
- Prima di usare dati reali in modo continuativo, è opportuno testare il flusso e valutare una soluzione di backup/sincronizzazione sicura.

## File
- `index.html`: app
- `manifest.webmanifest`: metadati PWA
- `sw.js`: cache offline


## Protezione dei dati — versione cifrata
- Al primo avvio si crea una password di almeno 10 caratteri.
- I dati salvati nel browser sono cifrati con AES-GCM 256 bit; la chiave deriva dalla password tramite PBKDF2-SHA-256 (310.000 iterazioni).
- La password non viene inviata al server e non è recuperabile. Se viene dimenticata, i dati non sono recuperabili senza una copia della password.
- Il backup JSON è cifrato. L'esportazione CSV è in chiaro e mostra un avviso prima del download.
- I dati restano sul dispositivo/browser; non esiste sincronizzazione cloud. Il blocco automatico al passaggio in background è una protezione aggiuntiva, non una garanzia assoluta su ogni versione iOS.
- Usare solo tramite HTTPS e proteggere anche l'iPhone con codice/Face ID.


## Accesso biometrico opzionale (Touch ID)
- La versione aggiornata aggiunge WebAuthn con autenticatore di piattaforma e estensione PRF, se supportata da iOS/Safari.
- Dopo aver sbloccato con la password, si può scegliere “Attiva Touch ID”. La chiave AES dell’archivio esistente viene avvolta in memoria con una chiave derivata dal risultato PRF; l’archivio cifrato e la sua chiave/sale originari non vengono sostituiti.
- Se il browser non conferma il supporto PRF, l’attivazione viene rifiutata e si continua con la password.
- La password resta sempre disponibile. Non cancellare dati del sito né cambiare dominio senza prima esportare e verificare un backup cifrato. La biometria è verificata da iOS: l’app non legge né memorizza l’impronta.
