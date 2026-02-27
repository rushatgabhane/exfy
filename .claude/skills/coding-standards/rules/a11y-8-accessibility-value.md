---
ruleId: A11Y-8
title: Range-based components should communicate values via accessibilityValue
---

## [A11Y-8] Range-based components should communicate values via accessibilityValue

### Reasoning

WCAG 2.1 SC 4.1.2 requires component values to be programmatically determinable. For sliders, progress bars, steppers, and ratings, use `accessibilityValue` with `min`, `max`, `now`, and optionally `text`.

### Incorrect

```tsx
<View style={styles.progressBar}>
    <View style={{width: `${progress}%`}} />
</View>
```

### Correct

```tsx
<View
    style={styles.progressBar}
    accessibilityRole="progressbar"
    accessibilityValue={{min: 0, max: 100, now: progress}}
>
    <View style={{width: `${progress}%`}} />
</View>
```

---

### Review Metadata

**When reviewing:** Look for sliders, progress indicators, steppers, or rating components. Check if `accessibilityValue` communicates the current value and range.

**Use judgment:** Consider whether the value is already communicated via visible text or if a library component handles this internally.

**Search Patterns:**
- `Slider`
- `progressBar`
- `stepper`
- `rating`
- `accessibilityValue`
