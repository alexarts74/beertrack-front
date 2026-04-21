# Front Beertrack (Expo) — Instructions Claude Code

## Stack
- **Expo** (SDK 54+)
- **React Native**
- **Gluestack UI v4** (composants UI)
- **NativeWind v5 + Tailwind CSS v4** (styling)
- **Expo Router** (navigation)

## Priorités des Skills

### UI & Styling
- Pour les composants UI et le styling → suivre **Gluestack UI v4** en priorité
- Pour la navigation, animations et patterns natifs iOS/Android → suivre **Expo `building-native-ui`**
- En cas de conflit → **Gluestack prévaut pour le styling**, **Expo prévaut pour la navigation et l'architecture**

### Performance
- Optimisation JS, rendering, bundle size → suivre **Callstack `react-native-best-practices`**
- Builds et déploiement → suivre **Expo `expo-dev-client`** et **`expo-api-routes`**

### Data & Réseau
- Data fetching, caching, mode offline → suivre **Expo `native-data-fetching`**

### Styling Engine
- Utiliser **NativeWind v5 + Tailwind CSS v4** comme moteur de styling
- Config initiale → skill **Expo `expo-tailwind-setup`**
- Usage quotidien avec composants → skill **Gluestack `styling`**

## Conventions front
- Un composant = un fichier
- Les écrans dans `app/(tabs)/` ou `app/(auth)/` selon le contexte Expo Router
- Les composants réutilisables dans `components/`
- Les hooks custom dans `hooks/` (préfixés `use*`)
- Les types dans `types/`
- Les services API dans `services/`

## Auth (Supabase Auth)
L'auth n'est **pas** faite via notre API NestJS — elle passe **directement** entre le front et Supabase.

- Utiliser **`@supabase/supabase-js`** côté front pour :
  - `supabase.auth.signUp()` / `signInWithPassword()` / `signInWithOAuth()` / `signOut()`
  - `supabase.auth.getSession()` / `onAuthStateChange()` pour suivre l'état connecté
  - Le refresh token est géré automatiquement par le SDK — ne pas implémenter un refresh custom
- Configurer le SDK avec **`expo-secure-store`** comme storage (via l'option `storage` du client Supabase) pour que les tokens ne soient jamais en clair dans AsyncStorage
- Pour les appels à notre API NestJS, récupérer le token via `supabase.auth.getSession()` et l'ajouter en header `Authorization: Bearer <access_token>`
- Sur 401 depuis notre API → laisser le SDK gérer le refresh (`onAuthStateChange` émet un event), ne pas refaire le flow manuellement

### Setup recommandé
```ts
// lib/supabase.ts
import 'react-native-url-polyfill/auto'
import AsyncStorage from '@react-native-async-storage/async-storage'
import * as SecureStore from 'expo-secure-store'
import { createClient } from '@supabase/supabase-js'

const ExpoSecureStoreAdapter = {
  getItem: (key: string) => SecureStore.getItemAsync(key),
  setItem: (key: string, value: string) => SecureStore.setItemAsync(key, value),
  removeItem: (key: string) => SecureStore.deleteItemAsync(key),
}

export const supabase = createClient(
  process.env.EXPO_PUBLIC_SUPABASE_URL!,
  process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY!,
  {
    auth: {
      storage: ExpoSecureStoreAdapter,
      autoRefreshToken: true,
      persistSession: true,
      detectSessionInUrl: false, // false en React Native
    },
  },
)
```

## Sécurité
- **Jamais** mettre `SUPABASE_SERVICE_ROLE_KEY` dans le front — cette clé bypass les RLS et ne doit vivre que côté API
- Utiliser uniquement `EXPO_PUBLIC_SUPABASE_URL` et `EXPO_PUBLIC_SUPABASE_ANON_KEY` côté front
- Tokens gérés par le SDK Supabase dans `expo-secure-store` (pas AsyncStorage)

## Commandes
```bash
npm install
npx expo start           # serveur de dev
npx expo start --ios     # simulator iOS
npx expo start --android # émulateur Android
```

## Skills installés
Voir `.claude/skills/` et l'inventaire détaillé dans `.claude/skills-inventory.md`.

## Résolution des conflits entre skills

Plusieurs skills se recoupent et donnent des consignes contradictoires. Voici la règle de précédence à appliquer :

### Conflit 1 — Moteur de styling (CSS/Tailwind vs inline styles)
- `building-native-ui` dit : *"CSS and Tailwind are not supported - use inline styles"* + *"Inline styles not StyleSheet.create"*
- `expo-tailwind-setup` et `gluestack-ui-v4` imposent NativeWind + Tailwind via `className`
- **Résolution** : on utilise **NativeWind + Tailwind** (puisqu'on a délibérément installé Gluestack UI v4). `building-native-ui` suppose un projet Expo vanilla — sa règle "inline styles" est **ignorée** dans notre contexte.

### Conflit 2 — Primitives RN vs composants Gluestack
- `building-native-ui` recommande directement `ScrollView`, `View`, `Text` de `react-native` avec des props Expo (`contentInsetAdjustmentBehavior="automatic"`)
- `gluestack-ui-v4/components` impose `Box`, `Text`, `ScrollView` depuis `@/components/ui/*`
- **Résolution** : on utilise **les composants Gluestack** (`Box`, `Text`, `Pressable`, `ScrollView`…) importés depuis `@/components/ui/*`. On passe les props Expo utiles (`contentInsetAdjustmentBehavior`, etc.) au composant Gluestack — elles sont transmises au primitive RN sous-jacent.

### Conflit 3 — Couleurs / thème
- `building-native-ui` propose `PlatformColor("label")` pour suivre le thème iOS natif
- `gluestack-ui-v4/styling` impose les **semantic tokens** (`text-foreground`, `bg-background`, `bg-primary`, `text-destructive`…)
- **Résolution** : on utilise **exclusivement les semantic tokens Gluestack**. Ne jamais utiliser `PlatformColor`, ni les couleurs Tailwind numérotées (`gray-500`, `red-600`…), ni les couleurs inline (`#FFF`, `bg-white`). Le thème clair/sombre est géré automatiquement par Gluestack via le `GluestackUIProvider`.

### Conflit 4 — Icônes
- `building-native-ui` recommande `expo-image` avec `source="sf:name"` pour SF Symbols (iOS natif)
- Gluestack utilise habituellement `lucide-react-native`
- **Résolution** : **`lucide-react-native`** (via le wrapper Gluestack) pour la cohérence cross-platform. Utiliser les SF Symbols uniquement pour les headers / tab bars natifs où Expo Router le gère automatiquement.

### Conflit 5 — Navigation
- `building-native-ui` couvre Expo Router (stacks, NativeTabs, form sheets, link previews)
- Gluestack ne couvre pas la navigation
- **Résolution** : **aucun conflit**. `building-native-ui` fait autorité sur la navigation, l'architecture des routes (`app/(tabs)`, `app/(auth)`), et les patterns natifs iOS/Android (menus, headers, search bars).

### Récapitulatif des priorités
| Domaine | Skill qui fait autorité |
|---|---|
| Composants UI (Box, Text, Button…) | **gluestack-ui-v4/components** |
| Styling (couleurs, spacing, dark mode) | **gluestack-ui-v4/styling** |
| Variants de composants | **gluestack-ui-v4/variants** |
| Icônes | **gluestack-ui-v4** (lucide) |
| Navigation, routes, stacks, tabs | **building-native-ui** |
| Animations Reanimated | **building-native-ui** |
| Patterns natifs iOS/Android | **building-native-ui** |
| Safe areas, responsiveness | **building-native-ui** |
| Setup Tailwind/NativeWind | **expo-tailwind-setup** |
| Data fetching, React Query, offline | **native-data-fetching** |
| Performance (FPS, TTI, bundle) | **react-native-best-practices** (Callstack) |
| CI/CD EAS workflows | **expo-cicd-workflows** |
| Dev clients, TestFlight interne | **expo-dev-client** |
| Release stores | **expo-deployment** |
| Upgrade SDK | **upgrading-expo** |
| API routes côté Expo (si besoin) | **expo-api-routes** |
| Modules natifs custom | **expo-module** |
| Composants DOM / webview | **use-dom** |
