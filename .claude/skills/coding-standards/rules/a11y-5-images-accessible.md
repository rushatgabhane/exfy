---
ruleId: A11Y-5
title: Images must have accessible labels or be marked decorative
---

## [A11Y-5] Images must have accessible labels or be marked decorative

### Reasoning

WCAG 2.1 Success Criterion 1.1.1 (Non-text Content) requires all images to have text alternatives. Screen reader users need to understand what images convey. In React Native, use `accessibilityLabel` for meaningful images. For decorative images, use `accessibilityElementsHidden={true}` (iOS) or `importantForAccessibility="no-hide-descendants"` (Android) to hide them from screen readers.

### Incorrect

```tsx
// Image without accessible label
<Image source={{uri: receiptUrl}} style={styles.receiptImage} />

// Avatar without context
<Avatar source={avatarSource} />

// Meaningful icon without label (standalone, not with text)
<View>
    <Icon src={Expensicons.Checkmark} fill={theme.success} />
</View>

// Logo without description
<Image source={require('./assets/logo.png')} />
```

### Correct

```tsx
// Receipt image with accessible label
<Image
    source={{uri: receiptUrl}}
    style={styles.receiptImage}
    accessibilityLabel={translate('common.receipt')}
    accessibilityRole="image"
/>

// Avatar with user context
<Avatar
    source={avatarSource}
    accessibilityLabel={`${userName} ${translate('common.avatar')}`}
/>

// Decorative icon hidden from accessibility tree (iOS)
<Icon
    src={Expensicons.Checkmark}
    fill={theme.success}
    accessibilityElementsHidden={true}
/>

// Decorative icon hidden from accessibility tree (Android)
<Icon
    src={Expensicons.Checkmark}
    fill={theme.success}
    importantForAccessibility="no-hide-descendants"
/>

// Cross-platform decorative hiding
<View
    accessibilityElementsHidden={true}
    importantForAccessibility="no-hide-descendants"
>
    <Icon src={Expensicons.Checkmark} fill={theme.success} />
</View>

// Status icon with meaning communicated via parent
<View accessibilityLabel={translate('common.approved')}>
    <Icon src={Expensicons.Checkmark} fill={theme.success} />
    <Text>Approved</Text>
</View>

// Logo with accessible label
<Image
    source={require('./assets/logo.png')}
    accessibilityLabel="Expensify"
    accessibilityRole="image"
/>

// Using aria-hidden for decorative content
<Image
    source={backgroundImage}
    aria-hidden={true}
/>
```

---

### Review Metadata

Flag ONLY when ALL of these are true:

- Element is an `Image` component or similar (`FastImage`, standalone `Icon`, `Avatar`)
- Element does NOT have `accessibilityLabel` or `aria-label` prop
- Element is NOT marked as decorative (`accessibilityElementsHidden`, `importantForAccessibility="no"`, `aria-hidden`)
- Image appears to convey meaningful content (receipt, avatar, status indicator, logo)

**DO NOT flag if:**

- Element has `accessibilityLabel` or `aria-label`
- Element has `accessibilityElementsHidden={true}`
- Element has `importantForAccessibility="no"` or `"no-hide-descendants"`
- Element has `aria-hidden={true}`
- Parent element provides accessible context for the image
- Icon is accompanied by visible text that conveys the same meaning
- Image is clearly decorative (background patterns, divider lines)

**Search Patterns** (hints for reviewers):
- `<Image`
- `<FastImage`
- `<Avatar`
- `<Icon`
- `source=`
- `accessibilityLabel`
- `accessibilityElementsHidden`
- `importantForAccessibility`
