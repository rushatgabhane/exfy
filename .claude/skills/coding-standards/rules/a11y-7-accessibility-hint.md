---
ruleId: A11Y-7
title: Non-obvious actions should have accessibilityHint
---

## [A11Y-7] Non-obvious actions should have accessibilityHint

### Reasoning

WCAG 2.1 SC 3.3.2 recommends additional context when labels alone are insufficient. `accessibilityHint` describes what will happen when an action is performed - especially important for destructive actions, external navigation, or gesture-based interactions.

### Incorrect

```tsx
<Pressable
    onPress={handleDelete}
    accessibilityLabel={translate('common.delete')}
>
    <Icon src={Expensicons.Trashcan} />
</Pressable>
```

### Correct

```tsx
<Pressable
    onPress={handleDelete}
    accessibilityLabel={translate('common.delete')}
    accessibilityHint={translate('accessibilityHints.deletesThisExpense')}
>
    <Icon src={Expensicons.Trashcan} />
</Pressable>
```

---

### Review Metadata

**When reviewing:** Look for actions with non-obvious consequences: delete/remove operations, external app navigation, long press actions, or swipe gestures.

**Use judgment:** Hints should add value. Don't flag obvious actions like "Submit" on a form or "Close" on a modal.

**Search Patterns:**
- `handleDelete`
- `onDelete`
- `Linking.openURL`
- `onLongPress`
