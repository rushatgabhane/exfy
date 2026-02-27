---
ruleId: A11Y-3
title: Stateful components should communicate state via accessibilityState
---

## [A11Y-3] Stateful components should communicate state via accessibilityState

### Reasoning

WCAG 2.1 SC 4.1.2 requires component states to be programmatically determinable. Screen reader users need to know if a button is disabled, checkbox is checked, or section is expanded.

**accessibilityState fields:**
- `disabled: boolean`
- `selected: boolean`
- `checked: boolean | 'mixed'`
- `busy: boolean`
- `expanded: boolean`

### Incorrect

```tsx
<Pressable
    onPress={handleSubmit}
    disabled={isLoading}
    style={isLoading && styles.buttonDisabled}
>
    <Text>Submit</Text>
</Pressable>
```

### Correct

```tsx
<Pressable
    onPress={handleSubmit}
    disabled={isLoading}
    accessibilityState={{disabled: isLoading}}
>
    <Text>Submit</Text>
</Pressable>
```

---

### Review Metadata

**When reviewing:** Look for elements with visual state changes (disabled styling, selected/active tabs, expanded/collapsed sections, checked checkboxes). Check if `accessibilityState` reflects that state.

**Use judgment:** Consider whether the state is meaningful for screen reader users. Visual-only states (hover, focus rings) don't need accessibilityState.

**Search Patterns:**
- `disabled={`
- `isActive`
- `isSelected`
- `isChecked`
- `isExpanded`
