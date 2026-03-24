# iOS 26 — SwiftUI Design Notes

For apps targeting iOS 26 / visionOS, the design language shifts significantly toward **liquid glass**, **spatial computing**, and **depth**. These notes cover what's different from iOS 17 and how to apply it to native apps.

---

## Core iOS 26 Design Principles

### 1. Liquid Glass
The defining visual of iOS 26. Surfaces appear to be made of translucent glass with real depth and refraction.

**Implementation (SwiftUI):**
```swift
.sheetStyle(.glass) // iOS 26 sheet modifier
.background(.glass) // Glass material
```

**Design rules:**
- Glass panels sit at `zIndex` layers — content beneath is refracted
- Borders: `0.5px` at most. No thick borders on glass.
- Corner radius: `22px` for cards, `18px` for sheets, `12px` for buttons
- Never use drop shadows on glass — depth comes from blur and refraction

### 2. Typography
- **SF Pro** — always, no substitutes
- Dynamic Type mandatory — all text must scale
- **Title 1** → 34pt Bold (screen titles)
- **Title 2** → 28pt Bold
- **Headline** → 17pt Semibold
- **Body** → 17pt Regular
- **Caption** → 12pt Regular

### 3. Spacing
- Base unit: **8pt** (not 4pt like web)
- Content margins: 16pt on iPhone, 24pt+ on iPad
- Safe area respect is non-negotiable
- Bottom sheet detents: 3 standard — `height(0.25)`, `medium()`, `full()`

### 4. Color
- Use **Semantic Colors** — system teal, system orange, etc. Not hex codes.
- `Color.accentColor` — always the brand accent
- System materials: `.ultraThinMaterial`, `.regularMaterial`, `.thickMaterial`, `.glass`
- Support both light and dark — test in both
- Dynamic colors via `Color("primary")` in asset catalog

### 5. Navigation
- **Large title navigation bar** — collapses on scroll
- **Tab bar** — use `.tabBarStyle(.glass)` for blur tab bar
- **Navigation stack** — swipe-back always enabled
- **Bottom sheets** — use `presentationDetents([.medium, .large])`

### 6. Motion
- System animation curves — `spring(response: 0.35, dampingFraction: 0.85)`
- 60fps always — no jank
- Match iOS system animations: sheet presentations, navigation transitions
- Haptic feedback: `.impact(style: .light)` on selection, `.notification(type: .success)` on confirm

### 7. Components

#### Buttons
```swift
// Primary — filled, rounded capsule
Button(action: {}) {
    Label("Continue", systemImage: "arrow.right")
}
.buttonStyle(.borderedProminent)

// Secondary — outlined
Button(action: {}) {
    Label("Learn More", systemImage: "book")
}
.buttonStyle(.bordered)

// Tertiary — text only
Button("See All") {}
.buttonStyle(.plain)
```

#### Cards
```swift
VStack {
    Image(...)
    Text("Title")
    Text("Subtitle")
}
.frame(maxWidth: .infinity)
.padding(20)
.background(.glass)
.clipShape(RoundedRectangle(cornerRadius: 22))
```

#### Floating Action / Pill Buttons
```swift
Button(action: {}) {
    Image(systemName: "plus")
        .font(.system(20, weight: .semibold))
        .foregroundStyle(.white)
        .frame(56, 56)
        .background(.ultraThinMaterial)
        .clipShape(Circle())
        .shadow(color: .black.opacity(0.25), radius: 8, y: 4)
}
```

### 8. Accessibility (Non-Negotiable)
- All interactive elements: minimum 44x44pt tap target
- VoiceOver labels on all icons and images
- Dynamic Type: every text element scales
- Reduce Motion: honor `accessibilityReduceMotion`
- High contrast: test in Accessibility > Increase Contrast

---

## Key Differences from iOS 17

| Aspect | iOS 17 | iOS 26 |
|---|---|---|
| Backgrounds | Solid / blur | Liquid glass / material layers |
| Borders | 1px solid | 0.5px, often glass edge |
| Cards | Shadow-based depth | Glass refraction depth |
| Typography | SF Pro | SF Pro + SF Rounded for headings |
| Animations | Spring | Physics-based, more fluid |
| Dark mode | Deep black | Rich gray surfaces |
| Corner radius | 13px cards | 22px cards, 18px sheets |

---

## SwiftUI File Structure for iOS Apps

```
ios/
├── App/
│   ├── ContentView.swift
│   └── AppEntry.swift
├── Views/
│   ├── Home/
│   │   └── HomeView.swift
│   ├── Components/
│   │   ├── GlassCard.swift
│   │   ├── PillButton.swift
│   │   └── GlassTabBar.swift
│   └── Shared/
├── Models/
├── ViewModels/
├── Services/
└── Resources/
    ├── Assets.xcassets/
    └── Fonts/
```

---

## Design Tokens (SwiftUI)

```swift
struct DesignTokens {
    // Colors
    static let accent      = Color.accentColor
    static let surface      = Color(uiColor: .systemBackground)
    static let surface2     = Color(uiColor: .secondarySystemBackground)
    static let text         = Color(uiColor: .label)
    static let textMuted    = Color(uiColor: .secondaryLabel)
    
    // Spacing (8pt grid)
    static let xs: CGFloat = 4
    static let sm: CGFloat = 8
    static let md: CGFloat = 16
    static let lg: CGFloat = 24
    static let xl: CGFloat = 32
    
    // Radius
    static let radiusSm: CGFloat = 6
    static let radiusMd: CGFloat = 12
    static let radiusLg: CGFloat = 22
    
    // Animation
    static let spring = SwiftUI.Animation.spring(response: 0.35, dampingFraction: 0.85)
}
```
