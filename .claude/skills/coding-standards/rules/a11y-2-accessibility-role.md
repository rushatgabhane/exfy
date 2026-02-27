---
ruleId: A11Y-2
title: Interactive elements must have appropriate accessibility roles
---

## [A11Y-2] Interactive elements must have appropriate accessibility roles

### Reasoning

WCAG 2.1 Success Criterion 4.1.2 (Name, Role, Value) requires user interface components to have appropriate roles communicated to assistive technology. In React Native, `accessibilityRole` (or `role`) tells screen readers what type of element they're interacting with. Without proper roles, screen reader users don't know how to interact with elements or what behavior to expect.

**Valid accessibilityRole values:** `'button'`, `'link'`, `'checkbox'`, `'radio'`, `'switch'`, `'tab'`, `'menuitem'`, `'header'`, `'image'`, `'imagebutton'`, `'search'`, `'adjustable'`, `'alert'`, `'combobox'`, `'menu'`, `'menubar'`, `'progressbar'`, `'radiogroup'`, `'scrollbar'`, `'spinbutton'`, `'summary'`, `'tablist'`, `'text'`, `'timer'`, `'togglebutton'`, `'toolbar'`, `'grid'`, `'none'`

### Incorrect

```tsx
// Pressable acting as button without role
<Pressable onPress={handleSubmit}>
    <Text>Submit</Text>
</Pressable>

// Link without link role
<Pressable onPress={() => Linking.openURL(url)}>
    <Text style={styles.link}>Visit website</Text>
</Pressable>

// Checkbox without checkbox role
<Pressable onPress={toggleChecked}>
    <Icon src={isChecked ? Expensicons.Checkmark : Expensicons.Square} />
    <Text>Remember me</Text>
</Pressable>

// Tab without tab role
<Pressable onPress={() => setActiveTab(tabId)}>
    <Text>{tabLabel}</Text>
</Pressable>
```

### Correct

```tsx
// Button with proper role
<Pressable
    onPress={handleSubmit}
    accessibilityRole="button"
    accessibilityLabel={translate('common.submit')}
>
    <Text>Submit</Text>
</Pressable>

// Link with proper role
<Pressable
    onPress={() => Linking.openURL(url)}
    accessibilityRole="link"
    accessibilityLabel={translate('common.visitWebsite')}
>
    <Text style={styles.link}>Visit website</Text>
</Pressable>

// Checkbox with role and state
<Pressable
    onPress={toggleChecked}
    accessibilityRole="checkbox"
    accessibilityState={{checked: isChecked}}
    accessibilityLabel={translate('common.rememberMe')}
>
    <Icon src={isChecked ? Expensicons.Checkmark : Expensicons.Square} />
    <Text>Remember me</Text>
</Pressable>

// Tab with role and selected state
<Pressable
    onPress={() => setActiveTab(tabId)}
    accessibilityRole="tab"
    accessibilityState={{selected: activeTab === tabId}}
>
    <Text>{tabLabel}</Text>
</Pressable>

// Using role prop (higher precedence alternative)
<Pressable
    onPress={handleSubmit}
    role="button"
>
    <Text>Submit</Text>
</Pressable>
```

---

### Review Metadata

Flag ONLY when ALL of these are true:

- Element is interactive (`Pressable`, `TouchableOpacity`, or has `onPress`)
- Element does NOT have `accessibilityRole` or `role` prop
- Element behavior clearly maps to a specific role (button, link, checkbox, switch, tab, menuitem, radio)

**DO NOT flag if:**

- Element has `accessibilityRole` or `role` prop
- Element uses a component that sets role internally (`Button`, `Switch`, `Checkbox`, `MenuItem`)
- Element is a generic touchable wrapper where role is set on parent/child
- Element has `accessible={false}`

**Search Patterns** (hints for reviewers):
- `Pressable`
- `TouchableOpacity`
- `onPress`
- `accessibilityRole`
- `Linking.openURL`
- `toggleChecked`
- `setActiveTab`
