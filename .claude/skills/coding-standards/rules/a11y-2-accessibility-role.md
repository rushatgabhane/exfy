---
ruleId: A11Y-2
title: Interactive elements should have appropriate accessibilityRole
---

## [A11Y-2] Interactive elements should have appropriate accessibilityRole

### Reasoning

WCAG 2.1 SC 4.1.2 requires user interface components to have appropriate roles. In React Native, `accessibilityRole` tells screen readers what type of element they're interacting with (button, link, checkbox, switch, tab, etc.).

**Valid roles:** `'button'`, `'link'`, `'checkbox'`, `'radio'`, `'switch'`, `'tab'`, `'menuitem'`, `'header'`, `'image'`, `'search'`, `'adjustable'`, `'alert'`, `'progressbar'`, `'togglebutton'`

### Incorrect

```tsx
<Pressable onPress={() => Linking.openURL(url)}>
    <Text style={styles.link}>Visit website</Text>
</Pressable>
```

### Correct

```tsx
<Pressable
    onPress={() => Linking.openURL(url)}
    accessibilityRole="link"
>
    <Text style={styles.link}>Visit website</Text>
</Pressable>
```

---

### Review Metadata

**When reviewing:** Look for custom touchables that behave like specific UI patterns (links opening URLs, checkboxes with toggle state, tabs). Check if `accessibilityRole` matches the behavior.

**Use judgment:** Many wrapper components set roles internally. Only flag when the behavior clearly maps to a role but none is set.

**Search Patterns:**
- `Linking.openURL`
- `toggleChecked`
- `setActiveTab`
- `accessibilityRole`
