---
ruleId: A11Y-6
title: Dynamic status changes should be announced to screen readers
---

## [A11Y-6] Dynamic status changes should be announced to screen readers

### Reasoning

WCAG 2.1 SC 4.1.3 requires status messages to be announced without focus. Use `accessibilityLiveRegion` (Android) or `AccessibilityInfo.announceForAccessibility()` for important dynamic content like errors, success messages, or counters.

**accessibilityLiveRegion values:**
- `'polite'` - Announce when idle
- `'assertive'` - Interrupt to announce immediately

### Incorrect

```tsx
{hasError && <Text style={styles.error}>{errorMessage}</Text>}
```

### Correct

```tsx
{hasError && (
    <Text
        style={styles.error}
        accessibilityLiveRegion="assertive"
        accessibilityRole="alert"
    >
        {errorMessage}
    </Text>
)}

// Or cross-platform
useEffect(() => {
    if (hasError) {
        AccessibilityInfo.announceForAccessibility(errorMessage);
    }
}, [hasError, errorMessage]);
```

---

### Review Metadata

**When reviewing:** Look for dynamically appearing content like error messages, success toasts, loading states, or live counters. Check if screen readers would be notified.

**Use judgment:** Not all dynamic content needs announcements. Focus on important status changes that affect the user's task.

**Search Patterns:**
- `hasError`
- `errorMessage`
- `toast`
- `snackbar`
- `AccessibilityInfo`
