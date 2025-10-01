# Data Model: Embedded Status Notifications

**Feature**: 002-do-not-show  
**Date**: 2025-09-29  
**Status**: Complete

## Entities

### StatusMessage
Represents a notification displayed to the user within the page interface.

**Attributes**:
- `id`: string (unique identifier)
- `type`: enum (success, error, info, warning)
- `title`: string (optional short title)
- `message`: string (main message content)
- `timestamp`: datetime (when message was created)
- `dismissible`: boolean (whether user can manually dismiss)
- `autoDismiss`: boolean (whether message auto-dismisses)
- `dismissTimeout`: number (auto-dismiss timeout in milliseconds)
- `actions`: array of ActionButton (optional action buttons)

**State Transitions**:
- `created` → `displayed` (immediately after creation)
- `displayed` → `dismissed` (user manually dismisses or auto-dismiss timeout)
- `displayed` → `replaced` (new message replaces this one)

**Validation Rules**:
- `message` must not be empty
- `type` must be one of: success, error, info, warning
- `dismissTimeout` must be positive number if `autoDismiss` is true
- `title` is optional but recommended for error messages

### ActionButton
Represents an optional action button within a status message.

**Attributes**:
- `label`: string (button text)
- `action`: string (action identifier)
- `style`: enum (primary, secondary, danger)
- `callback`: function (optional callback function)

**Validation Rules**:
- `label` must not be empty
- `action` must be unique within the message
- `style` must be one of: primary, secondary, danger

### StatusManager
Represents the central manager for status message lifecycle.

**Attributes**:
- `currentMessage`: StatusMessage | null (currently displayed message)
- `messageHistory`: array of StatusMessage (recent messages for debugging)
- `maxHistorySize`: number (maximum history entries to keep)

**Methods**:
- `show(message: StatusMessage)`: Display a new message
- `dismiss()`: Manually dismiss current message
- `clear()`: Clear all messages
- `getHistory()`: Get recent message history

## Relationships

- **StatusManager** has one **StatusMessage** (current)
- **StatusMessage** has zero or more **ActionButton** (actions)
- **StatusManager** has many **StatusMessage** (history)

## Data Flow

1. **Message Creation**: User action triggers status message creation
2. **Message Display**: StatusManager shows message in UI
3. **Message Lifecycle**: Auto-dismiss or manual dismiss
4. **Message Replacement**: New message replaces current message
5. **Message History**: Dismissed messages added to history

## Integration Points

### Frontend Modules
- **residents.js**: Show success/error messages for resident operations
- **guests.js**: Show success/error messages for guest operations
- **vehicles.js**: Show success/error messages for vehicle operations
- **permits.js**: Show success/error messages for permit operations

### Error Handling
- **API Errors**: Convert HTTP error responses to status messages
- **Validation Errors**: Display field-specific validation errors
- **Network Errors**: Show user-friendly network error messages

### Success Feedback
- **Create Operations**: "Successfully created [entity]"
- **Update Operations**: "Successfully updated [entity]"
- **Delete Operations**: "Successfully deleted [entity]"
- **Bulk Operations**: "Successfully processed X items"

## State Management

### Message Queue
- Single message display (replace previous)
- No queuing of multiple messages
- Immediate replacement of current message

### Persistence
- No persistent storage required
- Messages exist only in memory during session
- History limited to recent messages for debugging

### Cleanup
- Auto-dismiss after 5 seconds (configurable)
- Manual dismiss via user action
- Automatic cleanup of old history entries
