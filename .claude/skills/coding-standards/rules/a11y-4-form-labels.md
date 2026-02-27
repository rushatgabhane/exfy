---
ruleId: A11Y-4
title: Form inputs must have accessible labels
---

## [A11Y-4] Form inputs must have accessible labels

### Reasoning

WCAG 2.1 Success Criteria 1.3.1 (Info and Relationships) and 3.3.2 (Labels or Instructions) require form inputs to have programmatically associated labels. Screen reader users need to know what information to enter in each field. In React Native, use `accessibilityLabel` on `TextInput` components or use `accessibilityLabelledBy` (Android) / `aria-labelledby` to reference a visible label element by its `nativeID`.

### Incorrect

```tsx
// TextInput without accessible label
<TextInput
    value={email}
    onChangeText={setEmail}
    placeholder="Enter email"
/>

// TextInput with only placeholder (placeholder disappears when typing)
<TextInput
    value={amount}
    onChangeText={setAmount}
    placeholder="Amount"
    keyboardType="numeric"
/>

// Input without programmatic label association
<View>
    <Text>Username</Text>
    <TextInput value={username} onChangeText={setUsername} />
</View>
```

### Correct

```tsx
// TextInput with accessibilityLabel
<TextInput
    value={email}
    onChangeText={setEmail}
    placeholder="Enter email"
    accessibilityLabel={translate('common.email')}
/>

// TextInput with aria-label
<TextInput
    value={amount}
    onChangeText={setAmount}
    placeholder="0.00"
    aria-label={translate('common.amount')}
    keyboardType="numeric"
/>

// Input with accessibilityLabelledBy (Android) linking to visible label
<View>
    <Text nativeID="usernameLabel">Username</Text>
    <TextInput
        value={username}
        onChangeText={setUsername}
        accessibilityLabelledBy="usernameLabel"
    />
</View>

// Using aria-labelledby
<View>
    <Text nativeID="emailLabel">Email Address</Text>
    <TextInput
        value={email}
        onChangeText={setEmail}
        aria-labelledby="emailLabel"
    />
</View>

// Search input with search role (role implies purpose)
<TextInput
    value={searchQuery}
    onChangeText={setSearchQuery}
    accessibilityRole="search"
    accessibilityLabel={translate('common.search')}
/>
```

---

### Review Metadata

Flag ONLY when ALL of these are true:

- Element is a `TextInput` (or similar input component)
- Element does NOT have `accessibilityLabel` or `aria-label` prop
- Element does NOT have `accessibilityLabelledBy` or `aria-labelledby` prop
- Element is NOT wrapped by a component that provides label

**DO NOT flag if:**

- Element has `accessibilityLabel` or `aria-label` prop
- Element has `accessibilityLabelledBy` or `aria-labelledby` prop referencing a visible label
- Element is inside a wrapper component that handles accessible labeling (e.g., `TextInputWithLabel`, `InputWrapper`)
- Element has `accessible={false}`
- Input has `accessibilityRole="search"` with appropriate label

**Search Patterns** (hints for reviewers):
- `TextInput`
- `accessibilityLabel`
- `accessibilityLabelledBy`
- `aria-label`
- `aria-labelledby`
- `placeholder=`
- `onChangeText`
