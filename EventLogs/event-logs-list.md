# EventLogs list

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of content

- [Questions](#questions)
- [Logs objects](#logs-objects)
- [Event types](#event-types)
  - [Localization events](#localization-events)
  - [Lifecycle events](#lifecycle-events)
  - [Network events](#network-events)
  - [Navigation events](#navigation-events)
  - [Action events](#action-events)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Questions

* Est-ce que les événements doivent être loggés dès que l'action est entreprise ou seulement lorsque l'action est validée (genre les actions qui ont un impact sur la BD) ?
* Mettre les événements du cycle de vie d'une excursion dans action ou dans lifecycle ? (Je penche pour lifecycle)
* Lorsque la vue AR se lance, l'app est considérée comme "en pause"...

## Logs objects

The log objects are send to the backend and each represent an event in the app.
Their structure is as follow:

```json
{
  "occurredAt": "A string representing the moment of the event, in ISO format",
  "type": "The type of the event (see below)",
  "version": "The version of the structure on which this log is based",
  "properties": "An object whose properties vary depending on the type of the log (see below)"
}
```

## Event types

### Localization events

#### `localization`

> Fired each time the user's position is localized by their device, and that this new position is at least 10 meters farer from the previous localized position.

**Properties object:**

```json
{
  "excursionId": "The serverId of the excursion in the context of which the position has been localized",
  "position": {
    "latitude": "The new position's latitude",
    "longitude": "The new positions's longitude",
    "altitude": "The new positions' altitude, if known",
    "accuracy": "The localization's accuracy"
  },
  "context": "The context in which the position has been localized. This can be either on an excursion's card page (excursionCard) or when in the AR view (ar)."
}
```

### Lifecycle events

#### `lifecycle.app.started`

> Fired each time the user starts the BioSentiers app.

**Properties object:**

```json
{}
```

#### `lifecycle.app.paused`

> Fired each time the user put the BioSentiers app in the background.

**Properties object:**

```json
{}
```

#### `lifecycle.app.resumed`

> Fired each time the user resumes the BioSentiers app from the background.

**Properties object:**

```json
{}
```

#### `lifecycle.ar.launched`

> Fired each time the AR view is launched.

**Properties object:**

```json
{}
```

#### `lifecycle.ar.quitted`

> Fired each time the AR view is quitted.

**Properties object:**

```json
{}
```

### Network events

#### `network.offline`

> Fired each time the device loses network connectivity

**Properties object:**

```json
{}
```

#### `network.online`

> Fired each time the device gains or has network connectivity

**Properties object:**

```json
{
  "connectionType": "The type of the network connectivity at the moment the event is fired."
}
```

### Navigation events

#### `navigation.menuOpen`

> Fired each time the user opens the sidemenu

**Properties object:**

```json
{
  "fromState": {
    "url": "The URL of the state from which the user has opened the menu (accessed through the $state service).",
    "name": "The name of the state from which the user has opened the menu (accessed through the $state service)."
  }
}
```

#### `navigation.connect`

> Fired each time the user navigates or is redirected to the page where he can scan an excursion's QR code.

**Properties object:**

```json
{}
```

#### `navigation.excursionsList.all`, `navigation.excursionsList.pending`, `navigation.excursionsList.finished`, `navigation.excursionsList.finished`

> Fired each time the user navigates to any excursions list tabs

**Properties object:**

```json
{
  "tab": "The name of the tab to which the user has navigated.",
  "state": {
    "name": "The name of the state to which the user has navigated (accessed through the $state service).",
    "url": "The URL of the state to which the user has navigated. (accessed through the $state service)."
  }
}
```

#### `navigation.excursion.card`

> Fired each time the user navigates to an excursion card

**Properties object:**

```json
{
  "id": "The server id of the displayed excursion",
  "status": "The current status of the excursion"
}
```

#### `navigation.excursion.seenPois.list`

> Fired each time the user navigates to the seen pois list of an excursion.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the excursion of which the list of seen pois is being displayed",
    "status": "The status of the excursion of which the list of seen pois is being displayed" 
  }
}
```

#### `navigation.excursion.seenPois.card`

> Fired each time the user navigates to the card of an excursion's seen poi.

**Properties object:**

```json
{
  "specieId": "The id of the specie whose information are displayed on the seen poi card."s
}
```

### Action events

#### `action.scanQr.new`

> Fired each time the user scans a QR code of an excursion that is not present in their device, event with a different participant.

**Properties object:**

```json
{
  "excursionId": "The excursion server id from the scanned QR code",
  "participantId": "The participant id from the scanned QR code"
}
```

#### `action.scanQr.different`

> Fired each time the user scans a QR code of an excursion that is already present in their device, but with a different participant.

**Properties object:**

```json
{
  "excursionId": "The excursion server id from the scanned QR code",
  "participantId": "The participant id from the scanned QR code",
  "existingParticipants": ["array", "of", "existing", "participants", "id"]
}
```

#### `action.scanQr.identical`

> Fired each time the user scans a QR code of an excursion that is already present in their device, with the same participant.

**Properties object:**

```json
{
  "excursionId": "The excursion server id from the scanned QR code",
  "participantId": "The participant id from the scanned QR code"
}
```

#### `action.excursionsList.excursionActionSheet`

> Fired each time the user shows the Action Sheet relative to an excursion from any excursions list.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the excursion whose Action Sheet is being showed",
    "status": "The status of the excursion whose Action Sheet is being showed"
}
```

#### `action.excursionsList.contextMenu`

> Fired each time the user opens any excursions list context menu.

**Properties object:**

```json
{}
```

#### `action.excursionsList.archives.showed`

> Fired each time the user shows the archived excursions in any excursion list.

**Properties object:**

```json
{}
```

#### `action.excursionsList.archives.hidden`

> Fired each time the user hide the archived excursions in any excursion list.

**Properties object:**

```json
{}
```

#### `action.excursion.contextMenu`

> Fired each time the user opens the context menu of an excursion card page.

**Properties object:**

```json
{}
```

#### `action.excursion.created`

> Fired each time an excursion is created on the device's database.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the created excursion",
    "addedAt": "The date at which the excursion has been created in the database"
  }
}
```

#### `action.excursion.archived`

> Fired each time an excursion is archived by the user.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the archived excursion",
    "addedAt": "The date at which the excursion has been created in the database"
  }
}
```

#### `action.excursion.restored`

> Fired each time an excursion is restored by the user.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the restored excursion",
    "addedAt": "The date at which the excursion has been created in the database",
    "archivedAt": "The date at which the excursion has been archived by the user"
  }
}
```

#### `action.excursion.deleted`

> Fired each time an excursion is deleted by the user.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the deleted excursion",
    "addedAt": "The date at which the excursion has been created in the database",
    "archivedAt": "The date at which the excursion has been archived by the user"
  }
}
```

#### `action.excursion.flagAsNew`

> Fired each time the user manually flags an excursion as 'new'.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the excursion",
    "addedAt": "The date at which the excursion has been created in the database"
  }
}
```

#### `action.excursion.unflagAsNew`

> Fired each time the user manually removes the flag 'new' on an excursion.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the excursion",
    "addedAt": "The date at which the excursion has been created in the database"
  }
}
```

#### `action.excursion.reinitialized`

> Fired each time an excursion is reinitialized by the user.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the reinitialized excursion",
    "startedAt": "The date at which the excursion has been first started by the user",
    "finishedAt": "The date at which the excursion has been finished by the user"
  }
}
```

#### `action.excursion.started`

> Fired each time a pending excursion is started for the first time.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the started excursion",
    "addedAt": "The date at which the excursion has been created in the database"
  }
}
```

#### `action.excursion.paused`

> Fired each time an ongoing excursion is paused by the user (i.e. the user quit the AR view without having finished the excursion).

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the paused excursion",
    "addedAt": "The date at which the excursion has been created in the database",
    "startedAt": "The date at which the excursion has been first started by the user",
  }
}
```

#### `action.excursion.resumed`

> Fired each time an ongoing excursion is resumed by the user.

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the resumed excursion",
    "addedAt": "The date at which the excursion has been created in the database",
    "startedAt": "The date at which the excursion has been first started by the user",
    "pausedAt": "The date at which the excursion has been paused prior to the resuming"
  }
}
```

#### `action.excursion.finished`

> Fired each time an ongoing excursion is finished by the user (i.e. they has reached the end point).

**Properties object:**

```json
{
  "excursion": {
    "id": "The server id of the finished excursion",
    "addedAt": "The date at which the excursion has been created in the database",
    "startedAt": "The date at which the excursion has been first started by the user",
  }
}
```

#### `action.positionWatcher.activated`

> Fired each time the user manually activates a position watcher (currently only on an excursion card page).

**Properties object:**

```json
{}
```

#### `action.positionWatcher.deactivated`

> Fired each time the user manually deactivate a position watcher (currently only on an excursion card page).

**Properties object:**

```json
{}
```

#### `action.filters.opened`

> Fired each time the user opens the filters modal on the AR view.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion",
  "currentFilters": {
    "themes": "An array of the selected themes name",
    "settings": {
      "showSeenPois": "Indicates if seen pois should be visible in the AR view",
      "showOffSeason": "Indicates if off season species poi should be visible in the AR view"
  }
}
```

#### `action.filters.changed`

> Fired when the user changes the active filters in the AR view (this is debounced in the code to prevent a flood of events).

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion",
  "newFilters": {
    "themes": "An array of the selected themes name",
    "settings": {
      "showSeenPois": "Indicates if seen pois should be visible in the AR view",
      "showOffSeason": "Indicates if off season species poi should be visible in the AR view"
  }
}
```

#### `action.bigmap.opened`

> Fired each time the user clicks on the MiniMap and opens the BigMap.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion"
}
```

#### `action.bigmap.center.onTrail`

> Fired each time the user clicks on the "Center on trail" button in the BigMap.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion"
}
```

#### `action.bigmap.center.onUser`

> Fired each time the user clicks on the "Center on user" button in the BigMap.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion"
}
```

#### `action.bigmap.closed`

> Fired each time the user closes the BigMap.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion"
}
```

#### `action.ar.poi.clicked`

#### `action.ar.poi.checked`

#### `action.ar.poi.closed`

#### `action.ar.end.reached`

#### `action.ar.end.validated`

#### `action.ar.end.refused`

#### `action.ar.end.manual`