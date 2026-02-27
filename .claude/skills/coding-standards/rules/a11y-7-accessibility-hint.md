---
ruleId: A11Y-7
title: Use accessibilityHint for non-obvious actions
---

## [A11Y-7] Use accessibilityHint for non-obvious actions

### Reasoning

WCAG 2.1 Success Criterion 3.3.2 (Labels or Instructions) recommends providing additional context when labels alone are insufficient. In React Native, `accessibilityHint` provides extra information about what will happen when an action is performed. This is especially important for actions with non-obvious consequences, destructive actions, or navigation that takes users to unexpected places. Hints should describe the result of the action, not repeat the label.

### Incorrect

```tsx
// Delete action without hint about consequence
<Pressable
    onPress={handleDelete}
    accessibilityLabel={translate('common.delete')}
    accessibilityRole="button"
>
    <Icon src={Expensicons.Trashcan} />
</Pressable>

// Navigation with unclear destination
<Pressable
    onPress={navigateToSettings}
    accessibilityLabel="Settings"
    accessibilityRole="button"
>
    <Icon src={Expensicons.Gear} />
</Pressable>

// Action that opens external app
<Pressable
    onPress={openInMaps}
    accessibilityLabel={translate('common.viewOnMap')}
    accessibilityRole="button"
>
    <Text>View location</Text>
</Pressable>
```

### Correct

```tsx
// Delete action with hint about consequence
<Pressable
    onPress={handleDelete}
    accessibilityLabel={translate('common.delete')}
    accessibilityHint={translate('accessibilityHints.deletesThisExpense')}
    accessibilityRole="button"
>
    <Icon src={Expensicons.Trashcan} />
</Pressable>

// Navigation with clear destination hint
<Pressable
    onPress={navigateToSettings}
    accessibilityLabel="Settings"
    accessibilityHint={translate('accessibilityHints.opensAppSettings')}
    accessibilityRole="button"
>
    <Icon src={Expensicons.Gear} />
</Pressable>

// Action that opens external app with hint
<Pressable
    onPress={openInMaps}
    accessibilityLabel={translate('common.viewOnMap')}
    accessibilityHint={translate('accessibilityHints.opensMapsApp')}
    accessibilityRole="button"
>
    <Text>View location</Text>
</Pressable>

// Long press action with hint
<Pressable
    onPress={selectItem}
    onLongPress={showContextMenu}
    accessibilityLabel={itemName}
    accessibilityHint={translate('accessibilityHints.longPressForOptions')}
>
    <ItemContent />
</Pressable>

// Swipe action hint
<SwipeableRow
    accessibilityHint={translate('accessibilityHints.swipeToDelete')}
>
    <ListItem />
</SwipeableRow>
```

---

### Review Metadata

Flag ONLY when ALL of these are true:

- Element performs a non-obvious or potentially destructive action
- Element has `accessibilityLabel` but NOT `accessibilityHint`
- Action consequence is not clear from the label alone

Actions that typically need hints:
- Delete/remove operations
- Navigation to external apps
- Actions with side effects (sending, submitting, sharing)
- Long press or gesture-based actions
- Modal/sheet triggers

**DO NOT flag if:**

- Element has `accessibilityHint` prop
- Action is obvious from the label (e.g., "Submit form" button in a form)
- Element is a simple navigation link to an obvious destination
- Element is a toggle where the label describes the action clearly

**Search Patterns** (hints for reviewers):
- `accessibilityHint`
- `handleDelete`
- `onDelete`
- `remove`
- `Linking.openURL`
- `openInMaps`
- `onLongPress`
- `Trashcan`
