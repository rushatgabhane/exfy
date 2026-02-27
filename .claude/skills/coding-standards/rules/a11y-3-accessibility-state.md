---
ruleId: A11Y-3
title: Component states must be communicated via accessibilityState
---

## [A11Y-3] Component states must be communicated via accessibilityState

### Reasoning

WCAG 2.1 Success Criterion 4.1.2 (Name, Role, Value) requires the current state of user interface components to be programmatically determinable. Screen reader users need to know if a button is disabled, a checkbox is checked, an accordion is expanded, or a tab is selected. React Native's `accessibilityState` prop (or ARIA equivalents) communicates these states to assistive technology.

**accessibilityState fields:**
- `disabled: boolean` - Element is disabled
- `selected: boolean` - Selectable element is selected
- `checked: boolean | 'mixed'` - Checkable element state
- `busy: boolean` - Element is busy/loading
- `expanded: boolean` - Expandable element is expanded

### Incorrect

```tsx
// Disabled button without accessibilityState
<Pressable
    onPress={handleSubmit}
    disabled={isLoading}
    style={[styles.button, isLoading && styles.buttonDisabled]}
>
    <Text>Submit</Text>
</Pressable>

// Selected tab without state
<Pressable
    onPress={() => setActiveTab(tabId)}
    accessibilityRole="tab"
    style={[styles.tab, isActive && styles.tabActive]}
>
    <Text>{tabLabel}</Text>
</Pressable>

// Expandable section without expanded state
<Pressable onPress={toggleExpanded} accessibilityRole="button">
    <Text>{sectionTitle}</Text>
    <Icon src={isExpanded ? Expensicons.UpArrow : Expensicons.DownArrow} />
</Pressable>

// Checkbox showing visual state only
<Pressable onPress={toggleChecked} accessibilityRole="checkbox">
    <View style={[styles.checkbox, isChecked && styles.checkboxChecked]}>
        {isChecked && <Icon src={Expensicons.Checkmark} />}
    </View>
</Pressable>
```

### Correct

```tsx
// Disabled button with accessibilityState
<Pressable
    onPress={handleSubmit}
    disabled={isLoading}
    accessibilityRole="button"
    accessibilityState={{disabled: isLoading}}
    accessibilityLabel={translate('common.submit')}
    style={[styles.button, isLoading && styles.buttonDisabled]}
>
    <Text>Submit</Text>
</Pressable>

// Selected tab with state
<Pressable
    onPress={() => setActiveTab(tabId)}
    accessibilityRole="tab"
    accessibilityState={{selected: isActive}}
    style={[styles.tab, isActive && styles.tabActive]}
>
    <Text>{tabLabel}</Text>
</Pressable>

// Expandable section with expanded state
<Pressable
    onPress={toggleExpanded}
    accessibilityRole="button"
    accessibilityState={{expanded: isExpanded}}
    accessibilityLabel={sectionTitle}
>
    <Text>{sectionTitle}</Text>
    <Icon src={isExpanded ? Expensicons.UpArrow : Expensicons.DownArrow} />
</Pressable>

// Checkbox with checked state
<Pressable
    onPress={toggleChecked}
    accessibilityRole="checkbox"
    accessibilityState={{checked: isChecked}}
    accessibilityLabel={translate('common.agreeToTerms')}
>
    <View style={[styles.checkbox, isChecked && styles.checkboxChecked]}>
        {isChecked && <Icon src={Expensicons.Checkmark} />}
    </View>
</Pressable>

// Using aria-* equivalents
<Pressable
    onPress={handleSubmit}
    disabled={isLoading}
    aria-disabled={isLoading}
    aria-busy={isLoading}
>
    <Text>Submit</Text>
</Pressable>
```

---

### Review Metadata

Flag ONLY when ALL of these are true:

- Element has visual state changes (disabled styling, selected/active styling, expanded/collapsed, checked)
- Element does NOT have corresponding `accessibilityState` or aria-* prop
- State is meaningful for user interaction (not just visual decoration)

States to check for:
- `disabled` / `aria-disabled` - for disabled buttons/inputs
- `selected` / `aria-selected` - for tabs, toggles, selectable items
- `checked` / `aria-checked` - for checkboxes, radio buttons
- `expanded` / `aria-expanded` - for accordions, expandable sections
- `busy` / `aria-busy` - for elements in loading state

**DO NOT flag if:**

- Element has `accessibilityState` with the relevant state
- Element uses aria-* equivalent (`aria-disabled`, `aria-checked`, `aria-expanded`, `aria-selected`, `aria-busy`)
- Element uses a component that handles state internally (`Switch`, `Checkbox`)
- State is purely visual (hover effects, focus rings)
- Element has `accessible={false}`

**Search Patterns** (hints for reviewers):
- `disabled={`
- `isActive`
- `isSelected`
- `isChecked`
- `isExpanded`
- `isLoading`
- `accessibilityState`
