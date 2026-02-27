---
ruleId: A11Y-1
title: Interactive elements must have accessible labels
---

## [A11Y-1] Interactive elements must have accessible labels

### Reasoning

Screen reader users cannot interact with buttons, pressables, or other interactive elements that lack accessible names. WCAG 2.1 Success Criterion 4.1.2 (Name, Role, Value) requires all interactive elements to have accessible names. In React Native, use `accessibilityLabel` (or `aria-label`) on touchable components. Without this, screen reader users hear only the role (e.g., "button") with no context about what the button does.

### Incorrect

```tsx
// Icon-only button without accessible label
<Pressable onPress={handleClose}>
    <Icon src={Expensicons.Close} />
</Pressable>

// Button with only visual text in nested component
<PressableWithFeedback onPress={handleSubmit}>
    <View>
        <Text>Submit</Text>
    </View>
</PressableWithFeedback>

// TouchableOpacity without accessible label
<TouchableOpacity onPress={openMenu}>
    <Icon src={Expensicons.ThreeDots} />
</TouchableOpacity>
```

### Correct

```tsx
// Icon button with accessible label
<Pressable
    onPress={handleClose}
    accessibilityLabel={translate('common.close')}
    accessibilityRole="button"
>
    <Icon src={Expensicons.Close} />
</Pressable>

// Button with accessible label
<PressableWithFeedback
    onPress={handleSubmit}
    accessibilityLabel={translate('common.submit')}
    accessibilityRole="button"
>
    <View>
        <Text>Submit</Text>
    </View>
</PressableWithFeedback>

// Using aria-label (equivalent to accessibilityLabel)
<TouchableOpacity
    onPress={openMenu}
    aria-label={translate('common.moreOptions')}
    accessibilityRole="button"
>
    <Icon src={Expensicons.ThreeDots} />
</TouchableOpacity>
```

---

### Review Metadata

Flag ONLY when ALL of these are true:

- Element is interactive (`Pressable`, `TouchableOpacity`, `TouchableHighlight`, `TouchableWithoutFeedback`, `PressableWithFeedback`, or has `onPress`/`onLongPress`)
- Element does NOT have `accessibilityLabel` or `aria-label` prop
- Element contains only an `Icon` component (no visible `Text` that could serve as label)

**DO NOT flag if:**

- Element has `accessibilityLabel` or `aria-label` prop
- Element contains a `Text` component with visible text (React Native uses this as accessible name)
- Element is wrapped by a parent that provides the accessible label
- Element has `accessible={false}` (intentionally hidden from accessibility tree)
- Element uses `accessibilityLabelledBy` or `aria-labelledby` to reference another element

**Search Patterns** (hints for reviewers):
- `Pressable`
- `TouchableOpacity`
- `TouchableHighlight`
- `PressableWithFeedback`
- `onPress`
- `<Icon`
