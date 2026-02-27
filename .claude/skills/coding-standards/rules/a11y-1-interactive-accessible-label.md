---
ruleId: A11Y-1
title: Icon-only interactive elements must have accessibilityLabel
---

## [A11Y-1] Icon-only interactive elements must have accessibilityLabel

### Reasoning

Screen readers cannot describe icon-only buttons without an accessible label. WCAG 2.1 SC 4.1.2 requires interactive elements to have accessible names.

### Incorrect

```tsx
<Pressable onPress={handleClose}>
    <Icon src={Expensicons.Close} />
</Pressable>
```

### Correct

```tsx
<Pressable
    onPress={handleClose}
    accessibilityLabel={translate('common.close')}
>
    <Icon src={Expensicons.Close} />
</Pressable>
```

---

### Review Metadata

**When reviewing:** Look for touchables (`Pressable`, `TouchableOpacity`, etc.) that contain only icons without visible text. Check if `accessibilityLabel` or `aria-label` is present.

**Use judgment:** Consider whether the element already has an accessible label via a parent component or if visible text provides the label.

**Search Patterns:**
- `onPress`
- `<Icon`
