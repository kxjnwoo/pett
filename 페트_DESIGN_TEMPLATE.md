# Design Template - Paper Trading App "페트"

This document details the design system and UI/UX patterns of the project. Provide this template to AI to create various apps with a consistent design language.

---

## Design Philosophy

**Core Concept**: Modern Financial App with Glassmorphism
- Clean and professional Securitie Trading app design
- Modern feel using glassmorphism effects
- Smooth animations and interactions
- Balance between information density and readability
- Mobile-first design

---

## Color System

### Primary Colors
```typescript
primary: '#29CC90'        // Mint green - main actions, emphasis
secondary: '#16181F'      // Dark gray - text, icons
background: '#F9FAFB'     // Light gray - background
```

### Semantic Colors
```typescript
success: '#00C853'        // Buy, rise, positive change
error: '#FF3B30'          // Sell, fall, negative change
warning: '#FF9500'        // Warning, caution
```

### Text Colors (Hierarchy)
```typescript
textPrimary: '#16181F'    // Primary text (titles, important info)
textSecondary: '#6B7280'  // Secondary text (descriptions, labels)
textTertiary: '#9CA3AF'   // Tertiary text (meta info, timestamps)
```

### UI Elements
```typescript
cardBackground: 'rgba(255, 255, 255, 0.8)'  // Semi-transparent card background (glassmorphism)
border: '#E5E7EB'         // Borders
divider: '#F3F4F6'        // Dividers, inactive backgrounds
```

### Chart Colors (Data Visualization)
```typescript
chartColors: [
  '#29CC90',  // Primary
  '#3B82F6',  // Blue
  '#8B5CF6',  // Purple
  '#F59E0B',  // Amber
  '#EF4444'   // Red
]
```

---

## Typography System

### Font Families
```typescript
primary: 'System'         // System default font (San Francisco, Roboto)
mono: 'Courier'           // Monospace for numbers, amounts
```

### Font Sizes (Hierarchical Scale)
```typescript
headline: 28px    // Page titles, large numbers
title: 24px       // Section titles
subtitle: 20px    // Sub-section titles
body: 16px        // Body text, default text
caption: 14px     // Captions, labels
small: 12px       // Small text, meta info
tiny: 10px        // Very small text
```

### Font Weights
```typescript
regular: '400'    // Normal text
medium: '500'     // Slight emphasis
semibold: '600'   // Medium emphasis (buttons, labels)
bold: '700'       // Strong emphasis (titles, important info)
```

### Typography Usage Patterns
- **Amounts/Numbers**: `fontFamily: fonts.mono` + `fontWeight: semibold/bold`
- **Titles**: `fontSize: headline/title` + `fontWeight: bold`
- **Labels**: `fontSize: caption` + `fontWeight: semibold` + `color: textSecondary`
- **Body**: `fontSize: body` + `fontWeight: regular` + `color: textPrimary`

---

## Layout System

### Spacing Units (4px-based system)
```typescript
baseUnit: 4px         // Base unit
gutter: 16px          // Spacing between elements
pageMargin: 20px      // Page horizontal margins
cardPadding: 16px     // Card internal padding
borderRadius: 12px    // Default border radius
```

### Spacing Scale
- `4px` (1 unit): Very small spacing
- `8px` (2 units): Small spacing
- `12px` (3 units): Medium spacing
- `16px` (4 units): Default spacing (gutter)
- `20px` (5 units): Large spacing (pageMargin)
- `24px` (6 units): Section spacing

---

## Animation System

### Duration (milliseconds)
```typescript
pageTransition: 300ms   // Page transitions
buttonPress: 100ms      // Button press animation
cardHover: 200ms        // Card hover/press animation
```

### Animation Patterns

#### 1. Button Press Animation
```typescript
// Scale down to 0.95 on press
Animated.timing(scaleAnim, {
  toValue: 0.95,
  duration: 100,
  useNativeDriver: true,
})

// Return to 1.0 on release
Animated.timing(scaleAnim, {
  toValue: 1,
  duration: 100,
  useNativeDriver: true,
})
```

#### 2. Card Press Animation
```typescript
// Parallel animations: scale + shadow
Animated.parallel([
  Animated.timing(scaleAnim, {
    toValue: 0.95,
    duration: 100,
    useNativeDriver: true,
  }),
  Animated.timing(shadowAnim, {
    toValue: 1,
    duration: 200,
    useNativeDriver: false,
  }),
])
```

#### 3. Tab Bar Slider Animation
```typescript
// Spring animation for smooth sliding
Animated.spring(slideAnim, {
  toValue: targetIndex,
  useNativeDriver: false,
  damping: 20,        // Smooth deceleration
  stiffness: 200,     // Fast response
})
```

---

## Card Component Design

### Basic Card Style
```typescript
{
  backgroundColor: 'rgba(255, 255, 255, 0.8)',  // Semi-transparent (glassmorphism)
  borderRadius: 12,
  padding: 16,
  
  // Shadow effect (using Primary color)
  shadowColor: '#29CC90',
  shadowOffset: { width: 0, height: 10 },
  shadowOpacity: 0.2,
  shadowRadius: 30,
  elevation: 5,  // Android
}
```

### Card Variants

#### 1. Pressable Card (Interactive)
- Scale to `0.95` on touch
- Increased shadow intensity (hover state)
- Use cases: Clickable items (stock cards, transaction history)

#### 2. Static Card (Information Display)
- No animation
- Default shadow only
- Use cases: Charts, statistics

#### 3. Table Card (Data Table)
```typescript
{
  padding: 0,  // Manage individual padding internally
  
  // Header style
  header: {
    backgroundColor: colors.divider,
    borderTopLeftRadius: 12,
    borderTopRightRadius: 12,
    paddingHorizontal: 16,
    paddingVertical: 12,
  },
  
  // Row divider
  rowBorder: {
    borderBottomWidth: 1,
    borderBottomColor: colors.divider,
  }
}
```

---

## Button Component Design

### Button Variants

#### 1. Primary Button (Default)
```typescript
{
  backgroundColor: colors.primary,
  paddingVertical: 14,
  paddingHorizontal: 24,
  borderRadius: 12,
  
  text: {
    color: '#FFFFFF',
    fontSize: 16,
    fontWeight: '600',
  }
}
```

#### 2. Success Button (Buy)
```typescript
{
  backgroundColor: colors.success,
  // Rest same as primary
}
```

#### 3. Error Button (Sell)
```typescript
{
  backgroundColor: colors.error,
  // Rest same as primary
}
```

#### 4. Outline Button (Secondary Action)
```typescript
{
  backgroundColor: 'transparent',
  borderWidth: 2,
  borderColor: colors.primary,
  
  text: {
    color: colors.primary,
    fontSize: 16,
    fontWeight: '600',
  }
}
```

### Button States

#### Disabled State
```typescript
{
  opacity: 0.5,
  // Touch events disabled
}
```

#### Press Animation
- Scale: `1.0 → 0.95 → 1.0`
- Duration: `100ms`
- Uses native driver

---

## Custom Tab Bar Design

### Core Design Features
1. **Floating Tab Bar**: 20px above bottom of screen
2. **Glassmorphism**: Semi-transparent background + blur effect
3. **Sliding Indicator**: Smoothly follows selected tab
4. **Round Design**: Fully rounded corners (borderRadius: 50)

### Structure
```typescript
Container (absolute positioning)
  └─ BlurView (iOS/Web) / View (Android)
      ├─ Animated Slider (selection indicator)
      └─ Tab Buttons (icons)
```

### Style Details

#### Container
```typescript
{
  position: 'absolute',
  bottom: 20,              // 20px above bottom of screen
  left: 0,
  right: 0,
  alignItems: 'center',
  paddingHorizontal: 20,
}
```

#### Tab Bar Container
```typescript
{
  flexDirection: 'row',
  height: 64,
  borderRadius: 50,        // Fully rounded corners
  paddingHorizontal: 8,
  paddingVertical: 8,
  overflow: 'hidden',
  
  // Platform-specific background
  backgroundColor: Platform.select({
    android: 'rgba(255, 255, 255, 0.85)',
    default: 'rgba(255, 255, 255, 0.3)',  // iOS/Web (uses BlurView)
  }),
  
  // Platform-specific border
  borderWidth: Platform.OS !== 'android' ? 1 : 0,
  borderColor: 'rgba(255, 255, 255, 0.5)',
  
  // Shadow (Primary color)
  shadowColor: 'rgba(41, 204, 144, 0.3)',
  shadowOffset: { width: 0, height: 10 },
  shadowOpacity: 1,
  shadowRadius: 30,
  elevation: 10,  // Android
}
```

#### Slider (Selection Indicator)
```typescript
{
  position: 'absolute',
  top: 8,
  bottom: 8,
  left: 8,
  width: tabWidth,         // Dynamically calculated
  backgroundColor: colors.primary,
  borderRadius: 50,
  
  // Shadow
  shadowColor: colors.primary,
  shadowOffset: { width: 0, height: 2 },
  shadowOpacity: 0.3,
  shadowRadius: 8,
  elevation: 4,
  
  // Animation
  transform: [{ translateX: animatedValue }],
}
```

#### Tab Button
```typescript
{
  flex: 1,
  justifyContent: 'center',
  alignItems: 'center',
  zIndex: 1,  // Display above slider
}
```

### Icon Colors
```typescript
// Selected tab
iconColor: colors.secondary  // Dark color (contrast on slider)

// Unselected tab
iconColor: colors.textTertiary  // Light gray
```

### Animation Behavior
```typescript
// Spring animation for smooth sliding
Animated.spring(slideAnim, {
  toValue: selectedIndex,
  useNativeDriver: false,  // translateX is a layout property
  damping: 20,             // Deceleration rate (higher = stops faster)
  stiffness: 200,          // Spring strength (higher = faster response)
})
```

### Platform Differences

#### iOS & Web
- Uses `BlurView` (intensity: 60, tint: 'light')
- Semi-transparent background: `rgba(255, 255, 255, 0.3)`
- Has border
- `backdropFilter: 'blur(20px)'` (Web)

#### Android
- Uses regular `View` (BlurView performance issues)
- Opaque background: `rgba(255, 255, 255, 0.85)`
- No border
- Uses `elevation: 10`

---

#### Usage Patterns
- Portfolio performance trends
- Stock price changes
- Height: 180-200px

### Donut Chart (Asset Allocation)

#### Style
```typescript
{
  backgroundColor: 'transparent',
  hasLegend: true,
  
  // Legend style
  legendFontColor: colors.textSecondary,
  legendFontSize: 12,
  
  // Show absolute values
  absolute: true,
}
```

#### Color Usage
- Select from `chartColors` array by category
- Assign unique color to each section

---

## Glassmorphism Effects

### Application Areas
1. **Card Components**: Semi-transparent background
2. **Tab Bar**: BlurView + semi-transparent background
3. **Modal Overlays**: Background blur effect

### Implementation Methods

#### iOS & Web
```typescript
// Using BlurView
<BlurView intensity={60} tint="light">
  {children}
</BlurView>

// CSS (Web)
{
  backdropFilter: 'blur(20px)',
  backgroundColor: 'rgba(255, 255, 255, 0.3)',
}
```

#### Android
```typescript
// Replace with semi-transparent background
{
  backgroundColor: 'rgba(255, 255, 255, 0.85)',
}
```

### Layering
```
Background
  └─ Blur Layer
      └─ Semi-transparent Background
          └─ Content
```

---

## Shadow System

### Default Shadow (Basic Card)
```typescript
{
  shadowColor: '#29CC90',              // Uses Primary color
  shadowOffset: { width: 0, height: 10 },
  shadowOpacity: 0.2,
  shadowRadius: 30,
  elevation: 5,  // Android
}
```

### Hover/Active Shadow (On Interaction)
```typescript
{
  shadowColor: '#29CC90',
  shadowOffset: { width: 0, height: 14 },  // Lower
  shadowOpacity: 0.25,                      // Darker
  shadowRadius: 40,                         // Wider
  elevation: 8,  // Android
}
```

### Tab Bar Shadow
```typescript
{
  shadowColor: 'rgba(41, 204, 144, 0.3)',  // Semi-transparent Primary
  shadowOffset: { width: 0, height: 10 },
  shadowOpacity: 1,
  shadowRadius: 30,
  elevation: 10,  // Android
}
```

### Shadow Usage Principles
1. **Color**: Based on Primary color (brand consistency)
2. **Direction**: Always downward (height > 0)
3. **Intensity**: Changes based on interaction state
4. **Platform**: iOS/Web uses shadow, Android uses elevation

---

## Interactive States

### Pressable Elements

#### Default State
```typescript
{
  scale: 1.0,
  opacity: 1.0,
  shadow: default,
}
```

#### Press State
```typescript
{
  scale: 0.95,
  opacity: 1.0,
  shadow: hover,
}
```

#### Disabled State
```typescript
{
  scale: 1.0,
  opacity: 0.5,
  shadow: default,
}
```

### Color States (Financial Data)

#### Positive (Rise/Profit)
```typescript
{
  color: colors.success,
  icon: TrendingUp,
  prefix: '+',
}
```

#### Negative (Fall/Loss)
```typescript
{
  color: colors.error,
  icon: TrendingDown,
  prefix: '',  // Minus sign is automatic
}
```

#### Neutral
```typescript
{
  color: colors.textPrimary,
  icon: null,
  prefix: '',
}
```

---

## Screen Layout Patterns

### 1. Dashboard Layout (Home Screen)

```
┌─────────────────────────────────┐
│ Header (name)    Profile button │
├─────────────────────────────────┤
│ Balance Card (Large card)       │
│  - Total assets                 │
│  - Return rate (icon + color)   │
├─────────────────────────────────┤
│ Chart Card                      │
│  - Portfolio performance chart  │
│  - Compare to S&P 500           │
├─────────────────────────────────┤
│ Holdings Grid Card              │
│  ┌───────────────────────────┐  │
│  │ (logo) Ticker1            │  │
│  │───────────────────────────│  │
│  │ (logo) Ticker2            │  │
│  │───────────────────────────│  │
│  │ (logo) Ticker3            │  │
│  └───────────────────────────┘  │
├─────────────────────────────────┤
│ Recent Activity (List)          │
│  - Transaction 1                │
│  - Transaction 2                │
│  - Transaction 3                │
└─────────────────────────────────┘
```

### 2. Portfolio Layout

```
┌─────────────────────────────────┐
│ Header (Title + Logout)         │
│ Total Value (Large number)      │
│ Profit/Loss (Value + P/L Rate   │
├─────────────────────────────────┤
│ Donut Chart Card                │
│  - Asset allocation             │
├─────────────────────────────────┤
│ Performance Card                │
│  - Interval (1D/1W/3M/1Y/5Y)    │
│  - Line Chart                   │
├─────────────────────────────────┤
│ Holdings Table                  │
│  - Header Row                   │
│  - Data Rows                    │
└─────────────────────────────────┘
```

### 3. Trading Layout

```
┌─────────────────────────────────┐
│ Header (Title)                  │
├─────────────────────────────────┤
│ Stock Selector Card             │
│  - Logo + Name                  │
│  - Price + Change               │ 
│  - Symbol Search Button         │
├─────────────────────────────────┤
│ Order Book Card                 │
│  ┌──────────┬──────────┐        │
│  │     [Ask Price]     │        │
│  │     [Bid Price]     │        │
│  └──────────┴──────────┘        │
├─────────────────────────────────┤
│ Order Form Card                 │
│  - Buy/Sell Toggle              │
│  - Market/Limit Toggle          │
│  - Quantity Input               │
│  - Price Input (if limit)       │
│  - Summary                      │
│  - Average Executed Price       │
│  - Submit Button                │
└─────────────────────────────────┘
```

### 4. Stock Detail Layout

```
┌─────────────────────────────────┐
│ Header                          │
│  - Logo                         │
│  - Ticker + Name                │
│  - Price + Change               │
├─────────────────────────────────┤
│ Chart Card                      │
│  - Price Chart                  │
│  - Interval (1D/1W/3M/1Y/5Y)    │
├─────────────────────────────────┤
│ Stats Card                      │
│  - Key Metrics Grid (2 columns) │
└─────────────────────────────────┘
```

---

## Component Patterns

### 1. Stock Logo Component

```typescript
// Circular background + ticker text
{
  width: size,
  height: size,
  borderRadius: size / 2,
  backgroundColor: colors.primary + '20',  // 20% transparency
  justifyContent: 'center',
  alignItems: 'center',
  
  text: {
    fontSize: size / 2.5,
    fontWeight: 'bold',
    color: colors.primary,
  }
}
```

### 2. Badge Component (Buy/Sell Indicator)

```typescript
{
  paddingHorizontal: 6,
  paddingVertical: 2,
  borderRadius: 4,
  backgroundColor: color + '20',  // 20% transparency
  
  text: {
    fontSize: 12,
    fontWeight: '600',
    color: color,
  }
}
```

### 3. Input Field

```typescript
{
  backgroundColor: colors.divider,
  borderRadius: 8,
  paddingHorizontal: 16,
  paddingVertical: 12,
  fontSize: 16,
  color: colors.textPrimary,
  fontFamily: fonts.mono,  // For number input
}
```

### 4. Toggle Selector (Period, Order Type)

```typescript
// Container
{
  flexDirection: 'row',
  gap: 8,
}

// Button (Inactive)
{
  flex: 1,
  paddingVertical: 8-12,
  borderRadius: 8,
  backgroundColor: colors.divider,
  alignItems: 'center',
  
  text: {
    fontSize: 14,
    fontWeight: '600',
    color: colors.textSecondary,
  }
}

// Button (Active)
{
  backgroundColor: colors.primary,
  
  text: {
    color: '#FFFFFF',
  }
}
```

### 5. List Item (Transaction, Stock)

```typescript
{
  flexDirection: 'row',
  alignItems: 'center',
  padding: 12,
  gap: 12,
  
  // Divider (except last)
  borderBottomWidth: 1,
  borderBottomColor: colors.divider,
}

// Left Section
{
  flexDirection: 'row',
  alignItems: 'center',
  flex: 1,
  gap: 12,
}

// Right Section
{
  alignItems: 'flex-end',
}
```

---

## Modal Design

### Full Screen Modal (Stock Picker)

```typescript
// Modal Props
{
  animationType: 'slide',
  presentationStyle: 'pageSheet',
}

// Container
{
  flex: 1,
  backgroundColor: colors.background,
  paddingTop: insets.top,
}

// Header
{
  flexDirection: 'row',
  justifyContent: 'space-between',
  alignItems: 'center',
  paddingHorizontal: 20,
  paddingVertical: 16,
  borderBottomWidth: 1,
  borderBottomColor: colors.border,
}

// Search Bar
{
  flexDirection: 'row',
  alignItems: 'center',
  backgroundColor: colors.divider,
  borderRadius: 12,
  paddingHorizontal: 16,
  paddingVertical: 12,
  margin: 20,
  gap: 12,
}
```

---

## Grid System

### 2-Column Grid (Holdings)
```typescript
{
  flexDirection: 'row',
  flexWrap: 'wrap',
  gap: 16,
}

// Item
{
  width: (screenWidth - pageMargin * 2 - gutter) / 2,
}
```

### Stats Grid (Key Metrics)
```typescript
{
  flexDirection: 'row',
  flexWrap: 'wrap',
  gap: 16,
}

// Item
{
  width: '47%',  // Slight margin
  marginBottom: 12,
}
```

---

## Data Visualization Patterns

### 1. Price Display
```typescript
// Large price display
{
  fontSize: 28-36,
  fontWeight: 'bold',
  fontFamily: fonts.mono,
  color: colors.textPrimary,
}

// Change display
{
  flexDirection: 'row',
  alignItems: 'center',
  gap: 4-6,
}

// Icon + text
{
  icon: TrendingUp/TrendingDown,
  iconSize: 16-20,
  iconColor: colors.success/error,
  
  text: {
    fontSize: 14-16,
    fontWeight: '600',
    fontFamily: fonts.mono,
    color: colors.success/error,
  }
}
```

### 2. Table Display
```typescript
// Header
{
  flexDirection: 'row',
  backgroundColor: colors.divider,
  paddingHorizontal: 16,
  paddingVertical: 12,
  borderTopLeftRadius: 12,
  borderTopRightRadius: 12,
}

// Row
{
  flexDirection: 'row',
  paddingHorizontal: 16,
  paddingVertical: 12,
  alignItems: 'center',
  borderBottomWidth: 1,
  borderBottomColor: colors.divider,
}

// Cell Alignment
{
  flex: columnWidth,
  textAlign: 'left' | 'right' | 'center',
}
```

### 3. Order Book Display
```typescript
// 2-Column Layout
{
  flexDirection: 'row',
  gap: 12,
}

// Column
{
  flex: 1,
}

// Row
{
  flexDirection: 'row',
  justifyContent: 'space-between',
  paddingVertical: 4,
}

// Price (Color-coded)
{
  fontSize: 12,
  fontWeight: '600',
  fontFamily: fonts.mono,
  color: colors.success/error,  // Buy/Sell
}

// Quantity
{
  fontSize: 12,
  fontFamily: fonts.mono,
  color: colors.textSecondary,
}
```

---

## Icon Usage

### Icon Library
- Uses **lucide-react-native**
- Consistent style and size

### Icon Sizes
```typescript
small: 16px      // Inline icons
medium: 20px     // Button, tab icons
large: 24px      // Header icons
xlarge: 32px+    // Logo, large icons
```

### Icon Colors
```typescript
primary: colors.primary        // Emphasis, active state
secondary: colors.secondary    // Selected tab
tertiary: colors.textTertiary  // Inactive state
success: colors.success        // Positive action
error: colors.error            // Negative action
```

### Common Icons
- `TrendingUp` / `TrendingDown`: Price changes
- `Search`: Search
- `ChevronDown` / `ChevronRight`: Navigation
- `X`: Close
- `LogOut`: Logout

---

## Safe Area Handling

### Principles
1. **ScrollView**: Add `paddingTop: insets.top + extra` to `contentContainerStyle`
2. **Fixed Elements**: Apply insets to `top/bottom`
3. **Tab Bar**: Automatically handles bottom inset (absolute positioning)

### Patterns
```typescript
// ScrollView
<ScrollView
  contentContainerStyle={[
    styles.content,
    { paddingTop: insets.top + 20 }
  ]}
>
  {children}
</ScrollView>

// Modal
<View style={[styles.modal, { paddingTop: insets.top }]}>
  {children}
</View>
```

---

## Loading & Error States

### Loading State
```typescript
<View style={[styles.container, styles.centerContent]}>
  <ActivityIndicator size="large" color={colors.primary} />
</View>
```

### Error State
```typescript
<View style={[styles.container, styles.centerContent]}>
  <Text style={styles.errorText}>
    {errorMessage}
  </Text>
</View>
```

### Empty State
```typescript
<View style={styles.emptyState}>
  <Text style={styles.emptyTitle}>No data available</Text>
  <Text style={styles.emptyDescription}>
    Description text
  </Text>
</View>
```

---

## Accessibility

### Touch Targets
- Minimum size: `44x44px` (iOS HIG)
- Button padding: `paddingVertical: 12-14px`

### Text Contrast
- Primary text: High contrast (textPrimary on background)
- Secondary text: Medium contrast (textSecondary on background)
- Disabled: Low contrast (opacity: 0.5)

### Accessibility Props
```typescript
<Pressable
  accessibilityRole="button"
  accessibilityLabel="Description"
  accessibilityState={{ selected: isSelected }}
>
  {children}
</Pressable>
```

---

## Platform-Specific Adaptations

### iOS
- Uses BlurView
- System font (San Francisco)
- Uses Shadow properties

### Android
- Replace with semi-transparent background
- System font (Roboto)
- Uses Elevation

### Web
- Uses BlurView
- CSS backdrop-filter
- Uses Box-shadow
- Can add hover states

### Conditional Styling
```typescript
Platform.select({
  ios: { /* iOS styles */ },
  android: { /* Android styles */ },
  web: { /* Web styles */ },
  default: { /* Default styles */ },
})
```

---

## Code Style Guidelines

### StyleSheet Structure
```typescript
const styles = StyleSheet.create({
  // 1. Container/Layout
  container: { },
  content: { },
  
  // 2. Sections
  header: { },
  body: { },
  footer: { },
  
  // 3. Components
  card: { },
  button: { },
  
  // 4. Text
  title: { },
  subtitle: { },
  body: { },
  
  // 5. States
  active: { },
  disabled: { },
});
```

### Naming Conventions
- **Container**: `container`, `wrapper`, `content`
- **Layout**: `row`, `column`, `grid`, `flex`
- **Components**: `card`, `button`, `input`, `modal`
- **Text**: `title`, `subtitle`, `body`, `caption`, `label`
- **States**: `active`, `inactive`, `disabled`, `selected`
- **Positions**: `left`, `right`, `top`, `bottom`, `center`

---

## Design Checklist

When creating new screens/components, check:

### Visual
- [ ] Do colors follow the design system?
- [ ] Is typography consistent?
- [ ] Does spacing follow the 4px-based system?
- [ ] Are shadows properly applied?
- [ ] Is glassmorphism effect applied where needed?

### Interaction
- [ ] Do touchable elements have animations?
- [ ] Are button sizes at least 44x44px?
- [ ] Are loading/error states handled?
- [ ] Is feedback immediate?

### Layout
- [ ] Is Safe Area considered?
- [ ] Is ScrollView used where scrolling is needed?
- [ ] Is layout responsive?
- [ ] Are platform differences considered?

### Code
- [ ] Using StyleSheet?
- [ ] Reusing constants? (colors, fonts, layout)
- [ ] Is naming consistent?
- [ ] Are components reusable?

---

## Design Evolution

### Current Version: PDS v1.0 (페트 디자인 시스템, PDS)
- Securitie Trading app design system
- Glassmorphism effects
- Primary color-based shadows
- Floating tab bar

### Future Improvements
- Dark mode support
- For the "Stock Detail" Page, Use the TradingView Lightweight Charts™ Library for the price chart.
- Use React-financial-charts Library for all other charts needed in the app

---

## Important
- Each feature should be implemented based on a deep analysis of the API documentation

### URLs of API documentation
- Finnhub API documentation: "https://finnhub.io/docs/api"
- TradingView Lightweight Charts™ Library API Documentation: "https://tradingview.github.io/lightweight-charts/"
1. Getting started: "https://tradingview.github.io/lightweight-charts/docs"
2. Tutorials: "https://tradingview.github.io/lightweight-charts/tutorials"
3. API reference: "https://tradingview.github.io/lightweight-charts/docs/api"
