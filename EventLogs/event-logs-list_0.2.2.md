# EventLogs list (0.2.1)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of content

- [Questions](#questions)
- [Logs objects](#logs-objects)
- [Event types](#event-types)
  - [Location events](#location-events)
  - [App events](#app-events)
  - [AR events](#ar-events)
  - [Network events](#network-events)
  - [Navigation events](#navigation-events)
  - [Excursion events](#excursion-events)
  - [Action events](#action-events)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Questions

* Est-ce que les événements doivent être loggés dès que l'action est entreprise ou seulement lorsque l'action est validée (genre les actions qui ont un impact sur la BD) ?
* Mettre les événements du cycle de vie d'une excursion dans action ou dans lifecycle ? (Je penche pour lifecycle)

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

### Location events

Events related to the user's location.

#### `location`

> Fired each time the user's position is located by their device, and that the new position is at least 10 meters farer from the previous located position.

**Properties object:**

```json
{
  "excursionId": "The serverId of the excursion in the context of which the position has been located",
  "position": {
    "latitude": "The new position's latitude",
    "longitude": "The new positions's longitude",
    "altitude": "The new positions' altitude, if known",
    "accuracy": "The location's accuracy"
  },
  "context": "The context in which the position has been located. This can be either on an excursion's card page (excursionCard) or when in the AR view (ar)."
}
```

### App events

Events related to the native app lifecycle.

#### `app.started`

> Fired each time the user starts the BioSentiers app.

**Properties object:**

```json
{}
```

#### `app.paused`

> Fired each time the user put the BioSentiers app in the background.

**Properties object:**

```json
{}
```

#### `app.resumed`

> Fired each time the user resumes the BioSentiers app from the background.

**Properties object:**

```json
{}
```

### AR events

Events related to the lifecycle of the AR view.

#### `ar.launched`

> Fired each time the AR view is launched.

**Properties object:**

```json
{}
```

#### `ar.quitted`

> Fired each time the AR view is quitted.

**Properties object:**

```json
{}
```

### Network events

Events related to the device being connected or not to the network.

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

Events related to the user navigation inside the app.

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
  "excursion": {
    "id": "The server id of the excursion of which the list of seen pois is being displayed",
    "status": "The status of the excursion of which the list of seen pois is being displayed" 
  },
  "speciesId": "The id of the species whose information are displayed on the seen poi card."s
}
```

### Excursion events

Events related to the different states of an excursion. These events can be directly triggered by a user's action, or a indirect consequence of another action.

#### `excursion.contextMenu`

> Fired each time the user opens the context menu of an excursion card page.

**Properties object:**

```json
{}
```

#### `excursion.created`

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

#### `excursion.archived`

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

#### `excursion.restored`

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

#### `excursion.deleted`

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

#### `excursion.flagedAsNew`

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

#### `excursion.unflagedAsNew`

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

#### `excursion.reinitialized`

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

#### `excursion.started`

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

#### `excursion.paused`

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

#### `excursion.resumed`

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

#### `excursion.finished`

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

### Action events

Events related to the user's actions inside the app.

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

> Fired each time the user clicks on a POI's ArMarker in the AR view while being too far away.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion",
  "poiId": "The id of the clicked POI",
  "distance": "The distance between the user and the POI at the time of the click"
}
```

#### `action.ar.poi.opened`

> Fired each time the user opens up a POI modal.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion",
  "poiId": "The id of the clicked POI"
}
```

#### `action.ar.poi.checked`

> Fired each time the user declares having seen a POI by clicking the adequate button in the POI card, in the AR view.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion",
  "poiId": "The id of the clicked POI",
  "speciesId": "The id of the species represented by the POI"
}
```

#### `action.ar.poi.closed`

> Fired each time the user closes a POI card modal in the AR view.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion",
  "poiId": "The id of the clicked POI",
  "speciesId": "The id of the species represented by the POI"
}
```

#### `action.ar.end.reached`

> Fired each time the user reaches the end point in the AR view.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion"
}
```

#### `action.ar.end.prompt`

> Fired each time the user interacted with the prompt that offers them to finish the current excursion in the AR view.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion",
  "validated": "The result of the user's interaction with the automatic prompt. 'true' means that the user validated the automatic prompt, 'false' means he cancelled it and will manually finish the excursion at a later time."
}
```

#### `action.ar.end.manual`

> Fired each time the user manually clicked on the "Finish" button in the AR view.

**Properties object:**

```json
{
  "excursionId": "The server id of the current excursion"
}
```
