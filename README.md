# Playwright

## Installation

Deux setups possibles:
- WSL + Xserver
- Windows + node

### WSL + Xserver

#### Sur Windows

- Installer WSL2 (dÃ©jÃ  fait ^^)
- Installer Xserver (VcXsrv) [Lien pour le tÃ©lÃ©charger](https://sourceforge.net/projects/vcxsrv/)
- Lancer XLaunch
    - Laisser "Multiple Windows"
    - Suivant 
    - Laisser "Start no client"
    - Suivant
    - Ajoutez bien "Disable access control"
    - Suivant
    - Terminer

#### Sur WSL

- Mettre Ã  jour votre Ubuntu
```bash
# update a list of packages in the repositories to download
sudo apt update
# apply downloaded changes
sudo apt upgrade -y
```
- Configurer la communication entre WSL et Xserver
    - Ouvrir le fichier `~/.bashrc`
    ```bash
    code ~/.bashrc
    ```
    - Ajouter les lignes suivantes Ã  la fin du fichier
    ```bash
    export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2; exit;}'):0.0
    sudo /etc/init.d/dbus start &> /dev/null
    ```
    - Donner Ã  dbus les droits d'administrateur
    ```bash
    sudo visudo -f /etc/sudoers.d/dbus
    ```
    - Ajouter la ligne suivante
    ```bash
    <your_username> ALL = (root) NOPASSWD: /etc/init.d/dbus
    ```
    - remplacer `<your_username>` par votre nom d'utilisateur
    - si vous ne connaissez pas votre nom d'utilisateur, tapez la commande `whoami` dans le terminal
    - Sauvegarder et quitter le fichier
        - `Ctrl + X`
        - `Y`
        - `EntrÃ©e`
    - RedÃ©marrer WSL ou taper la commande suivante
    ```bash
    source ~/.bashrc
    ```
- Installer les librairies nÃ©cessaires
```bash
sudo apt-get install ca-certificates fonts-liberation libappindicator3-1 libasound2 libatk-bridge2.0-0 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgbm1 libgcc1 libglib2.0-0 libgtk-3-0 libnspr4 libnss3 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 lsb-release wget xdg-utils
```

### Windows + NodeJS

- Installer NodeJS [Lien pour le tÃ©lÃ©charger](https://nodejs.org/en/download/)
- Priez pour que Ã§a marche vu que c'est Windows
- Il va falloir apprendre Ã  crÃ©er un projet dans windows et gÃ©rer vos clÃ©s ssh

Blague Ã  part, vous pouvez aller avec WSL dans vos fichiers de Windows en allant dans `/mnt/c/Users/<your_username_windows>`

## CrÃ©er un projet Playwright

- CrÃ©er un projet vite en React + Typescript, qui se nomme tests-e2e-playwright
```bash
npm init vite@latest
```
- Installer les dÃ©pendances
```bash
cd tests-e2e-playwright
npm install
```
- Installer Playwright
```bash
npm init playwright@latest
```
    - Where to put your end-to-end tests? `e2e`
    - Add a Github Actions workflow? `y`
    - Install Playwright browsers (can be done manually via 'npx playwright install')? `y`
    - Tout ce qu'il y a aprÃ¨s, appuyez sur `yes` et `Enter` ^^

## Lancer les tests

- CrÃ©er un script dans `package.json`
```json
"scripts": {
    ...
    "e2e": "playwright test"
}
```
- Lancer les tests
```bash
npm run e2e
```

## S'amuser avec Playwright

Essayez de comprendre les tests e2e qui sont gÃ©nÃ©rÃ©s par Playwright.
Lancer les tests en headless et en mode debug.

### En headless

les tests sont lancÃ©s sans ouvrir de navigateur.

```bash
npm run e2e
```

ou 

```bash
npx playwright test
```

### En mode UI (headless false)

les tests sont lancÃ©s avec un navigateur ouvert.

```bash
npx playwright test --ui
```

En mode debug, les tests sont lancÃ©s avec un navigateur ouvert et un mode debug qui permet de passer d'Ã©tapes en Ã©tapes.

```bash
npx playwright test --debug
```

### CrÃ©er des tests

avec l'outil Record / Codegen de Playwright.

- Lancer l'outil
```bash
npx playwright codegen
```
- Suivre les instructions
- Copier le code gÃ©nÃ©rÃ© dans le fichier `tests/e2e/example.spec.ts`
- Lancer les tests
```bash
npm run e2e
```

### Changer la configuration

- Ouvrir le fichier `playwright.config.ts`
- Changer la configuration en supprimant les navigateurs que vous ne voulez pas utiliser. Conservez un seul navigateur fonctionnel sur votre machine.
- relancer les tests
```bash
npx playwright test --ui
```

## TP - Compteur

- Lancez votre application `vite`
- CrÃ©ez un test qui vÃ©rifie que le compteur est bien Ã  0 au dÃ©marrage
- CrÃ©ez un test qui vÃ©rifie que le compteur est bien Ã  2 aprÃ¨s deux clics sur le bouton `+`
- Ajouter votre code sur github
- Allez dans l'onglet `Actions` de votre projet
- VÃ©rifiez que les tests passent bien

Vous venez de crÃ©er votre premier test e2e avec Playwright et de l'intÃ©grer Ã  votre pipeline de CI de Github !