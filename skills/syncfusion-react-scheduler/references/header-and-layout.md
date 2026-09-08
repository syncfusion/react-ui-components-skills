# Header and Layout

## Table of Contents
- [Header and Layout](#header-and-layout)
  - [Overview](#overview)
  - [Header Bar](#header-bar)
    - [Show or Hide Header Bar](#show-or-hide-header-bar)
    - [Customizing Header Bar Using Template](#customizing-header-bar-using-template)
    - [Customizing Header Bar Using Event](#customizing-header-bar-using-event)
    - [Display View Options in Header Bar Popup](#display-view-options-in-header-bar-popup)
    - [Date Header Customization](#date-header-customization)
    - [Customizing Date Range Text](#customizing-date-range-text)
    - [Customizing Header Indent Cells](#customizing-header-indent-cells)
  - [Header Rows](#header-rows)
    - [Available Header Row Types](#available-header-row-types)
    - [Display Year and Month Rows](#display-year-and-month-rows)
    - [Display Week Numbers](#display-week-numbers)
    - [Display Full Year in Timeline](#display-full-year-in-timeline)
    - [Customizing Header Rows with Templates](#customizing-header-rows-with-templates)

## Overview

The Syncfusion React Scheduler provides comprehensive customization options for header and layout features. This includes customizing the header bar, configuring additional header rows for Timeline views, setting dimensions, and enabling row auto-height adjustments. These features allow you to create a flexible and responsive scheduler that adapts to various display requirements and data volumes.

Key layout features include:
- **Header Bar Customization**: Control toolbar visibility, add custom items, and customize date navigation
- **Header Rows**: Add additional temporal rows (Year, Month, Week, Date, Hour) in Timeline views
- **Dimensions Control**: Set scheduler size using auto, pixel, or percentage values
- **Row Auto Height**: Automatically adjust row heights based on appointment count
- **Responsive Design**: Built-in adaptive UI for mobile and desktop experiences

## Header Bar

The header bar is the top section of the Scheduler that contains date navigation, view switchers, and other toolbar items. It can be fully customized to match your application requirements.

### Show or Hide Header Bar

By default, the header bar displays date and view navigation options. You can hide it using the `showHeaderBar` property set to `false` (default is `true`).

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Inject, 
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
      showHeaderBar={false} 
      eventSettings={eventSettings}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='WorkWeek' />
      </ViewsDirective>
      <Inject services={[Day, Week, WorkWeek]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Customizing Header Bar Using Template

Add custom items to the Scheduler header bar using the `toolbarItems` property with `ToolbarItemsDirective`. Each item requires a `name` field for default items like `Previous`, `Next`, `Today`, `DateRangeText`, `NewEvent`, and `Views`. Custom items should use `Custom` as the name.

```tsx
import { useRef } from 'react';
import * as React from 'react';
import { 
  ScheduleComponent, EventSettingsModel, ViewsDirective, ViewDirective,
  ResourcesDirective, ResourceDirective, Month, Inject, Resize, 
  DragAndDrop, ToolbarItemsDirective, ToolbarItemDirective 
} from '@syncfusion/ej2-react-schedule';
import { DropDownListComponent, ChangeEventArgs } from '@syncfusion/ej2-react-dropdowns';
import { Predicate, Query } from '@syncfusion/ej2-data';
import { scheduleData } from './datasource';

const App = () => {
  const schedule = useRef<ScheduleComponent>(null);
  const eventSettings: EventSettingsModel = { 
    dataSource: scheduleData, 
    query: new Query().where('OwnerId', 'equal', 1) 
  };
  
  const ownerData: { [key: string]: Object }[] = [
    { OwnerText: 'Margaret', OwnerId: 1, Color: '#ea7a57' },
    { OwnerText: 'Robert', OwnerId: 2, Color: '#df5286' },
    { OwnerText: 'Laura', OwnerId: 3, Color: '#865fcf' }
  ];
  
  const fields: object = { text: 'OwnerText', value: 'OwnerId' };

  const template = () => {
    return (
      <DropDownListComponent 
        id='ddlelement' 
        dataSource={ownerData} 
        fields={fields} 
        value={1} 
        change={OnChange} 
      />
    );
  };

  const OnChange = (args: ChangeEventArgs) => {
    let predicate: Predicate;
    predicate = new Predicate('OwnerId', 'equal', parseInt(args.value as string, 10));
    if (schedule.current) {
      schedule.current.eventSettings.query = new Query().where(predicate);
    }
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='650px' 
      ref={schedule} 
      selectedDate={new Date(2024, 11, 15)} 
      eventSettings={eventSettings}
    >
      <ResourcesDirective>
        <ResourceDirective 
          field='OwnerId' 
          title='Owner' 
          name='Owners' 
          dataSource={ownerData} 
          textField='OwnerText' 
          idField='OwnerId' 
          colorField='Color'
        />
      </ResourcesDirective>
      <ViewsDirective>
        <ViewDirective option='Month' />
      </ViewsDirective>
      <Inject services={[Month, Resize, DragAndDrop]} />
      <ToolbarItemsDirective>
        <ToolbarItemDirective name='Previous' align='Left' />
        <ToolbarItemDirective name='Next' align='Left' />
        <ToolbarItemDirective name='DateRangeText' align='Left' />
        <ToolbarItemDirective name='Today' align='Right' />
        <ToolbarItemDirective align='Center' template={template} />
      </ToolbarItemsDirective>
    </ScheduleComponent>
  );
};

export default App;
```

### Customizing Header Bar Using Event

Use the `actionBegin` event to add custom items dynamically to the header bar. This example adds a user profile image that displays a popup on click.

```tsx
import { useRef, useEffect } from 'react';
import * as React from 'react';
import {
  ScheduleComponent, ViewsDirective, ViewDirective, Month, Inject,
  ActionEventArgs, ToolbarActionArgs, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { createElement, compile } from '@syncfusion/ej2-base';
import { ItemModel } from '@syncfusion/ej2-react-navigations';
import { Popup } from '@syncfusion/ej2-popups';
import { scheduleData } from './datasource';

const App = () => {
  const schedule = useRef<ScheduleComponent>(null);
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };
  let profilePopup: Popup;
  
  const onActionBegin = (args: ActionEventArgs & ToolbarActionArgs): void => {
    if (args.requestType === 'toolbarItemRendering') {
      const userIconItem: ItemModel = {
        align: 'Right', 
        prefixIcon: 'user-icon', 
        text: 'Nancy', 
        cssClass: 'e-schedule-user-icon'
      };
      args.items.push(userIconItem);
    }
  };

  const onActionComplete = (args: ActionEventArgs): void => {
    const scheduleElement: HTMLElement = document.getElementById('schedule');
    if (args.requestType === 'toolBarItemRendered' && scheduleElement) {
      const userIconEle: HTMLElement = scheduleElement.querySelector('.e-schedule-user-icon');
      if (userIconEle) {
        userIconEle.onclick = () => {
          if (profilePopup) {
            profilePopup.relateTo = userIconEle;
            profilePopup.dataBind();
            profilePopup.element.classList.contains('e-popup-close') 
              ? profilePopup.show() 
              : profilePopup.hide();
          }
        };
      }
    }
  };

  return (
    <ScheduleComponent 
      cssClass='schedule-header-bar' 
      width='100%' 
      height='550px' 
      ref={schedule}
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
      actionBegin={onActionBegin} 
      actionComplete={onActionComplete}
    >
      <ViewsDirective>
        <ViewDirective option='Month' />
      </ViewsDirective>
      <Inject services={[Month]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Display View Options in Header Bar Popup

For adaptive UI on mobile devices, move view options to a header bar popup by setting `enableAdaptiveUI` to `true`.

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Agenda, Inject, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      width='100%' 
      height='500px' 
      selectedDate={new Date(2018, 1, 15)} 
      enableAdaptiveUI={true} 
      eventSettings={eventSettings}
    >
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Date Header Customization

Customize date header cells in Day, Week, WorkWeek, and Timeline views using the `dateHeaderTemplate` option.

```tsx
import * as React from 'react';
import { 
  ScheduleComponent, ViewsDirective, ViewDirective, 
  Day, Week, WorkWeek, Inject, TimelineViews 
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';
import { Internationalization } from '@syncfusion/ej2-base';

const App = () => {
  const eventSettings = { dataSource: scheduleData };
  const instance: Internationalization = new Internationalization();
  
  const getDateHeaderText = (value: Date): string => {
    return instance.formatDate(value, { skeleton: 'Ed' });
  };
  
  const getWeather = (value: Date) => {
    const weatherMap = {
      0: '25°C', 1: '18°C', 2: '10°C', 3: '16°C',
      4: '8°C', 5: '27°C', 6: '17°C'
    };
    return `<div class="weather-text">${weatherMap[value.getDay()]}</div>`;
  };
  
  const dateHeaderTemplate = (props): JSX.Element => {
    return (
      <div>
        <div>{getDateHeaderText(props.date)}</div>
        <div 
          className="date-text" 
          dangerouslySetInnerHTML={{ __html: getWeather(props.date) }}
        />
      </div>
    );
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      cssClass='schedule-date-header-template'
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
      dateHeaderTemplate={dateHeaderTemplate}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='WorkWeek' />
        <ViewDirective option='TimelineWeek' />
      </ViewsDirective>
      <Inject services={[Day, Week, WorkWeek, TimelineViews]} />
    </ScheduleComponent>
  );
};

export default App;
```

For Month view, use the `renderCell` event to customize date headers:

```tsx
import * as React from 'react';
import { 
  ScheduleComponent, ViewsDirective, ViewDirective, 
  Month, RenderCellEventArgs, Inject 
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings = { dataSource: scheduleData };
  
  const getWeather = (value: Date) => {
    const weatherMap = {
      0: '25°C', 1: '18°C', 2: '10°C', 3: '16°C',
      4: '8°C', 5: '27°C', 6: '17°C'
    };
    return `<div class="weather-text">${weatherMap[value.getDay()]}</div>`;
  };
  
  const onRenderCell = (args: RenderCellEventArgs): void => {
    if (args.elementType === 'monthCells') {
      let ele: Element = document.createElement('div');
      ele.innerHTML = getWeather(args.date);
      args.element.appendChild(ele.firstChild);
    }
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      cssClass='schedule-date-header-template'
      renderCell={onRenderCell} 
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
    >
      <ViewsDirective>
        <ViewDirective option='Month' />
      </ViewsDirective>
      <Inject services={[Month]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Customizing Date Range Text

Customize the date range text displayed in the header bar using the `dateRangeTemplate` option.

```tsx
import * as React from 'react';
import { 
  ScheduleComponent, ViewsDirective, ViewDirective, 
  Day, Week, WorkWeek, Inject, TimelineViews 
} from '@syncfusion/ej2-react-schedule';
import { Internationalization } from '@syncfusion/ej2-base';

const App = () => {
  const instance: Internationalization = new Internationalization();
  
  const getDateRange = (startDate: Date, endDate: Date): string => {
    return instance.formatDate(startDate, { skeleton: 'yMd' }) + 
           ' - ' + 
           instance.formatDate(endDate, { skeleton: 'yMd' });
  };
  
  const dateRangeTemplate = (props): JSX.Element => {
    return <div>{getDateRange(props.startDate, props.endDate)}</div>;
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px'
      dateRangeTemplate={dateRangeTemplate}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='WorkWeek' />
        <ViewDirective option='TimelineWeek' />
      </ViewsDirective>
      <Inject services={[Day, Week, WorkWeek, TimelineViews]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Customizing Header Indent Cells

Customize header indent cells in vertical and Timeline views using the `headerIndentTemplate` option.

```tsx
import * as React from 'react';
import {
  Week, TimelineViews, TimelineMonth, Day, ScheduleComponent, GroupModel,
  ViewsDirective, ViewDirective, ResourcesDirective, EventSettingsModel,
  ResourceDirective, Inject
} from '@syncfusion/ej2-react-schedule';
import { resourceData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: resourceData };
  const group: GroupModel = { resources: ['Owners'] };
  const ownerData: object[] = [
    { OwnerText: 'Nancy', Id: 1, OwnerColor: '#ffaa00' },
    { OwnerText: 'Steven', Id: 2, OwnerColor: '#f8a398' },
    { OwnerText: 'Michael', Id: 3, OwnerColor: '#7499e1' }
  ];

  const headerIndentTemplate = () => {
    return (
      <div className='e-resource-text'>
        <div className="text">Resources</div>
      </div>
    );
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      currentView='Week' 
      headerIndentTemplate={headerIndentTemplate} 
      selectedDate={new Date(2018, 3, 1)} 
      eventSettings={eventSettings} 
      group={group}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='TimelineWeek' />
        <ViewDirective option='TimelineMonth' />
      </ViewsDirective>
      <ResourcesDirective>
        <ResourceDirective 
          field='OwnerId' 
          title='Owner' 
          name='Owners' 
          allowMultiple={true} 
          dataSource={ownerData} 
          textField='OwnerText' 
          idField='Id' 
          colorField='OwnerColor'
        />
      </ResourcesDirective>
      <Inject services={[Day, Week, TimelineViews, TimelineMonth]} />
    </ScheduleComponent>
  );
};

export default App;
```

## Header Rows

Timeline views support additional header rows beyond the default date and time headers. You can add Year, Month, Week, Date, and Hour rows using `HeaderRowDirective`.

### Available Header Row Types

The following header row options are available:
- **Year**: Displays year information
- **Month**: Displays month information
- **Week**: Displays week numbers
- **Date**: Displays date information
- **Hour**: Displays hour information (not applicable for Timeline Month view)

**Note**: Import `HeaderRowsDirective` and `HeaderRowDirective` to use this feature.

Example showing all available header rows:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, HeaderRowDirective, HeaderRowsDirective, 
  TimelineViews, Inject, ViewsDirective, ViewDirective
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings = { dataSource: scheduleData };
  
  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      selectedDate={new Date(2018, 11, 31)}
      eventSettings={eventSettings} 
      startHour='09:00' 
      endHour='13:00'
    >
      <HeaderRowsDirective>
        <HeaderRowDirective option='Year' />
        <HeaderRowDirective option='Month' />
        <HeaderRowDirective option='Week' />
        <HeaderRowDirective option='Date' />
        <HeaderRowDirective option='Hour' />
      </HeaderRowsDirective>
      <ViewsDirective>
        <ViewDirective option='TimelineWeek' />
      </ViewsDirective>
      <Inject services={[TimelineViews]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Display Year and Month Rows

Display only year and month header rows in Timeline views:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, HeaderRowDirective, HeaderRowsDirective, 
  TimelineMonth, Inject, ViewsDirective, ViewDirective
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      selectedDate={new Date(2018, 11, 31)}
      eventSettings={eventSettings}
    >
      <HeaderRowsDirective>
        <HeaderRowDirective option='Year' />
        <HeaderRowDirective option='Month' />
      </HeaderRowsDirective>
      <ViewsDirective>
        <ViewDirective option='TimelineMonth' interval={24} />
      </ViewsDirective>
      <Inject services={[TimelineMonth]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Display Week Numbers

Display week numbers in a separate header row:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, HeaderRowDirective, HeaderRowsDirective, 
  TimelineMonth, TimelineViews, Inject, ViewsDirective, ViewDirective
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      selectedDate={new Date(2018, 11, 31)}
      eventSettings={eventSettings}
    >
      <HeaderRowsDirective>
        <HeaderRowDirective option='Week' />
        <HeaderRowDirective option='Date' />
        <HeaderRowDirective option='Hour' />
      </HeaderRowsDirective>
      <ViewsDirective>
        <ViewDirective option='TimelineMonth' interval={24} />
        <ViewDirective option='TimelineWeek' interval={3} />
        <ViewDirective option='TimelineDay' interval={4} />
      </ViewsDirective>
      <Inject services={[TimelineMonth, TimelineViews]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Display Full Year in Timeline

Display a complete year in Timeline view by setting the `interval` to 12:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, HeaderRowDirective, HeaderRowsDirective, 
  TimelineMonth, Inject, ViewsDirective, ViewDirective
} from '@syncfusion/ej2-react-schedule';
import { eventData } from './datasource';

const App = () => {
  const eventSettings = { dataSource: eventData };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      selectedDate={new Date(2018, 0, 1)}
      eventSettings={eventSettings}
    >
      <HeaderRowsDirective>
        <HeaderRowDirective option='Month' />
        <HeaderRowDirective option='Date' />
      </HeaderRowsDirective>
      <ViewsDirective>
        <ViewDirective option='TimelineMonth' interval={12} />
      </ViewsDirective>
      <Inject services={[TimelineMonth]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Customizing Header Rows with Templates

Customize header row text and display images or formatted text using the `template` option:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, getWeekNumber, HeaderRowDirective, HeaderRowsDirective, 
  TimelineMonth, Inject, ViewsDirective, ViewDirective, CellTemplateArgs
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';
import { Internationalization } from '@syncfusion/ej2-base';

const App = () => {
  const eventSettings = { dataSource: scheduleData };
  const instance: Internationalization = new Internationalization();
  
  const getYearDetails = (value: CellTemplateArgs) => {
    return 'Year: ' + instance.formatDate(value.date, { skeleton: 'y' });
  };
  
  const getMonthDetails = (value: CellTemplateArgs) => {
    return 'Month: ' + instance.formatDate(value.date, { skeleton: 'M' });
  };
  
  const getWeekDetails = (value: CellTemplateArgs) => {
    return 'Week ' + getWeekNumber(value.date);
  };
  
  const yearTemplate = (props): JSX.Element => {
    return <span className="year">{getYearDetails(props)}</span>;
  };
  
  const monthTemplate = (props): JSX.Element => {
    return <span className="month">{getMonthDetails(props)}</span>;
  };
  
  const weekTemplate = (props): JSX.Element => {
    return <span className="week">{getWeekDetails(props)}</span>;
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      selectedDate={new Date(2018, 0, 1)}
      eventSettings={eventSettings}
    >
      <HeaderRowsDirective>
        <HeaderRowDirective option='Year' template={yearTemplate} />
        <HeaderRowDirective option='Month' template={monthTemplate} />
        <HeaderRowDirective option='Week' template={weekTemplate} />
        <HeaderRowDirective option='Date' />
      </HeaderRowsDirective>
      <ViewsDirective>
        <ViewDirective option='TimelineMonth' />
      </ViewsDirective>
      <Inject services={[TimelineMonth]} />
    </ScheduleComponent>
  );
};

export default App;
```

