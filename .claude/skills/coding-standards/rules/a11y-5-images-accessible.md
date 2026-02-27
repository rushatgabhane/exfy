---
ruleId: A11Y-5
title: Meaningful images must have accessibilityLabel or be marked decorative
---

## [A11Y-5] Meaningful images must have accessibilityLabel or be marked decorative

### Reasoning

WCAG 2.1 SC 1.1.1 requires images to have text alternatives. For meaningful images, use `accessibilityLabel`. For decorative images, use `accessibilityElementsHidden={true}` (iOS) or `importantForAccessibility="no-hide-descendants"` (Android).

### Incorrect

```tsx
<Image source={{uri: receiptUrl}} style={styles.receiptImage} />
```

### Correct

```tsx
// Meaningful image
<Image
    source={{uri: receiptUrl}}
    accessibilityLabel={translate('common.receipt')}
/>

// Decorative image
<Image
    source={backgroundPattern}
    accessibilityElementsHidden={true}
    importantForAccessibility="no-hide-descendants"
/>
```

---

### Review Metadata

**When reviewing:** Look for `Image`, `FastImage`, or `Avatar` components. Determine if the image conveys meaning (receipts, avatars, status icons) or is decorative (backgrounds, patterns).

**Use judgment:** Consider whether adjacent text already conveys the image's meaning, or if a parent component provides the accessible label.

**Search Patterns:**
- `<Image`
- `<Avatar`
- `source=`
