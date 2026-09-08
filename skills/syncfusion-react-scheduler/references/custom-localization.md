# Custom Localization

## Table of Contents

- [Custom Localization](#custom-localization)
  - [Table of Contents](#table-of-contents)
  - [Custom Locale Strings](#custom-locale-strings)
    - [Available Locale Keys](#available-locale-keys)
    - [Loading Custom Locale Strings](#loading-custom-locale-strings)

## Custom Locale Strings

The Scheduler allows customization of all static text through the L10n (Localization) library. This enables complete translation of the UI for any language.

### Available Locale Keys

The Scheduler uses two main locale namespaces: `schedule` and `recurrenceeditor`.

**Schedule Locale Keys:**

```typescript
{
  "schedule": {
    // View names
    "day": "Day",
    "week": "Week",
    "workWeek": "Work Week",
    "month": "Month",
    "year": "Year",
    "agenda": "Agenda",
    "weekAgenda": "Week Agenda",
    "workWeekAgenda": "Work Week Agenda",
    "monthAgenda": "Month Agenda",
    
    // Timeline views
    "timelineDay": "Timeline Day",
    "timelineWeek": "Timeline Week",
    "timelineWorkWeek": "Timeline Work Week",
    "timelineMonth": "Timeline Month",
    "timelineYear": "Timeline Year",
    
    // Navigation
    "today": "Today",
    "previous": "Previous",
    "next": "Next",
    
    // Event messages
    "noEvents": "No events",
    "emptyContainer": "There are no events scheduled on this day.",
    
    // Event fields
    "allDay": "All day",
    "start": "Start",
    "end": "End",
    "subject": "Subject",
    "title": "Title",
    "location": "Location",
    "description": "Description",
    "timezone": "Timezone",
    "startTimezone": "Start Timezone",
    "endTimezone": "End Timezone",
    "repeat": "Repeat",
    
    // Actions
    "more": "more",
    "moreEvents": "More Events",
    "moreDetails": "More Details",
    "close": "Close",
    "cancel": "Cancel",
    "save": "Save",
    "delete": "Delete",
    "edit": "Edit",
    "createEvent": "Create",
    "newEvent": "New Event",
    "addTitle": "Add title",
    "noTitle": "(No Title)",
    
    // Dialog titles
    "deleteEvent": "Delete Event",
    "deleteMultipleEvent": "Delete Multiple Events",
    "deleteSeries": "Delete Series",
    "editEvent": "Edit Event",
    "editSeries": "Edit Series",
    "editFollowingEvent": "Following Events",
    "editTitle": "Edit Event",
    "deleteTitle": "Delete Event",
    
    // Button labels
    "saveButton": "Save",
    "cancelButton": "Cancel",
    "deleteButton": "Delete",
    "ok": "Ok",
    "yes": "Yes",
    "no": "No",
    
    // Recurrence
    "recurrence": "Recurrence",
    "editRecurrence": "Edit Recurrence",
    "repeats": "Repeats",
    "occurrence": "Occurrence",
    "series": "Series",
    
    // Messages and alerts
    "alert": "Alert",
    "selectedItems": "Items selected",
    "editContent": "Do you want to edit only this event or entire series?",
    "deleteRecurrenceContent": "Do you want to delete only this event or entire series?",
    "deleteContent": "Are you sure you want to delete this event?",
    "deleteMultipleContent": "Are you sure you want to delete the selected events?",
    "wrongPattern": "The recurrence pattern is not valid.",
    "startEndError": "The selected end date occurs before the start date.",
    "invalidDateError": "The entered date value is invalid.",
    "blockAlert": "Events cannot be scheduled within the blocked time range.",
    "sameDayAlert": "Two occurrences of the same event cannot occur on the same day.",
    "occurenceAlert": "Cannot reschedule an occurrence of the recurring appointment if it skips over a later occurrence of the same appointment.",
    "createError": "The duration of the event must be shorter than how frequently it occurs. Shorten the duration, or change the recurrence pattern in the recurrence event editor.",
    "recurrenceDateValidation": "Some months have fewer than the selected date. For these months, the occurrence will fall on the last date of the month.",
    "seriesChangeAlert": "The changes made to specific instances of this series will be cancelled and those events will match the series again.",
    
    // Other
    "beginFrom": "Begin From",
    "endAt": "End At",
    "searchTimezone": "Search Timezone",
    "noRecords": "No records found",
    "of": "of",
    "expandAllDaySection": "Expand",
    "collapseAllDaySection": "Collapse"
  }
}
```

**Recurrence Editor Locale Keys:**

```typescript
{
  "recurrenceeditor": {
    "none": "None",
    "daily": "Daily",
    "weekly": "Weekly",
    "monthly": "Monthly",
    "month": "Month",
    "yearly": "Yearly",
    "never": "Never",
    "until": "Until",
    "count": "Count",
    "first": "First",
    "second": "Second",
    "third": "Third",
    "fourth": "Fourth",
    "last": "Last",
    "repeat": "Repeat",
    "repeatEvery": "Repeat Every",
    "on": "Repeat On",
    "end": "End",
    "onDay": "Day",
    "days": "Day(s)",
    "weeks": "Week(s)",
    "months": "Month(s)",
    "years": "Year(s)",
    "every": "every",
    "summaryTimes": "time(s)",
    "summaryOn": "on",
    "summaryUntil": "until",
    "summaryRepeat": "Repeats",
    "summaryDay": "day(s)",
    "summaryWeek": "week(s)",
    "summaryMonth": "month(s)",
    "summaryYear": "year(s)"
  }
}
```

### Loading Custom Locale Strings

Use the `L10n.load()` method to register custom locale strings before creating the Scheduler component.

**Example: French Locale Strings**

```typescript
import React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Inject,
  ViewsDirective, ViewDirective
} from '@syncfusion/ej2-react-schedule';
import { L10n, loadCldr } from '@syncfusion/ej2-base';
import { scheduleData } from './datasource';

// Import CLDR data
import frNumberData from '@syncfusion/ej2-cldr-data/main/fr-CH/numbers.json';
import frTimeZoneData from '@syncfusion/ej2-cldr-data/main/fr-CH/timeZoneNames.json';
import frGregorian from '@syncfusion/ej2-cldr-data/main/fr-CH/ca-gregorian.json';
import frNumberingSystem from '@syncfusion/ej2-cldr-data/supplemental/numberingSystems.json';

// Load CLDR data
loadCldr(frNumberData, frTimeZoneData, frGregorian, frNumberingSystem);

// Load French locale strings
L10n.load({
  'fr-CH': {
    'schedule': {
      'day': 'Jour',
      'week': 'Semaine',
      'workWeek': 'Semaine de travail',
      'month': 'Mois',
      'year': 'Année',
      'agenda': 'Ordre du jour',
      'today': "Aujourd'hui",
      'noEvents': 'Aucun événement',
      'emptyContainer': "Aucun événement n'est prévu pour ce jour.",
      'allDay': 'Toute la journée',
      'start': 'Début',
      'end': 'Fin',
      'more': 'plus',
      'close': 'Fermer',
      'cancel': 'Annuler',
      'noTitle': '(Sans titre)',
      'delete': 'Supprimer',
      'deleteEvent': "Supprimer l'événement",
      'deleteMultipleEvent': 'Supprimer plusieurs événements',
      'selectedItems': 'Éléments sélectionnés',
      'deleteSeries': 'Supprimer la série',
      'edit': 'Modifier',
      'editSeries': 'Modifier la série',
      'editEvent': "Modifier l'événement",
      'createEvent': 'Créer',
      'subject': 'Sujet',
      'addTitle': 'Ajouter un titre',
      'moreDetails': 'Plus de détails',
      'save': 'Enregistrer',
      'editContent': 'Voulez-vous modifier uniquement cet événement ou toute la série?',
      'deleteRecurrenceContent': 'Voulez-vous supprimer uniquement cet événement ou toute la série?',
      'deleteContent': 'Êtes-vous sûr de vouloir supprimer cet événement?',
      'deleteMultipleContent': 'Êtes-vous sûr de vouloir supprimer les événements sélectionnés?',
      'newEvent': 'Nouvel événement',
      'title': 'Titre',
      'location': 'Lieu',
      'description': 'Description',
      'timezone': 'Fuseau horaire',
      'startTimezone': 'Fuseau horaire de début',
      'endTimezone': 'Fuseau horaire de fin',
      'repeat': 'Répéter',
      'saveButton': 'Enregistrer',
      'cancelButton': 'Annuler',
      'deleteButton': 'Supprimer',
      'recurrence': 'Récurrence',
      'wrongPattern': "Le modèle de récurrence n'est pas valide.",
      'ok': 'Ok',
      'yes': 'Oui',
      'no': 'Non'
    },
    'recurrenceeditor': {
      'none': 'Aucun',
      'daily': 'Quotidien',
      'weekly': 'Hebdomadaire',
      'monthly': 'Mensuel',
      'yearly': 'Annuel',
      'never': 'Jamais',
      'until': "Jusqu'à",
      'count': 'Nombre',
      'first': 'Premier',
      'second': 'Deuxième',
      'third': 'Troisième',
      'fourth': 'Quatrième',
      'last': 'Dernier',
      'repeat': 'Répéter',
      'repeatEvery': 'Répéter tous les',
      'on': 'Répéter le',
      'end': 'Fin',
      'onDay': 'Jour',
      'days': 'Jour(s)',
      'weeks': 'Semaine(s)',
      'months': 'Mois',
      'years': 'Année(s)',
      'every': 'tous les',
      'summaryTimes': 'fois',
      'summaryOn': 'le',
      'summaryUntil': "jusqu'à",
      'summaryRepeat': 'Répète',
      'summaryDay': 'jour(s)',
      'summaryWeek': 'semaine(s)',
      'summaryMonth': 'mois',
      'summaryYear': 'année(s)'
    }
  }
});

function App() {
  const eventSettings = { dataSource: scheduleData };

  return (
    <ScheduleComponent
      height='550px'
      selectedDate={new Date(2018, 1, 15)}
      locale='fr-CH'
      eventSettings={eventSettings}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='WorkWeek' />
        <ViewDirective option='Month' />
      </ViewsDirective>
      <Inject services={[Day, Week, WorkWeek, Month]} />
    </ScheduleComponent>
  );
}

export default App;
```

**Loading from External JSON File:**

```typescript
import { Ajax, L10n } from '@syncfusion/ej2-base';

// Load locale strings from external file
let localeTexts: string;
let ajax: Ajax = new Ajax('./locale.json', 'GET', false);
ajax.onSuccess = (value: string) => {
  localeTexts = value;
};
ajax.send();
L10n.load(JSON.parse(localeTexts));
```

**External JSON File Structure (locale.json):**

```json
{
  "fr-CH": {
    "schedule": {
      "day": "Jour",
      "week": "Semaine",
      ...
    },
    "recurrenceeditor": {
      "none": "Aucun",
      ...
    }
  },
  "de-DE": {
    "schedule": {
      "day": "Tag",
      "week": "Woche",
      ...
    }
  }
}
```

**Summary:**
This reference provides comprehensive guidance for implementing accessibility and localization features in the Syncfusion React Scheduler. By following these guidelines, you can create inclusive, globally-ready scheduling applications that meet WCAG 2.2 standards and support diverse user needs across different regions and abilities.
