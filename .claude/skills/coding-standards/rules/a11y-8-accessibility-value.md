---
ruleId: A11Y-8
title: Range-based components must communicate their values
---

## [A11Y-8] Range-based components must communicate their values

### Reasoning

WCAG 2.1 Success Criterion 4.1.2 (Name, Role, Value) requires that the current value of components be programmatically determinable. For adjustable/range-based components like sliders, progress indicators, and steppers, screen reader users need to know the current value and the valid range. In React Native, use `accessibilityValue` with `min`, `max`, `now`, and optionally `text` fields to communicate this information.

**accessibilityValue fields:**
- `min: number` - Minimum value of the range (required if `now` is set)
- `max: number` - Maximum value of the range (required if `now` is set)
- `now: number` - Current value
- `text: string` - Textual description of value (overrides min/max/now)

### Incorrect

```tsx
// Slider without value information
<Slider
    value={volume}
    onValueChange={setVolume}
    minimumValue={0}
    maximumValue={100}
/>

// Progress bar without value
<View style={styles.progressBar}>
    <View style={[styles.progressFill, {width: `${progress}%`}]} />
</View>

// Stepper without value context
<View>
    <Pressable onPress={decrement}>
        <Text>-</Text>
    </Pressable>
    <Text>{quantity}</Text>
    <Pressable onPress={increment}>
        <Text>+</Text>
    </Pressable>
</View>

// Rating component without value
<View>
    {[1, 2, 3, 4, 5].map((star) => (
        <Icon
            key={star}
            src={star <= rating ? Expensicons.StarFilled : Expensicons.Star}
        />
    ))}
</View>
```

### Correct

```tsx
// Slider with accessibilityValue
<Slider
    value={volume}
    onValueChange={setVolume}
    minimumValue={0}
    maximumValue={100}
    accessibilityRole="adjustable"
    accessibilityLabel={translate('common.volume')}
    accessibilityValue={{
        min: 0,
        max: 100,
        now: volume,
    }}
/>

// Progress bar with value
<View
    style={styles.progressBar}
    accessibilityRole="progressbar"
    accessibilityLabel={translate('common.uploadProgress')}
    accessibilityValue={{
        min: 0,
        max: 100,
        now: progress,
        text: `${progress}%`,
    }}
>
    <View style={[styles.progressFill, {width: `${progress}%`}]} />
</View>

// Stepper with value context
<View
    accessible={true}
    accessibilityRole="adjustable"
    accessibilityLabel={translate('common.quantity')}
    accessibilityValue={{
        min: 1,
        max: 99,
        now: quantity,
    }}
    accessibilityActions={[
        {name: 'increment'},
        {name: 'decrement'},
    ]}
    onAccessibilityAction={(event) => {
        if (event.nativeEvent.actionName === 'increment') {
            increment();
        } else if (event.nativeEvent.actionName === 'decrement') {
            decrement();
        }
    }}
>
    <Pressable onPress={decrement} accessibilityLabel={translate('common.decrease')}>
        <Text>-</Text>
    </Pressable>
    <Text>{quantity}</Text>
    <Pressable onPress={increment} accessibilityLabel={translate('common.increase')}>
        <Text>+</Text>
    </Pressable>
</View>

// Rating with text value (clearer than numbers)
<View
    accessible={true}
    accessibilityLabel={translate('common.rating')}
    accessibilityValue={{
        text: translate('common.ratingOutOf', {rating, max: 5}),
    }}
>
    {[1, 2, 3, 4, 5].map((star) => (
        <Icon
            key={star}
            src={star <= rating ? Expensicons.StarFilled : Expensicons.Star}
        />
    ))}
</View>
```

---

### Review Metadata

Flag ONLY when ALL of these are true:

- Element represents a range-based or adjustable value (slider, progress, stepper, rating)
- Element does NOT have `accessibilityValue` prop
- Value information is important for user understanding

**DO NOT flag if:**

- Element has `accessibilityValue` prop with appropriate fields
- Element uses a library component that handles value internally
- Element is purely decorative progress indicator
- Value is already communicated via `accessibilityLabel`

**Search Patterns** (hints for reviewers):
- `Slider`
- `accessibilityValue`
- `accessibilityRole="adjustable"`
- `accessibilityRole="progressbar"`
- `progressBar`
- `stepper`
- `rating`
- `increment`
- `decrement`
