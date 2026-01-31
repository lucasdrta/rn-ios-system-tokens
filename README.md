# @lucasdrta/rn-ios-system-tokens

Access native iOS System Fonts (Dynamic Type) and System Colors (semantic colors) in React Native with Expo. 

This library allows your app to respect the user's system-wide font size settings (accessibility) and appearance preferences (Dark/Light mode) by exposing the raw values from UIKit.

## Features

- 🔠 **Dynamic Type Support**: Get the exact font metrics (size, weight, line height, letter spacing) for standard iOS text styles (e.g., `Body`, `Headline`, `Title1`).
- 🎨 **Semantic Colors**: Access iOS system colors (e.g., `systemBackground`, `label`, `systemBlue`) that automatically adapt to Light and Dark modes.
- ⚡ **Reactive Hooks**: `useSystemFonts` and `useSystemColors` hooks that automatically update when system settings change.
- 📱 **Native Performance**: Uses `expo-modules-core` for efficient native bridging.

## Installation

```bash
npm install @lucasdrta/rn-ios-system-tokens
```

or

```bash
yarn add @lucasdrta/rn-ios-system-tokens
```

## Usage

### 1. Using Hooks (Recommended)

The hooks are the easiest way to use the library as they automatically trigger a re-render when system settings change (e.g., user changes font size in Settings or toggles Dark Mode).

#### Hooks Example

```tsx
import React from 'react';
import { Text, View, StyleSheet } from 'react-native';
import { useSystemFonts, useSystemColors } from '@lucasdrta/rn-ios-system-tokens';

export default function App() {
  const fonts = useSystemFonts();
  const colors = useSystemColors();

  if (!fonts || !colors) return null; // Wait for initial load

  return (
    <View style={[styles.container, { backgroundColor: colors.systemBackground }]}>
      <Text style={{ 
        color: colors.label,
        fontSize: fonts.largeTitle.fontSize,
        fontWeight: fonts.largeTitle.fontWeight,
        lineHeight: fonts.largeTitle.lineHeight,
      }}>
        Large Title
      </Text>
      
      <Text style={{ 
        color: colors.secondaryLabel,
        fontSize: fonts.body.fontSize,
        fontWeight: fonts.body.fontWeight, 
        marginTop: 10
      }}>
        This text respects the user's dynamic type settings. 
        Try changing the text size in iOS Settings > Display & Brightness > Text Size.
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    justifyContent: 'center',
  },
});
```

### 2. Imperative API

You can also fetch the values once if you don't need them to be reactive or want to use them outside of a component.

```ts
import { getSystemFonts, getSystemColors } from '@lucasdrta/rn-ios-system-tokens';

// Get current values
const fonts = getSystemFonts();
const colors = getSystemColors();

console.log(fonts.body.fontSize); // e.g., 17
console.log(colors.systemBlue);   // e.g., "#007AFF"
```

## API Reference

### `useSystemFonts()`

Returns `SystemFonts | null`. Hook that subscribes to `UIContentSizeCategoryDidChangeNotification`.

### `useSystemColors()`

Returns `SystemColors | null`. Hook that detects appearance changes.

### Types

#### `SystemFonts`
An object containing metrics for all Apple [Human Interface Guidelines text styles](https://developer.apple.com/design/human-interface-guidelines/typography#iOS-iPadOS-Dynamic-Type-sizes):
- `largeTitle`
- `title1`, `title2`, `title3`
- `headline`, `subheadline`
- `body`, `callout`
- `caption1`, `caption2`
- `footnote`

Each style contains:
- `fontSize`: number
- `fontWeight`: 'regular' | 'bold' | 'semibold' etc.
- `lineHeight`: number
- `letterSpacing`: number

#### `SystemColors`
Contains standard iOS semantic colors.
- **Text**: `label`, `secondaryLabel`, `tertiaryLabel`, `link`, etc.
- **Backgrounds**: `systemBackground`, `secondarySystemBackground`, `systemGroupedBackground`, etc.
- **Fills**: `systemFill`, `secondarySystemFill`, etc.
- **Standard**: `systemBlue`, `systemRed`, `systemGreen`, etc.

## Requirements

- iOS 13.0+
- Expo Modules (this library is an Expo Module)

## License

MIT
