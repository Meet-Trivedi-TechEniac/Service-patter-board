# Calendar Application - Modular Documentation

## Overview

The Calendar Application is a production-grade, modular JavaScript application that provides a resource timeline view for managing events and resources. It has been refactored from a monolithic structure into a well-organized, maintainable, and scalable modular architecture.

## Architecture

### Module Structure

The application is organized into the following modules:

1. **Utils** - Utility functions and helpers
2. **DataManager** - Data management and state
3. **FilterManager** - Filtering and search functionality
4. **TooltipManager** - Tooltip initialization and management
5. **HeightSyncManager** - Dynamic height synchronization
6. **Renderer** - UI rendering and calendar creation
7. **EventHandlers** - Event listener management
8. **Initializer** - Application initialization

### Design Patterns

- **Module Pattern**: Each module is encapsulated with its own scope
- **Observer Pattern**: Used for filter state management
- **Factory Pattern**: Calendar creation and configuration
- **Singleton Pattern**: Global application instance

## Module Details

### 1. Utils Module

**Purpose**: Provides utility functions used throughout the application.

**Key Functions**:
- `getById(id)` - Safe element retrieval with error handling
- `debounce(func, delay)` - Performance optimization for frequent events
- `querySelector(selector, parent)` - Safe DOM querying
- `formatDuration(startMs, endMs)` - Duration formatting
- `escapeHtml(str)` - HTML escaping for security

**Usage**:
```javascript
const element = CalendarApp.Utils.getById('my-element');
const debouncedFunction = CalendarApp.Utils.debounce(myFunction, 500);
```

### 2. DataManager Module

**Purpose**: Manages application data state including events and resources.

**Key Functions**:
- `setInitialData()` - Loads initial calendar data
- `setLeaveData()` - Loads leave-specific data
- `getEvents()` - Returns current events
- `getResources()` - Returns current resources
- `updateEvents(events)` - Updates events data
- `updateResources(resources)` - Updates resources data

**Data Structure**:
```javascript
// Event Structure
{
  resourceId: "1",
  start: new Date("2025-07-28T11:45:00+05:30"),
  end: new Date("2025-07-29T13:00:00+05:30"),
  id: "123",
  type: "Full",
  editable: false,
  className: ["ec-event-active"],
  extendedProps: {
    employeeID: "100123",
    employeeName: "Diana Alexiou",
    address: "12 King Street, Newtown NSW 2042",
    careerType: "Care Type xyz",
    bookingStatus: "Scheduled",
    region: "Bankstown",
    eventType: "Care Worker"
  }
}

// Resource Structure
{
  id: 1,
  extendedProps: {
    name: "Diana Alexiou",
    totalTime: "244h 29m",
    imgUrl: "Assets/profiles/R1.jpg"
  }
}
```

### 3. FilterManager Module

**Purpose**: Handles all filtering, searching, and sorting operations.

**State Management**:
```javascript
state: {
  region: null,
  worktype: null,
  search: "",
  sortAsc: true
}
```

**Key Functions**:
- `reset()` - Resets all filters to default
- `setRegion(region)` - Sets region filter
- `setWorktype(worktype)` - Sets work type filter
- `setSearch(search)` - Sets search query
- `toggleSort()` - Toggles sort order
- `getFilteredEventIds()` - Gets filtered event IDs
- `getFilteredAndSearchedResources()` - Gets filtered resources
- `applyFilters()` - Applies all current filters

**Filter Logic**:
1. Filters events by region and work type
2. Maps filtered events to resource IDs
3. Filters resources by search query
4. Sorts resources alphabetically
5. Falls back to all resources if no matches

### 4. TooltipManager Module

**Purpose**: Manages Bootstrap tooltips for calendar events.

**Key Functions**:
- `disposeAll()` - Removes all existing tooltips
- `initializeAll()` - Initializes tooltips for all elements
- `renderTooltipContent(arg)` - Renders tooltip HTML content

**Tooltip Configuration**:
```javascript
{
  container: '.ec-body',
  boundary: 'clippingParents',
  fallbackPlacements: ['top', 'bottom', 'left', 'right']
}
```

### 5. HeightSyncManager Module

**Purpose**: Synchronizes dynamic heights for calendar elements.

**Key Functions**:
- `syncDynamicHeight()` - Calculates and applies dynamic heights

**Height Calculation Logic**:
1. Finds all event containers
2. Calculates maximum event offset
3. Adds height offset (82px)
4. Applies CSS custom properties for borders

### 6. Renderer Module

**Purpose**: Handles all UI rendering and calendar creation.

**Key Functions**:
- `createCalendar()` - Creates EventCalendar instance
- `parseDate(date)` - Formats date headers with week parity
- `renderEventDetails(arg)` - Renders event HTML
- `renderResources(info)` - Renders resource HTML
- `renderSearch()` - Creates search interface
- `reRenderEvents()` - Re-renders all events
- `updateResources(data)` - Updates resources display

**Calendar Configuration**:
```javascript
{
  view: 'resourceTimelineDay',
  initialView: 'resourceTimelineDay',
  slotWidth: '249',
  slotHeight: '80',
  headerToolbar: false,
  editable: false,
  slotEventOverlap: true,
  slotMinTime: '9:00:00',
  slotMaxTime: '20:00:00'
}
```

### 7. EventHandlers Module

**Purpose**: Manages all event listeners and user interactions.

**Key Functions**:
- `setupFilterDropdownsAndReset()` - Sets up filter dropdowns
- `setupTabSwitching()` - Sets up tab switching
- `setupTodayButton()` - Sets up today button

**Event Handling**:
- Filter dropdown selections
- Tab switching (Initial/Leave)
- Today button navigation
- Search input with debouncing
- Sort button toggles

### 8. Initializer Module

**Purpose**: Handles application initialization and startup.

**Key Functions**:
- `init()` - Initializes the entire application

**Initialization Sequence**:
1. Create calendar instance
2. Set initial data
3. Setup event handlers
4. Setup filter dropdowns
5. Setup tab switching
6. Setup today button

## Public API

The application exposes a public API for external integration:

```javascript
// Core modules
CalendarApp.DataManager
CalendarApp.FilterManager
CalendarApp.TooltipManager
CalendarApp.HeightSyncManager
CalendarApp.Renderer
CalendarApp.EventHandlers
CalendarApp.Initializer
CalendarApp.Utils

// Public methods
CalendarApp.init()                    // Initialize application
CalendarApp.getCalendar()             // Get calendar instance
CalendarApp.getData()                 // Get current data
CalendarApp.updateData(events, resources) // Update data
CalendarApp.applyFilters()            // Apply current filters
CalendarApp.resetFilters()            // Reset all filters
CalendarApp.syncHeight()              // Sync dynamic heights
CalendarApp.reinitializeTooltips()    // Reinitialize tooltips
```

## Configuration

The application uses a centralized configuration object:

```javascript
const CONFIG = {
  SELECTORS: {
    // DOM selectors
  },
  CALENDAR_OPTIONS: {
    // Calendar configuration
  },
  TOOLTIP_CONFIG: {
    // Tooltip configuration
  },
  DEBOUNCE_DELAY: 500,
  HEIGHT_OFFSET: 82,
  WEEK_COLORS: {
    1: '#ff0026ff',
    2: '#225f27ff'
  }
};
```

## Error Handling

The application includes comprehensive error handling:

- DOM element validation
- Calendar library availability checks
- Tooltip initialization error handling
- Data validation
- Graceful fallbacks for missing elements

## Performance Optimizations

1. **Debouncing**: Search input uses debouncing to prevent excessive API calls
2. **Event Delegation**: Efficient event handling
3. **Lazy Initialization**: Components initialized only when needed
4. **Memory Management**: Proper cleanup of tooltips and event listeners
5. **DOM Caching**: Selectors cached for performance

## Browser Compatibility

- Modern browsers with ES6+ support
- Bootstrap 5.x for tooltips
- EventCalendar library
- jQuery (for daterangepicker integration)
- Moment.js (for date handling)

## Dependencies

- EventCalendar library
- Bootstrap 5.x
- jQuery (optional, for daterangepicker)
- Moment.js (optional, for date formatting)

## Usage Examples

### Basic Initialization
```javascript
// Automatic initialization on DOM ready
// Or manual initialization
CalendarApp.init();
```

### Data Management
```javascript
// Get current data
const data = CalendarApp.getData();

// Update data
CalendarApp.updateData(newEvents, newResources);
```

### Filter Management
```javascript
// Apply filters
CalendarApp.applyFilters();

// Reset filters
CalendarApp.resetFilters();
```

### Height Synchronization
```javascript
// Manual height sync
CalendarApp.syncHeight();
```

## Troubleshooting

### Common Issues

1. **Calendar not rendering**: Check EventCalendar library availability
2. **Tooltips not working**: Verify Bootstrap is loaded
3. **Filters not working**: Check DOM element selectors
4. **Height sync issues**: Verify CSS custom properties support

### Debug Mode

Enable debug logging by checking browser console for detailed error messages and initialization logs.

## Future Enhancements

1. **TypeScript Migration**: Add type safety
2. **Unit Testing**: Comprehensive test coverage
3. **Performance Monitoring**: Add performance metrics
4. **Accessibility**: Improve ARIA support
5. **Internationalization**: Multi-language support
6. **Plugin System**: Extensible architecture
7. **State Management**: Advanced state management
8. **Offline Support**: Service worker integration 