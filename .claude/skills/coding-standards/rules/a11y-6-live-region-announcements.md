---
ruleId: A11Y-6
title: Dynamic content changes must be announced to screen readers
---

## [A11Y-6] Dynamic content changes must be announced to screen readers

### Reasoning

WCAG 2.1 Success Criterion 4.1.3 (Status Messages) requires that status messages be programmatically announced without receiving focus. Screen reader users need to be notified of important dynamic content changes like form validation errors, success messages, loading states, or live counters. In React Native, use `accessibilityLiveRegion` (Android) or `AccessibilityInfo.announceForAccessibility()` for cross-platform announcements.

**accessibilityLiveRegion values:**
- `'none'` - No announcement (default)
- `'polite'` - Announce when screen reader is idle
- `'assertive'` - Interrupt current speech to announce immediately

### Incorrect

```tsx
// Error message not announced
{hasError && (
    <Text style={styles.errorText}>
        {errorMessage}
    </Text>
)}

// Success toast not announced
<View style={styles.toast}>
    <Text>Payment successful!</Text>
</View>

// Counter updates not announced
<Text>Items in cart: {cartCount}</Text>

// Loading state change not communicated
{isLoading && <ActivityIndicator />}
```

### Correct

```tsx
// Error message with live region (Android) and alert role
{hasError && (
    <Text
        style={styles.errorText}
        accessibilityLiveRegion="assertive"
        accessibilityRole="alert"
    >
        {errorMessage}
    </Text>
)}

// Using aria-live equivalent
{hasError && (
    <Text
        style={styles.errorText}
        aria-live="assertive"
        accessibilityRole="alert"
    >
        {errorMessage}
    </Text>
)}

// Success toast with polite announcement
<View style={styles.toast}>
    <Text accessibilityLiveRegion="polite">
        Payment successful!
    </Text>
</View>

// Counter with live region for updates
<Text accessibilityLiveRegion="polite">
    Items in cart: {cartCount}
</Text>

// Cross-platform announcement using AccessibilityInfo
useEffect(() => {
    if (hasError) {
        AccessibilityInfo.announceForAccessibility(errorMessage);
    }
}, [hasError, errorMessage]);

// Loading state with busy announcement
<View
    accessibilityRole="progressbar"
    accessibilityState={{busy: isLoading}}
    accessibilityLabel={isLoading ? translate('common.loading') : translate('common.loaded')}
>
    {isLoading && <ActivityIndicator />}
</View>

// Form validation errors announced
const handleValidationError = (error: string) => {
    setError(error);
    AccessibilityInfo.announceForAccessibility(error);
};
```

---

### Review Metadata

Flag ONLY when ALL of these are true:

- Content changes dynamically based on user action or async operation
- Content is important for user awareness (errors, success messages, counters, status changes)
- Element does NOT have `accessibilityLiveRegion`, `aria-live`, or `accessibilityRole="alert"`
- Code does NOT use `AccessibilityInfo.announceForAccessibility()`

**DO NOT flag if:**

- Element has `accessibilityLiveRegion` prop (`polite` or `assertive`)
- Element has `aria-live` prop
- Element has `accessibilityRole="alert"`
- Code uses `AccessibilityInfo.announceForAccessibility()` for the message
- Content change is visual-only (animations, styling changes)
- Content is already focused (focus announces content automatically)

**Search Patterns** (hints for reviewers):
- `accessibilityLiveRegion`
- `aria-live`
- `AccessibilityInfo`
- `announceForAccessibility`
- `errorMessage`
- `hasError`
- `isLoading`
- `toast`
- `snackbar`
