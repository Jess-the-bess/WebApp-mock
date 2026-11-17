# RGB Liquid iOS Button Code

This guide provides copy-pasteable implementations for creating an iOS 26-style **Liquid Glass** button with custom **RGB colors** in both native **SwiftUI** and **HTML/CSS**.

---

## 1. Native iOS (SwiftUI — iOS 26+)

In iOS 26 and later, Apple provides the `.glassEffect()` modifier to apply Liquid Glass to views and buttons. To give it a custom **RGB** tint or animated RGB glow, pass a `Color(red:green:blue:)` into `.tint()` and pair it with an animated border overlay.

```swift
import SwiftUI

struct RGBLiquidGlassButton: View {
    @State private var hue: Double = 0.0

    var body: some View {
        Button(action: {
            print("Button tapped")
        }) {
            Text("RGB Liquid Button")
                .font(.headline)
                .foregroundStyle(.white)
                .padding(.horizontal, 24)
                .padding(.vertical, 14)
        }
        // Applies the native iOS 26 Liquid Glass material with an RGB tint
        .glassEffect(
            .regular
                .tint(Color(red: 0.2, green: 0.6, blue: 1.0, opacity: 0.6))
                .interactive(),
            in: .capsule
        )
        // Optional animated RGB border glow
        .overlay(
            Capsule()
                .stroke(
                    AngularGradient(
                        colors: [.red, .green, .blue, .red],
                        center: .center,
                        angle: .degrees(hue)
                    ),
                    lineWidth: 2
                )
        )
        .onAppear {
            withAnimation(.linear(duration: 4).repeatForever(autoreverses: false)) {
                hue = 360
            }
        }
    }
}
```

### SwiftUI Notes
* **Standard Glass Styles.** If you do not need custom RGB tinting, use `.buttonStyle(.glass)` or `.buttonStyle(.glassProminent)` on a standard SwiftUI `Button`.
* **Grouping Buttons.** Wrap multiple glass buttons inside a `GlassEffectContainer` so nearby shapes blend and morph together smoothly.

---

## 2. Web Simulation (HTML & CSS)

To recreate the iOS 26 Liquid Glass aesthetic with an animated RGB border on the web, use `backdrop-filter` paired with a masked `conic-gradient` rim.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RGB Liquid Glass Button</title>
<style>
  body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background: linear-gradient(135deg, #1a1a2e, #16213e, #0f3460);
    font-family: -apple-system, BlinkMacSystemFont, sans-serif;
  }

  .liquid-rgb-btn {
    position: relative;
    padding: 16px 36px;
    font-size: 17px;
    font-weight: 600;
    color: #ffffff;
    background: rgba(255, 255, 255, 0.12);
    border: none;
    border-radius: 999px;
    cursor: pointer;
    backdrop-filter: blur(18px) saturate(180%);
    -webkit-backdrop-filter: blur(18px) saturate(180%);
    box-shadow: 
      0 8px 32px rgba(0, 0, 0, 0.3),
      inset 0 1px 1px rgba(255, 255, 255, 0.4);
    transition: transform 0.2s ease, background 0.2s ease;
    overflow: hidden;
  }

  /* Animated RGB Rim */
  .liquid-rgb-btn::before {
    content: "";
    position: absolute;
    inset: 0;
    border-radius: 999px;
    padding: 2px;
    background: conic-gradient(
      from var(--angle, 0deg),
      rgb(255, 0, 85),
      rgb(0, 255, 170),
      rgb(0, 140, 255),
      rgb(255, 0, 85)
    );
    -webkit-mask: 
      linear-gradient(#fff 0 0) content-box, 
      linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
    mask-composite: exclude;
    animation: spin-rgb 4s linear infinite;
  }

  .liquid-rgb-btn:hover {
    transform: scale(1.04);
    background: rgba(255, 255, 255, 0.18);
  }

  .liquid-rgb-btn:active {
    transform: scale(0.97);
  }

  @property --angle {
    syntax: "<angle>";
    initial-value: 0deg;
    inherits: false;
  }

  @keyframes spin-rgb {
    to {
      --angle: 360deg;
    }
  }
</style>
</head>
<body>
  <button class="liquid-rgb-btn">RGB Liquid Glass</button>
</body>
</html>
```
