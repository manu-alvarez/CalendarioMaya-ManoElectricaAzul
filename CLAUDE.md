# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## REGLAS PRINCIPALES

1. **IDIOMA:** Siempre usar **castellano (español)** como idioma principal en todo: comentarios, mensajes de error, textos de la interfaz, documentación y comunicación con el usuario.

2. **DATOS REALES:** Esta aplicación está en **PRODUCCIÓN**. Nunca usar datos falsos, demos, mocks, placeholders o datos inventados. Siempre utilizar datos reales y auténticos.

## Project Overview

**Calendario Maya - Mano Electrica Azul** is a React Native mobile application built with Expo that displays the Mayan sacred calendar (Tzolkin). It converts Gregorian dates to Mayan Kin (260-day cycle), shows the corresponding seal and tone, and displays daily reflections in Spanish.

- **Framework:** Expo SDK 53, React 19, React Native 0.79.5
- **Platforms:** Android, iOS, and Web
- **Working Directory:** All development happens in the `app/` folder
- **Entry Point:** `app/App.js`

## Development Commands

```bash
# From the app/ directory
npm install              # Install dependencies
npx expo start           # Start development server (all platforms)
npm run android          # Start for Android only
npm run ios              # Start for iOS only
npm run web              # Start for web only
npm run lint             # Run ESLint
```

## Architecture

### Mayan Calendar Logic (`src/utils/kin.ts`)

The core calendar calculation is in `kinFromDate()`. Important details:

- **Base date:** January 1, 1910 = Kin 63
- **Cycle:** 260 days (20 seals x 13 tones)
- **February 29th handling:** Treated as "Hunab Ku" (Kin 0), excluded from the 260-day cycle calculation
- **Date handling:** All calculations use UTC dates to avoid timezone issues
- **Returns:** `{ num, sello, tono }` where num is 1-260, sello is 1-20, tono is 1-13

The `diasSinHunabKu()` function counts days between two dates while excluding February 29th occurrences.

### Main Application (`App.js`)

The entire UI is implemented in a single component file (~46KB). Key patterns:

- Uses local midnight dates for state (`fechaSeleccionada`) to avoid timezone shifts
- Converts local dates to UTC before calling `kinFromDate()`
- Displays Spanish date formatting using `toLocaleDateString('es-ES', ...)`
- Seal images are hosted remotely on GitHub (manu-alvarez/semilla-resonante repository)

### Data Files

- `reflexiones_completas.json`: 260 daily reflections, indexed by Kin number (1-260)
- `src/utils/selloData.js`: Seal names and image URLs

## Important Implementation Notes

- **Timezone handling:** The app uses a two-step process: local date → UTC date → Kin calculation. This ensures consistent results regardless of user timezone.
- **Date input format:** Manual date entry expects `YYYY-MM-DD` format.
- **Language:** All user-facing text is in Spanish.
- **Navigation:** Previous/next day buttons and "Today" quick access for date navigation.

## Build Configuration

- **EAS Build:** Configured in `eas.json` for production Android (AAB) and iOS builds
- **Docker:** Multi-stage Node.js 20 build, exposed on port 8081
- **Cloud Build:** Automated Docker image pushes to `gcr.io/$PROJECT_ID/calendario-maya`
