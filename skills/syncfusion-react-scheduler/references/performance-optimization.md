# Performance Optimization

## Table of Contents

- [Performance Optimization](#performance-optimization)
  - [Table of Contents](#table-of-contents)
  - [Virtual Scrolling](#virtual-scrolling)
    - [Enabling Virtual Scrolling](#enabling-virtual-scrolling)
    - [Lazy Loading for Appointments](#lazy-loading-for-appointments)
  - [Performance Tips](#performance-tips)

## Virtual Scrolling

Virtual scrolling support in the Scheduler component enhances performance when working with a substantial number of resources and events. This feature allows large sets of resources and events to load dynamically as users scroll.

### Enabling Virtual Scrolling

Enable virtual scrolling by setting the `allowVirtualScrolling` property to `true` within the specific timeline view settings:

```typescript
import * as ReactDOM from 'react-dom';
import * as React from 'react';
import {
  ScheduleComponent, ViewsDirective, ViewDirective, ResourcesDirective,
  ResourceDirective, TimelineMonth, TimelineYear, Resize, DragAndDrop, 
  Inject, EventSettingsModel, GroupModel
} from '@syncfusion/ej2-react-schedule';

const App = () => {
  const generateStaticEvents = (
    start: Date, 
    resCount: number, 
    overlapCount: number
  ): Object[] => {
    let data: Object[] = [];
    let id: number = 1;
    
    for (let i: number = 0; i < resCount; i++) {
      let randomCollection: number[] = [];
      let random: number = 0;
      
      for (let j: number = 0; j < overlapCount; j++) {
        random = Math.floor(Math.random() * 30);
        random = (random === 0) ? 1 : random;
        
        if (randomCollection.indexOf(random) !== -1 || 
            randomCollection.indexOf(random + 2) !== -1 ||
            randomCollection.indexOf(random - 2) !== -1) {
          random += (Math.max.apply(null, randomCollection) + 10);
        }
        
        for (let k: number = 1; k <= 2; k++) {
          randomCollection.push(random + k);
        }
        
        let startDate: Date = new Date(start.getFullYear(), start.getMonth(), random);
        startDate = new Date(startDate.getTime() + (((random % 10) * 10) * (1000 * 60)));
        let endDate: Date = new Date(startDate.getTime() + ((1440 + 30) * (1000 * 60)));
        
        data.push({
          Id: id,
          Subject: 'Event #' + id,
          StartTime: startDate,
          EndTime: endDate,
          IsAllDay: (id % 10) ? false : true,
          ResourceId: i + 1
        });
        id++;
      }
    }
    return data;
  }
  
  const generateResourceData = (
    startId: number, 
    endId: number, 
    text: string
  ): Object[] => {
    let data: { [key: string]: Object }[] = [];
    let colors: string[] = [
      '#ff8787', '#9775fa', '#748ffc', '#3bc9db', '#69db7c',
      '#fdd835', '#748ffc', '#9775fa', '#df5286', '#7fa900',
      '#fec200', '#5978ee', '#00bdae', '#ea80fc'
    ];
    
    for (let a: number = startId; a <= endId; a++) {
      let n: number = Math.floor(Math.random() * colors.length);
      data.push({
        Id: a,
        Text: text + ' ' + a,
        Color: colors[n]
      });
    }
    return data;
  }
  
  const eventSettings: EventSettingsModel = { 
    dataSource: generateStaticEvents(new Date(2018, 4, 1), 300, 12) 
  };
  const group: GroupModel = { resources: ['Resources'] };

  return (
    <ScheduleComponent 
      cssClass='virtual-scrolling' 
      width='100%'
      height='550px' 
      selectedDate={new Date(2018, 4, 1)}
      eventSettings={eventSettings}
      group={group}
    >
      <ResourcesDirective>
        <ResourceDirective 
          field='ResourceId' 
          title='Resource' 
          name='Resources' 
          allowMultiple={true}
          dataSource={generateResourceData(1, 300, 'Resource')}
          textField='Text' 
          idField='Id' 
          colorField='Color'
        />
      </ResourcesDirective>
      <ViewsDirective>
        <ViewDirective 
          option='TimelineMonth' 
          allowVirtualScrolling={true} 
          isSelected={true} 
        />
        <ViewDirective 
          option='TimelineYear' 
          orientation='Vertical' 
          allowVirtualScrolling={true} 
        />
      </ViewsDirective>
      <Inject services={[TimelineMonth, TimelineYear, Resize, DragAndDrop]} />
    </ScheduleComponent>
  );
}
```

**Note:** Virtual loading of resources and events is not supported in `MonthAgenda`, `Year`, and `TimelineYear` (Horizontal Orientation) views.

### Lazy Loading for Appointments

The lazy loading feature provides an efficient approach for loading appointment data into the Scheduler on-demand. This allows large volumes of appointments to be loaded without performance issues.

**How It Works:**
- Scheduler sends queries to the server to retrieve appointments only for resources currently displayed
- Queries include resource IDs and current date range as a comma-separated string
- Server parses resource IDs to filter and serve only necessary appointments
- Additional appointment data is fetched on-demand as new resources enter the viewport

**Enable lazy loading by setting the `enableLazyLoading` property to `true`:**

```typescript
import * as ReactDOM from 'react-dom';
import * as React from 'react';
import {
  ScheduleComponent, ViewsDirective, ViewDirective, ResourcesDirective,
  ResourceDirective, TimelineMonth, Inject, EventSettingsModel, GroupModel
} from '@syncfusion/ej2-react-schedule';
import { DataManager, WebApiAdaptor } from '@syncfusion/ej2-data';

const App = () => {
  const dataManager: DataManager = new DataManager({
    url: 'url',
    adaptor: new WebApiAdaptor,
    crossDomain: true
  });
  
  const eventSettings: EventSettingsModel = { dataSource: dataManager };
  const group: GroupModel = { resources: ['Resources'] };
  
  const generateResourceData = (
    startId: number, 
    endId: number, 
    text: string
  ): Object[] => {
    let data: { [key: string]: Object }[] = [];
    let colors: string[] = [
      '#ff8787', '#9775fa', '#748ffc', '#3bc9db', '#69db7c',
      '#fdd835', '#748ffc', '#9775fa', '#df5286', '#7fa900',
      '#fec200', '#5978ee', '#00bdae', '#ea80fc'
    ];
    
    for (let a: number = startId; a <= endId; a++) {
      let n: number = Math.floor(Math.random() * colors.length);
      data.push({
        Id: a,
        Text: text + ' ' + a,
        Color: colors[n]
      });
    }
    return data;
  }
  
  return (
    <ScheduleComponent 
      width='100%'
      height='550px' 
      selectedDate={new Date(2023, 3, 1)}
      eventSettings={eventSettings}
      group={group} 
      readonly={true}
    >
      <ResourcesDirective>
        <ResourceDirective 
          field='ResourceId' 
          title='Resource' 
          name='Resources'
          dataSource={generateResourceData(1, 1000, 'Resource')}
          textField='Text' 
          idField='Id' 
          colorField='Color'
        />
      </ResourcesDirective>
      <ViewsDirective>
        <ViewDirective 
          option='TimelineMonth' 
          enableLazyLoading={true} 
          isSelected={true} 
        />
      </ViewsDirective>
      <Inject services={[TimelineMonth]} />
    </ScheduleComponent>
  );
}
```

**Server-Side Implementation (C#):**

```csharp
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System;
using Microsoft.EntityFrameworkCore;
using System.Linq;
using Microsoft.AspNetCore.OData.Query;

namespace LazyLoadingServices.Controllers
{
    public class VirtualEventDataController : Controller
    {
        private readonly EventsContext dbContext;

        [HttpGet]
        [EnableQuery]
        [Route("api/VirtualEventData")]
        public IActionResult GetData([FromQuery] Params param)
        {
            IQueryable<EventData> query = dbContext.Events;
            
            // Filter the appointment data based on the ResourceId query params
            if (!string.IsNullOrEmpty(param.ResourceId))
            {
                string[] resourceId = param.ResourceId.Split(',');
                query = query.Where(data => resourceId.Contains(data.ResourceId.ToString()));
            }
            
            return Ok(query.ToList());
        }
    }
    
    public class Params
    {
        public DateTime? StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string ResourceId { get; set; }
    }
}
```

**Important Notes:**
- This property is effective when large numbers of resources and appointments are bound to the Scheduler
- This property is applicable only when resource grouping is enabled in Scheduler

---

## Performance Tips

When working with advanced features in the Syncfusion React Scheduler, consider these performance optimization tips:

### 1. Virtual Scrolling
- **Use virtual scrolling** for large datasets (300+ resources or 1000+ events)
- Enable `allowVirtualScrolling` in timeline views for better performance
- Combine with `enableLazyLoading` for optimal server-side data retrieval

### 2. Data Management
- **Limit initial data load:** Use lazy loading to fetch data on-demand
- **Optimize queries:** Filter data server-side before sending to the client
- **Use DataManager:** Leverage efficient data binding with remote services
- **Implement caching:** Cache frequently accessed data to reduce server calls

### 3. Event Rendering
- **Reduce event complexity:** Minimize custom templates and complex styling
- **Use event templates wisely:** Keep templates lightweight and avoid heavy computations
- **Limit visible events:** Use date range filters to show only necessary events

### 4. Resource Handling
- **Group resources efficiently:** Avoid unnecessary nested grouping
- **Limit resource count:** Display only essential resources initially
- **Use color coding:** Simplify visual representation instead of complex styles

### 5. Export Operations
- **Export selectively:** Use field filters to export only necessary data
- **Batch exports:** For large datasets, consider server-side export generation
- **Optimize file size:** Exclude unnecessary fields and limit date ranges

### 6. Clipboard Operations
- **Disable when not needed:** Set `allowClipboard` to `false` if not using clipboard features
- **Optimize event handlers:** Keep `beforePaste` event handlers lightweight
- **Batch operations:** Process multiple clipboard operations together

### 7. State Persistence
- **Clear old data:** Periodically clear localStorage to prevent bloat
- **Selective persistence:** Only persist essential state information
- **Monitor storage usage:** Check localStorage size limits in different browsers

### 8. General Optimization
- **Disable unused features:** Only inject required modules and services
- **Optimize view switching:** Minimize data reloading when switching views
- **Use readonly mode:** Enable `readonly` for view-only scenarios
- **Debounce scroll events:** Implement debouncing for scroll-triggered operations
- **Minimize DOM manipulation:** Batch DOM updates when possible

### 9. Network Optimization
- **Use compression:** Enable gzip compression for data transfers
- **Implement pagination:** Load data in chunks rather than all at once
- **Use CDN:** Serve static resources from CDN for faster loading
- **Minimize API calls:** Combine multiple requests where possible

### 10. Browser Considerations
- **Test across browsers:** Ensure performance is acceptable on target browsers
- **Monitor memory usage:** Watch for memory leaks with browser dev tools
- **Profile performance:** Use browser profiling tools to identify bottlenecks
- **Handle edge cases:** Test with maximum expected data volumes

By following these performance tips, you can ensure that your Syncfusion React Scheduler application remains responsive and efficient, even when working with large datasets and complex scheduling scenarios.

---
