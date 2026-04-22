# beertrack-front

App mobile Beertrack — le Strava de la bière.

## Stack

- **Expo** SDK 54+ + **React Native**
- **Gluestack UI v4** — composants UI
- **NativeWind v5 + Tailwind CSS v4** — styling
- **Expo Router** — navigation
- **`@supabase/supabase-js`** — auth + DB directe
- Auth gérée directement par Supabase (pas via l'API NestJS)

## Prérequis

- Node.js >= 20
- Expo CLI : `npm i -g expo-cli` (ou via `npx expo`)
- iOS : Xcode + simulateur / Android : Android Studio + émulateur
- App Expo Go sur ton téléphone (pour le dev sans build natif)

## Installation

```bash
npm install
```

## Variables d'environnement

Copier `.env.example` en `.env` et remplir les valeurs :

```bash
cp .env.example .env
```

| Variable | Description |
|---|---|
| `EXPO_PUBLIC_SUPABASE_URL` | URL du projet Supabase |
| `EXPO_PUBLIC_SUPABASE_ANON_KEY` | Clé publique Supabase |
| `EXPO_PUBLIC_API_BASE_URL` | URL de l'API NestJS (ex: `http://localhost:3000/v1`) |

> Les variables préfixées `EXPO_PUBLIC_` sont exposées côté client. Ne jamais y mettre la `service_role key`.

## Lancer en dev

```bash
npx expo start           # QR code pour Expo Go
npx expo start --ios     # simulateur iOS
npx expo start --android # émulateur Android
```

## Tests

À configurer (Jest + Testing Library).

## Déploiement

Via EAS Build :

```bash
npx eas build --platform ios
npx eas build --platform android
npx eas submit
```
