# Skills Inventory — Beertrack Front

Inventaire des skills installés dans `.claude/skills/`. Pour la politique de résolution des conflits, voir le `CLAUDE.md` à la racine du repo.

| Skill | Source | Couvre | S'active quand | Lignes |
|---|---|---|---|---|
| building-native-ui | [expo/skills](https://github.com/expo/skills) | Expo Router, navigation, stacks, NativeTabs, animations Reanimated, form sheets, context menus, link previews, modals, patterns natifs iOS/Android | Construction d'écrans, navigation, headers, animations, interactions natives | 321 |
| expo-api-routes | [expo/skills](https://github.com/expo/skills) | API routes Expo Router + EAS Hosting | Création de routes serveur côté Expo (alternative au backend NestJS) | ~ |
| expo-cicd-workflows | [expo/skills](https://github.com/expo/skills) | Fichiers YAML `.eas/workflows/`, pipelines EAS | Configuration CI/CD, builds automatisés | ~ |
| expo-deployment | [expo/skills](https://github.com/expo/skills) | iOS App Store, Play Store, web hosting | Release de l'app en production | ~ |
| expo-dev-client | [expo/skills](https://github.com/expo/skills) | Custom dev clients, distribution locale, TestFlight interne | Build custom (modules natifs, extensions Apple, …) | ~ |
| expo-module | [expo/skills](https://github.com/expo/skills) | Expo Modules API (Swift, Kotlin, TS), config plugins, autolinking | Écriture de modules natifs custom | ~ |
| expo-tailwind-setup | [expo/skills](https://github.com/expo/skills) | Install Tailwind v4 + NativeWind v5 + react-native-css | Setup initial du styling (une seule fois) | 480 |
| expo-ui-jetpack-compose | [expo/skills](https://github.com/expo/skills) | `@expo/ui/jetpack-compose` — Jetpack Compose dans Expo (SDK 55) | Vues Android natives via Compose | ~ |
| expo-ui-swiftui | [expo/skills](https://github.com/expo/skills) | `@expo/ui/swift-ui` — SwiftUI dans Expo (SDK 55) | Vues iOS natives via SwiftUI | ~ |
| gluestack-ui-v4 | [gluestack/agent-skills](https://github.com/gluestack/agent-skills) | Meta-skill Gluestack v4 (overview + orientation vers les sub-skills) | Tout travail UI | 199 |
| gluestack-ui-v4/setup | gluestack/agent-skills | Installation + config initiale de Gluestack | `npx gluestack-ui init`, provider setup | ~ |
| gluestack-ui-v4/components | gluestack/agent-skills | Usage de Box, Text, Button, Input, ScrollView… (copy-paste `@/components/ui/*`) | Construction de composants UI | 757 |
| gluestack-ui-v4/styling | gluestack/agent-skills | Semantic tokens v4, dark mode, spacing, tva, className merging | Toute décision de style (couleur, espacement, typo) | 698 |
| gluestack-ui-v4/variants | gluestack/agent-skills | Variants de composants via `tva`, composition | Création de variantes (size, intent, …) | ~ |
| gluestack-ui-v4/creating-components | gluestack/agent-skills | Patterns pour créer ses propres composants Gluestack | Composants custom réutilisables | ~ |
| gluestack-ui-v4/performance | gluestack/agent-skills | Optimisations spécifiques à Gluestack (re-renders, memo) | Perf d'un écran Gluestack | ~ |
| gluestack-ui-v4/validation | gluestack/agent-skills | Patterns de validation de forms avec Gluestack | Formulaires | ~ |
| gluestack-ui-v4/migrate-to-v5 | gluestack/agent-skills | Migration v4 → v5 | Quand v5 sortira | ~ |
| native-data-fetching | [expo/skills](https://github.com/expo/skills) | fetch, React Query, SWR, caching, offline, `useLoaderData` Expo Router | Tout appel réseau / API / data loader | ~ |
| react-native-best-practices | [callstackincubator/agent-skills](https://github.com/callstackincubator/agent-skills) | Perf FPS/TTI, Hermes, bundle size, re-renders, FlashList, bridge | Optim de performance, debug de jank, mesure | 253 |
| upgrading-expo | [expo/skills](https://github.com/expo/skills) | Upgrade Expo SDK, fix deps | Migration SDK (annuelle) | ~ |
| use-dom | [expo/skills](https://github.com/expo/skills) | Expo DOM components — code web dans webview native | Migration incrémentale de code web | ~ |

**Total : 14 skills top-level + 8 sub-skills Gluestack = 22 skills actifs**

## Conflits identifiés
Voir la section "Résolution des conflits entre skills" dans le `CLAUDE.md` à la racine du repo. TL;DR :
- **Styling** : Gluestack gagne (NativeWind + semantic tokens)
- **Primitives UI** : Gluestack gagne (`Box`, `Text`, `Pressable` depuis `@/components/ui/*`)
- **Thème / couleurs** : Gluestack gagne (semantic tokens, pas de `PlatformColor`)
- **Icônes** : Gluestack/lucide pour l'UI, SF Symbols pour headers natifs gérés par Expo Router
- **Navigation / architecture / patterns natifs** : Expo `building-native-ui` gagne
