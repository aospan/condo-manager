# Quickstart: Embedded Status Notifications

**Feature**: 002-do-not-show  
**Date**: 2025-09-29  
**Status**: Ready for Implementation

## Overview

This quickstart guide demonstrates how to use the embedded status notification system to replace popup dialogs with inline status messages in the condo management application.

## Prerequisites

- Condo management application running (frontend + backend)
- Modern web browser (Chrome 90+, Firefox 88+, Safari 14+, Edge 90+)
- Basic understanding of JavaScript and HTML

## Quick Test Scenarios

### Scenario 1: Success Message Display
**Goal**: Verify success messages appear inline instead of popups

**Steps**:
1. Open the condo management application
2. Navigate to the Residents page
3. Click "Add Resident" button
4. Fill in resident details:
   - Name: "John Doe"
   - Address: "123 Main St, Apt 1A"
5. Click "Save" button
6. **Expected Result**: Green success message appears at top of page saying "Resident John Doe created successfully"
7. **Expected Result**: No popup dialog appears
8. **Expected Result**: Message auto-dismisses after 5 seconds

### Scenario 2: Error Message Display
**Goal**: Verify error messages appear inline with proper formatting

**Steps**:
1. Navigate to the Residents page
2. Click "Add Resident" button
3. Leave required fields empty
4. Click "Save" button
5. **Expected Result**: Red error message appears at top of page
6. **Expected Result**: Message includes clear error description
7. **Expected Result**: Message includes "Retry" action button
8. **Expected Result**: Message does not auto-dismiss (requires manual dismiss)

### Scenario 3: Multiple Message Handling
**Goal**: Verify new messages replace previous messages

**Steps**:
1. Create a resident (triggers success message)
2. Immediately try to create another resident with invalid data
3. **Expected Result**: Error message replaces success message
4. **Expected Result**: Only one message visible at a time
5. **Expected Result**: No message queue or stacking

### Scenario 4: Manual Message Dismissal
**Goal**: Verify users can manually dismiss messages

**Steps**:
1. Create a resident (triggers success message)
2. Click the "X" button on the message
3. **Expected Result**: Message disappears immediately
4. **Expected Result**: No auto-dismiss occurs

### Scenario 5: Accessibility Features
**Goal**: Verify status messages are accessible to screen readers

**Steps**:
1. Enable screen reader (or browser accessibility tools)
2. Create a resident (triggers success message)
3. **Expected Result**: Screen reader announces the message
4. **Expected Result**: Message has proper ARIA attributes
5. **Expected Result**: Focus management works correctly

## Integration Examples

### JavaScript Usage

```javascript
// Show success message
StatusManager.show({
  type: 'success',
  title: 'Resident Created',
  message: 'John Doe has been successfully added.',
  dismissible: true,
  autoDismiss: true,
  dismissTimeout: 5000
});

// Show error message with action
StatusManager.show({
  type: 'error',
  title: 'Validation Error',
  message: 'Please check the required fields.',
  dismissible: true,
  autoDismiss: false,
  actions: [
    {
      label: 'Retry',
      action: 'retry',
      style: 'primary',
      callback: 'handleRetry'
    }
  ]
});

// Dismiss current message
StatusManager.dismiss();

// Clear all messages
StatusManager.clear();
```

### CSS Classes

```css
/* Status message container */
.status-message {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1000;
  max-width: 500px;
  padding: 16px;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  animation: slideDown 0.3s ease-out;
}

/* Message types */
.status-message.success {
  background-color: #d4edda;
  border: 1px solid #c3e6cb;
  color: #155724;
}

.status-message.error {
  background-color: #f8d7da;
  border: 1px solid #f5c6cb;
  color: #721c24;
}

.status-message.info {
  background-color: #d1ecf1;
  border: 1px solid #bee5eb;
  color: #0c5460;
}

.status-message.warning {
  background-color: #fff3cd;
  border: 1px solid #ffeaa7;
  color: #856404;
}
```

## Troubleshooting

### Common Issues

**Issue**: Messages not appearing
- **Check**: JavaScript console for errors
- **Check**: StatusManager module is loaded
- **Check**: CSS styles are applied

**Issue**: Messages not auto-dismissing
- **Check**: `autoDismiss` is set to `true`
- **Check**: `dismissTimeout` is a positive number
- **Check**: No JavaScript errors preventing timeout

**Issue**: Messages not accessible
- **Check**: ARIA attributes are present
- **Check**: Screen reader compatibility
- **Check**: Keyboard navigation works

**Issue**: Messages overlapping content
- **Check**: CSS z-index is high enough
- **Check**: Position is fixed, not absolute
- **Check**: No conflicting CSS rules

### Debug Mode

Enable debug mode to see detailed logging:

```javascript
StatusManager.debug = true;
```

This will log all status message operations to the console.

## Performance Considerations

- **Message Creation**: < 1ms
- **Message Display**: < 100ms
- **Auto-dismiss**: Precise timing
- **Memory Usage**: Minimal (messages not persisted)
- **Animation**: Hardware-accelerated CSS transitions

## Browser Support

- **Chrome**: 90+ (full support)
- **Firefox**: 88+ (full support)
- **Safari**: 14+ (full support)
- **Edge**: 90+ (full support)
- **IE**: Not supported (use polyfills if needed)

## Next Steps

1. **Implementation**: Follow the tasks.md for step-by-step implementation
2. **Testing**: Run the test scenarios above
3. **Integration**: Integrate with existing modules
4. **Customization**: Adjust styling and behavior as needed
5. **Documentation**: Update user documentation with new UI patterns
