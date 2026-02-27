---
ruleId: A11Y-4
title: TextInput must have accessibilityLabel
---

## [A11Y-4] TextInput must have accessibilityLabel

### Reasoning

WCAG 2.1 SC 1.3.1 and 3.3.2 require form inputs to have accessible labels. Placeholders disappear when typing and aren't reliable labels for screen reader users.

### Incorrect

```tsx
<TextInput
    value={email}
    onChangeText={setEmail}
    placeholder="Enter email"
/>
```

### Correct

```tsx
<TextInput
    value={email}
    onChangeText={setEmail}
    placeholder="Enter email"
    accessibilityLabel={translate('common.email')}
/>
```

---

### Review Metadata

**When reviewing:** Look for `TextInput` components. Check if they have `accessibilityLabel`, `aria-label`, or `accessibilityLabelledBy`.

**Use judgment:** Consider whether the input is inside a wrapper component that handles labeling (like `InputWrapper` or `TextInputWithLabel`).

**Search Patterns:**
- `TextInput`
- `onChangeText`
