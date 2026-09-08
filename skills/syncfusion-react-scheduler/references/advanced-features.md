# Advanced Features - React Scheduler

This reference provides comprehensive documentation for advanced features in the Syncfusion React Scheduler component.

## Table of Contents

- [State Persistence](#state-persistence)
- [Exporting](#exporting)
  - [Excel Exporting](#excel-exporting)
  - [ICS File Exporting](#ics-file-exporting)
  - [Importing from ICS Files](#importing-from-ics-files)
  - [Print Functionality](#print-functionality)
- [Clipboard Operations](#clipboard-operations)
  - [Cut, Copy, and Paste Using Keyboard](#cut-copy-and-paste-using-keyboard)
  - [Cut, Copy, and Paste Using Context Menu](#cut-copy-and-paste-using-context-menu)
  - [Modifying Content Before Pasting](#modifying-content-before-pasting)

---

## State Persistence

State persistence allows the Scheduler to retain the `currentView`, `selectedDate`, and scroll position values in the browser's `localStorage` for state maintenance even if the browser is refreshed or you navigate to another page. This behavior is enabled through the `enablePersistence` property, which is disabled by default.

**Key Points:**
- When set to `true`, the Scheduler's `currentView`, `selectedDate`, and scroll position values are preserved after a page refresh
- The Scheduler `id` is required to enable state persistence

**Implementation:**

```typescript
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Inject,
  ViewsDirective, ViewDirective, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };
  
  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings} 
      enablePersistence={true}
    >
      <Inject services={[Day, Week, WorkWeek, Month]} />
    </ScheduleComponent>
  );
};

const root = ReactDOM.createRoot(document.getElementById('schedule'));
root.render(<App />);
```

---

## Exporting

The Scheduler supports exporting appointments to Excel, ICS files, and provides print functionality for generating reports and sharing schedules.

### Excel Exporting

The Scheduler enables exporting events to an Excel file using the `exportToExcel` method. By default, it includes all fields mapped in the `eventSettings` property.

**Prerequisites:**
- Import and inject the `ExcelExport` module from `@syncfusion/ej2-schedule`

#### Basic Excel Export

```typescript
import * as ReactDOM from 'react-dom';
import { useRef } from 'react';
import * as React from 'react';
import { ItemModel } from '@syncfusion/ej2-react-navigations';
import {
  ScheduleComponent, ViewDirective, Week, Resize, ExcelExport, ExportOptions,
  ActionEventArgs, ToolbarActionArgs, DragAndDrop, Inject, ViewsDirective, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const scheduleObj = useRef<ScheduleComponent>(null);
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  const onActionBegin = (args: ActionEventArgs & ToolbarActionArgs): void => {
    if (args.requestType === 'toolbarItemRendering') {
      let exportItem: ItemModel = {
        align: 'Right', 
        showTextOn: 'Both', 
        prefixIcon: 'e-icon-schedule-excel-export',
        text: 'Excel Export', 
        cssClass: 'e-excel-export', 
        click: onExportClick
      };
      args.items.push(exportItem);
    }
  }

  const onExportClick = (): void => {
    scheduleObj.current.exportToExcel();
  }

  return (
    <ScheduleComponent 
      cssClass='excel-export' 
      width='100%' 
      height='550px' 
      id='schedule' 
      ref={scheduleObj}
      selectedDate={new Date(2019, 0, 10)} 
      eventSettings={eventSettings}
      actionBegin={onActionBegin}
    >
      <ViewsDirective>
        <ViewDirective option='Week' />
      </ViewsDirective>
      <Inject services={[Week, Resize, DragAndDrop, ExcelExport]} />
    </ScheduleComponent>
  );
};
```

#### Exporting with Custom Fields

To export only specific fields, define the required fields through the `fields` option in `ExportOptions`:

```typescript
const onExportClick = (): void => {
  let exportValues: ExportOptions = {
    fields: ['Id', 'Subject', 'StartTime', 'EndTime', 'Location']
  };
  scheduleObj.current.exportToExcel(exportValues);
}
```

#### Exporting Individual Occurrences of Recurring Series

By default, recurring events are exported as a single record. To export each occurrence separately, set `includeOccurrences` to `true`:

```typescript
const onExportClick = (): void => {
  let exportValues: ExportOptions = { includeOccurrences: true };
  scheduleObj.current.exportToExcel(exportValues);
}
```

#### Exporting Custom Event Data

To export specific events or custom data collections, pass them through the `customData` option:

```typescript
const onExportClick = (): void => {
  let exportValues: ExportOptions = {
    customData: [
      {
        Id: 1,
        Subject: 'Explosion of Betelgeuse Star',
        Location: 'Space Centre USA',
        StartTime: new Date(2019, 0, 6, 9, 30),
        EndTime: new Date(2019, 0, 6, 11, 0),
        CategoryColor: '#1aaa55'
      },
      {
        Id: 2,
        Subject: 'Thule Air Crash Report',
        Location: 'Newyork City',
        StartTime: new Date(2019, 0, 7, 12, 0),
        EndTime: new Date(2019, 0, 7, 14, 0),
        CategoryColor: '#357cd2'
      }
    ]
  };
  scheduleObj.current.exportToExcel(exportValues);
}
```

#### Customizing Column Headers

Use the `fieldsInfo` option to customize header names when exporting:

```typescript
const onExportClick = (): void => {
  let customFields: ExportFieldInfo[] = [
    { name: 'Subject', text: 'Summary' },
    { name: 'StartTime', text: 'First Date' },
    { name: 'EndTime', text: 'Last Date' },
    { name: 'Location', text: 'Place' },
    { name: 'OwnerId', text: 'Owners' }
  ];
  let exportValues: ExportOptions = { fieldsInfo: customFields };
  scheduleObj.current.exportToExcel(exportValues);
}
```

#### Export with Custom File Name

The default exported file name is `Schedule.xlsx`. Customize it using the `fileName` option:

```typescript
const onExportClick = (): void => {
  let exportValues: ExportOptions = { fileName: "SchedulerData" };
  scheduleObj.current.exportToExcel(exportValues);
}
```

#### Excel File Formats

Export to `.xlsx` or `.csv` formats by setting the `exportType` option:

```typescript
const onExportClick = (): void => {
  let exportValues: ExportOptions = { exportType: "csv" };
  scheduleObj.current.exportToExcel(exportValues);
}
```

#### Custom Separator in CSV

Change the default CSV separator (`,`) using the `separator` property:

```typescript
const onExportClick = (): void => {
  let exportValues: ExportOptions = { 
    exportType: 'csv', 
    separator: ';' 
  };
  scheduleObj.current.exportToExcel(exportValues);
}
```

#### Customizing Excel Sheet Before Export

Use the `excelExport` event to customize the Excel sheet before exporting:

```typescript
const onExcelExport = (args: ExcelExportEventArgs) => {
  const worksheet = args.worksheets[0];
  
  // Add custom header
  worksheet.rows.unshift({
    index: 1,
    cells: [{
      index: 1,
      value: 'Sales Report',
      style: {
        bold: true,
        fontSize: 18,
        hAlign: 'Center',
        fill: { color: '#1E90FF' }, 
        color: '#FFFFFF',
      },
      colSpan: worksheet.columns.length,
    }]
  });
  
  // Add custom footer
  worksheet.rows.push({
    index: worksheet.rows.length + 1,
    cells: [{
      index: 1,
      value: 'End of Report',
      style: {
        bold: true,
        fontSize: 14,
        hAlign: 'Center',
        fill: { color: '#FFD700' },
      },
      colSpan: worksheet.columns.length,
    }]
  });
}
```

### ICS File Exporting

You can export Scheduler events to a calendar (.ics) file format, compatible with Google Calendar, Outlook, and other calendar applications.

**Prerequisites:**
- Import and inject the `ICalendarExport` module from `@syncfusion/ej2-schedule`

#### Basic ICS Export

```typescript
import * as ReactDOM from 'react-dom';
import * as React from 'react';
import { useRef } from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Agenda, 
  ICalendarExport, Inject
} from '@syncfusion/ej2-react-schedule';
import { ButtonComponent } from '@syncfusion/ej2-react-buttons';
import { scheduleData } from './datasource';

const App = () => {
  const scheduleObj = useRef<ScheduleComponent>(null);
  const eventSettings = { dataSource: scheduleData };

  const onClick = (): void => {
    scheduleObj.current.exportToICalendar();
  }
  
  return (
    <div>
      <ButtonComponent id='ics-export' title='Export' onClick={onClick}>
        Export
      </ButtonComponent>
      <ScheduleComponent 
        ref={scheduleObj} 
        width='100%' 
        height='520px' 
        selectedDate={new Date(2018, 1, 15)} 
        eventSettings={eventSettings}
      >
        <Inject services={[Day, Week, WorkWeek, Month, Agenda, ICalendarExport]} />
      </ScheduleComponent>
    </div>
  );
};
```

#### Exporting with Custom File Name

By default, the calendar is exported as `Calendar.ics`. Customize the file name:

```typescript
const onClick = (): void => {
  scheduleObj.current.exportToICalendar('ScheduleEvents');
}
```

### Importing from ICS Files

Import events from external calendars (ICS files) using the `importICalendar` method, which accepts a blob object of an .ics file.

**Prerequisites:**
- Import and inject the `ICalendarImport` module from `@syncfusion/ej2-schedule`

```typescript
import * as ReactDOM from 'react-dom';
import * as React from 'react';
import { useRef } from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Agenda, 
  ICalendarImport, Inject
} from '@syncfusion/ej2-react-schedule';
import { UploaderComponent } from '@syncfusion/ej2-react-inputs';
import { scheduleData } from './datasource';

const App = () => {
  const scheduleObj = useRef<ScheduleComponent>(null);
  const allowedExtensions: string = '.ics';
  const eventSettings = { dataSource: scheduleData };

  const onSelect = (args): void => {
    scheduleObj.current.importICalendar(args.event.target.files[0]);
  }
  
  return (
    <div>
      <UploaderComponent 
        id='fileUpload' 
        type='file' 
        allowedExtensions={allowedExtensions} 
        cssClass='calendar-import'
        buttons={{ browse: 'Choose file' }} 
        multiple={false} 
        showFileList={false}
        selected={onSelect}
      />
      <ScheduleComponent 
        ref={scheduleObj} 
        width='100%' 
        height='520px' 
        selectedDate={new Date(2018, 1, 15)} 
        eventSettings={eventSettings}
      >
        <Inject services={[Day, Week, WorkWeek, Month, Agenda, ICalendarImport]} />
      </ScheduleComponent>
    </div>
  );
};
```

### Print Functionality

The Scheduler allows printing the scheduler element using the `print` client-side method.

**Prerequisites:**
- Import and inject the `Print` module from `@syncfusion/ej2-react-schedule`

#### Using Print Method Without Options

```typescript
import * as ReactDOM from 'react-dom';
import * as React from 'react';
import { useRef } from 'react';
import { 
  ScheduleComponent, Day, Week, WorkWeek, Month, 
  Print, Inject, ActionEventArgs, ToolbarActionArgs 
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const scheduleObj = useRef<ScheduleComponent>(null);
  const eventSettings = { dataSource: scheduleData };

  const onActionBegin = (args: ActionEventArgs & ToolbarActionArgs): void => {
    if (args.requestType === 'toolbarItemRendering') {
      let printItem = {
        align: 'Right', 
        showTextOn: 'Both', 
        prefixIcon: 'e-icon-schedule-print',
        text: 'Print', 
        cssClass: 'e-schedule-print', 
        click: onPrintIconClick
      };
      args.items.push(printItem);
    }
  }

  const onPrintIconClick = (): void => {
    scheduleObj.current.print();
  }

  return (
    <ScheduleComponent 
      ref={scheduleObj} 
      width='100%' 
      height='520px' 
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings} 
      actionBegin={onActionBegin}
    >
      <Inject services={[Day, Week, WorkWeek, Month, Print]} />
    </ScheduleComponent>
  );
};
```

#### Using Print Method with Options

Customize the print output by passing print options:

```typescript
const onPrintIconClick = (): void => {
  let printModel: ScheduleModel = {
    agendaDaysCount: 14,
    cssClass: 'e-print-schedule',
    currentView: scheduleObj.current.currentView,
    dateFormat: 'dd-MMM-yyyy',
    enableRtl: false,
    endHour: '18:00',
    firstDayOfWeek: 1,
    height: 'auto',
    readonly: true,
    showHeaderBar: false,
    showTimeIndicator: false,
    startHour: '06:00',
    width: 'auto',
    workDays: [1, 2, 3, 4, 5]
  };
  scheduleObj.current.print(printModel);
}
```

#### Customizing Print Layout

Use the `beforePrint` event to customize the print layout:

```typescript
const onBeforePrint = (args: BeforePrintEventArgs) => {
  // Add custom header
  const headerElement = document.createElement('div');
  headerElement.innerHTML = `
    <h1>Schedule Report</h1>
    <p>Date: ${new Date().toLocaleString()}</p>
  `;
  headerElement.style.backgroundColor = '#4CAF50';
  headerElement.style.color = 'white';
  headerElement.style.padding = '10px';
  args.printElement.insertBefore(headerElement, args.printElement.firstChild);

  // Add custom footer
  const footerElement = document.createElement('div');
  footerElement.textContent = 'Confidential Document';
  args.printElement.appendChild(footerElement);
}
```

---

## Clipboard Operations

The Clipboard functionality in the Syncfusion Scheduler control enhances scheduling efficiency by enabling users to cut, copy, and paste appointments with ease.

**Activation:**
- Set the `allowClipboard` property to `true`
- The `allowKeyboardInteraction` property must also be `true`

### Cut, Copy, and Paste Using Keyboard

The Scheduler supports keyboard shortcuts for clipboard operations:

| Operation | Shortcut  | Description                                                      |
|-----------|-----------|------------------------------------------------------------------|
| Copy      | Ctrl+C    | Duplicate appointments to streamline the scheduling process      |
| Cut       | Ctrl+X    | Move appointments to a new time slot without duplicates          |
| Paste     | Ctrl+V    | Place copied or cut appointments into the desired time slot      |

**Note:** For Mac users, use **Cmd** instead of **Ctrl** for copy, cut, and paste operations.

**Implementation:**

```typescript
import { useRef } from 'react';
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import {
  ScheduleComponent, ViewsDirective, ViewDirective,
  Day, Week, WorkWeek, Month, Agenda, Inject
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const scheduleObj = useRef<ScheduleComponent>(null);
  const eventSettings = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      height='550px' 
      ref={scheduleObj} 
      selectedDate={new Date(2024, 1, 15)} 
      eventSettings={eventSettings}
      allowClipboard={true} 
      showQuickInfo={false}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='WorkWeek' />
        <ViewDirective option='Month' />
        <ViewDirective option='Agenda' />
      </ViewsDirective>
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};
```

### Cut, Copy, and Paste Using Context Menu

You can programmatically manage appointments using the public methods `cut`, `copy`, and `paste`:

| Method | Parameters                              | Description                                                      |
|--------|-----------------------------------------|------------------------------------------------------------------|
| `copy` | None                                    | Duplicate the selected appointment for reuse                     |
| `cut`  | None                                    | Remove the selected appointment from its current slot for moving |
| `paste`| targetElement (Scheduler's work-cell)   | Insert the copied or cut appointment into the specified time slot|

**Implementation with Context Menu:**

```typescript
import { useRef } from 'react';
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import { closest, isNullOrUndefined, remove } from '@syncfusion/ej2-base';
import {
  ScheduleComponent, ViewsDirective, ViewDirective,
  Day, Week, WorkWeek, Month, Inject
} from '@syncfusion/ej2-react-schedule';
import { 
  BeforeOpenCloseMenuEventArgs, MenuEventArgs, MenuItemModel, 
  ContextMenuComponent 
} from '@syncfusion/ej2-react-navigations';
import { scheduleData } from './datasource';

const App = () => {
  const scheduleObj = useRef<ScheduleComponent>(null);
  const menuObj = useRef<ContextMenuComponent>(null);
  const eventSettings = { dataSource: scheduleData };
  let selectedTarget: Element;
  let targetElement: HTMLElement;
  
  const menuItems: MenuItemModel[] = [
    { text: 'Cut Event', iconCss: 'e-icons e-cut', id: 'Cut' },
    { text: 'Copy Event', iconCss: 'e-icons e-copy', id: 'Copy' },
    { text: 'Paste', iconCss: 'e-icons e-paste', id: 'Paste' }
  ];

  const onContextMenuBeforeOpen = (args: BeforeOpenCloseMenuEventArgs): void => {
    targetElement = args.event.target as HTMLElement;
    selectedTarget = closest(
      targetElement, 
      '.e-appointment,.e-work-cells,.e-all-day-cells,.e-header-cells'
    );
    
    if (isNullOrUndefined(selectedTarget)) {
      args.cancel = true;
      return;
    }
    
    if (selectedTarget.classList.contains('e-appointment')) {
      menuObj.current.showItems(['Cut', 'Copy'], true);
      menuObj.current.hideItems(['Paste'], true);
    } else {
      menuObj.current.showItems(['Paste'], true);
      menuObj.current.hideItems(['Cut', 'Copy'], true);
    }
  }

  const onMenuItemSelect = (args: MenuEventArgs): void => {
    switch (args.item.id) {
      case 'Cut':
        scheduleObj.current.cut([selectedTarget] as HTMLElement[]);
        break;
      case 'Copy':
        scheduleObj.current.copy([selectedTarget] as HTMLElement[]);
        break;
      case 'Paste':
        scheduleObj.current.paste(targetElement);
        break;
    }
  }

  return (
    <div>
      <ScheduleComponent 
        height='550px' 
        ref={scheduleObj} 
        selectedDate={new Date(2024, 1, 15)} 
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
      <ContextMenuComponent 
        target='.e-schedule' 
        items={menuItems}
        beforeOpen={onContextMenuBeforeOpen} 
        select={onMenuItemSelect}
        cssClass='schedule-context-menu' 
        ref={menuObj} 
      />
    </div>
  );
};
```

### Modifying Content Before Pasting

Use the `beforePaste` event to modify appointment content before pasting:

```typescript
interface ScheduleData {
  Id: string;
  Subject: string;
  StartTime: string;
  EndTime: string;
  Location: string;
  Description: string;
}

const onBeforePasting = (args: BeforePasteEventArgs) => {
  if (typeof args.data === 'string') {
    const dataArray: string[] = (args.data as string).split('\t');
    const result: ScheduleData = {
      Id: dataArray[0],
      Subject: dataArray[1],
      StartTime: new Date(dataArray[4]).toISOString(),
      EndTime: new Date(new Date(dataArray[4]).getTime() + 60 * 60 * 1000).toISOString(),
      Location: dataArray[2],
      Description: dataArray[3]
    };
    args.data = [result];
  }
}
```

**Example: Copying from Grid to Scheduler**

```typescript
<ScheduleComponent 
  height='550px' 
  ref={scheduleObj} 
  selectedDate={new Date(2024, 1, 15)} 
  eventSettings={eventSettings}
  allowClipboard={true} 
  showQuickInfo={false} 
  beforePaste={onBeforePasting}
>
  <Inject services={[Day, Week, WorkWeek, Month]} />
</ScheduleComponent>
<GridComponent 
  dataSource={gridData} 
  width="40%" 
  height="400px" 
  allowSelection={true}
  ref={gridObj}
>
  <ColumnsDirective>
    <ColumnDirective field="OrderID" headerText="Order ID" width={90} />
    <ColumnDirective field="CustomerID" headerText="Customer ID" width={100} />
    <ColumnDirective field="ShipCity" headerText="Ship City" width={100} />
  </ColumnsDirective>
</GridComponent>
```

---
