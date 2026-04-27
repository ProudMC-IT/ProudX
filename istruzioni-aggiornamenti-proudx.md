# ProudX - Gestione del fork da Velocity

ProudX è un fork di [PaperMC/Velocity](https://github.com/PaperMC/Velocity).

La repo originale Velocity usa come branch principale:

```text
dev/3.0.0
````

Nel nostro fork usiamo questo flusso:

```text
dev/3.0.0     = copia pulita aggiornata di Velocity
proudx-custom = Velocity + modifiche ProudX
```

## Regola principale

Non modificare direttamente `dev/3.0.0`.

Le modifiche di ProudX vanno fatte su:

```text
proudx-custom
```

In questo modo possiamo aggiornare ProudX con gli ultimi cambiamenti di Velocity senza perdere le nostre modifiche.

---

## Configurazione iniziale

Controllare i remote:

```powershell
git remote -v
```

Se manca `upstream`, aggiungere la repo originale Velocity:

```powershell
git remote add upstream https://github.com/PaperMC/Velocity.git
```

Poi aggiornare i dati da Velocity:

```powershell
git fetch upstream
git remote set-head upstream -a
```

Verificare il branch principale di Velocity:

```powershell
git remote show upstream
```

Dovrebbe comparire:

```text
HEAD branch: dev/3.0.0
```

---

## Branch usati

### `dev/3.0.0`

Questo branch deve restare pulito e aggiornato con Velocity.

Serve come base per ProudX.

### `proudx-custom`

Questo è il branch dove facciamo le modifiche di ProudX.

È il branch consigliato da usare come branch principale del progetto ProudX.

---

## Creare il branch ProudX

Se non esiste ancora:

```powershell
git checkout dev/3.0.0
git fetch upstream
git merge upstream/HEAD
git push origin dev/3.0.0

git checkout -b proudx-custom
git push -u origin proudx-custom
```

---

## Fare modifiche a ProudX

Prima spostarsi sul branch ProudX:

```powershell
git checkout proudx-custom
```

Poi modificare i file.

Quando le modifiche sono pronte:

```powershell
git status
git add .
git commit -m "Customize ProudX"
git push
```

---

## Aggiornare ProudX quando Velocity viene aggiornata

Quando la repo originale Velocity riceve aggiornamenti, prima aggiorniamo il branch pulito:

```powershell
git checkout dev/3.0.0
git fetch upstream
git merge upstream/HEAD
git push origin dev/3.0.0
```

Poi portiamo gli aggiornamenti dentro ProudX:

```powershell
git checkout proudx-custom
git rebase dev/3.0.0
git push --force-with-lease
```

---

## Comandi rapidi per aggiornare tutto

Usare questi comandi ogni volta che si vuole aggiornare ProudX con gli ultimi cambiamenti di Velocity:

```powershell
git checkout dev/3.0.0
git fetch upstream
git merge upstream/HEAD
git push origin dev/3.0.0

git checkout proudx-custom
git rebase dev/3.0.0
git push --force-with-lease
```

---

## Se ci sono conflitti

Durante il rebase Git potrebbe segnalare conflitti.

Aprire i file indicati da Git e sistemare le parti segnate così:

```text
<<<<<<< HEAD
codice aggiornato da Velocity
=======
codice modificato da ProudX
>>>>>>> commit ProudX
```

Dopo aver risolto i conflitti:

```powershell
git add .
git rebase --continue
```

Se ci sono altri conflitti, ripetere gli stessi passaggi.

Quando il rebase è finito:

```powershell
git push --force-with-lease
```

Per annullare il rebase:

```powershell
git rebase --abort
```

---

## Branch default su GitHub

Per ProudX è consigliato usare come branch default:

```text
proudx-custom
```

Per cambiarlo da GitHub:

1. Aprire la repo `ProudMC-IT/ProudX`
2. Andare su **Settings**
3. Andare su **Branches**
4. Cambiare **Default branch**
5. Selezionare `proudx-custom`

Oppure con GitHub CLI:

```powershell
gh repo edit ProudMC-IT/ProudX --default-branch proudx-custom
```

---

## Riassunto

```text
upstream/HEAD  = branch principale aggiornato di Velocity
dev/3.0.0      = copia pulita di Velocity nel fork ProudX
proudx-custom  = branch con le modifiche di ProudX
```

Flusso consigliato:

```text
Velocity aggiorna dev/3.0.0
        ↓
aggiorniamo dev/3.0.0 nel fork
        ↓
facciamo rebase di proudx-custom
        ↓
ProudX ha sia gli aggiornamenti Velocity sia le modifiche nostre
```

La regola importante è:

```text
Modifiche ProudX solo su proudx-custom.
dev/3.0.0 resta pulito e sincronizzato con Velocity.
```
