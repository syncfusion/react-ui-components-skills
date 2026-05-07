# Content Loading

## Table of Contents
- [Overview](#overview)
- [Loading Content from Functions](#loading-content-from-functions)
- [Loading from Data Source](#loading-from-data-source)
- [Loading via HTTP Requests](#loading-via-http-requests)
- [Template-Based Rendering](#template-based-rendering)
- [Rendering Other React Components](#rendering-other-react-components)
- [Dynamic Item Loading](#dynamic-item-loading)
- [Lazy Loading Patterns](#lazy-loading-patterns)

## Overview

The Accordion component supports multiple ways to load and render content:

1. **Static Content** - Hardcoded strings or functions
2. **Data Source** - Arrays of objects mapped to accordion items
3. **HTTP Requests** - Fetch content from APIs
4. **JSX Templates** - Render React components inside panels
5. **Dynamic Loading** - Add/remove items at runtime

Choose the method based on your content source and update patterns.

## Loading Content from Functions

Content can be provided as JavaScript functions that return JSX or strings:

### Basic Function Content

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const htmlContent = () => (
    <div>
      <p>HTML (HyperText Markup Language) is the standard markup language for creating web pages.</p>
      <p>It provides the structure and semantics for web content.</p>
    </div>
  );

  const cssContent = () => (
    <div>
      <p>CSS (Cascading Style Sheets) is used to style and layout web pages.</p>
      <p>It handles colors, fonts, spacing, and responsive design.</p>
    </div>
  );

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective header='HTML' content={htmlContent} />
        <AccordionItemDirective header='CSS' content={cssContent} />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Content with State

Functions can access component state:

```jsx
import React, { useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [userName, setUserName] = useState('John');
  const [userEmail, setUserEmail] = useState('john@example.com');

  const profileContent = () => (
    <div>
      <p><strong>Name:</strong> {userName}</p>
      <p><strong>Email:</strong> {userEmail}</p>
      <input 
        placeholder='Enter new name' 
        onChange={(e) => setUserName(e.target.value)} 
      />
    </div>
  );

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective header='User Profile' content={profileContent} />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

## DataSource Binding

The `dataSource` property binds array data directly to the accordion, automatically generating items from data objects.

### Basic DataSource Binding

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const faqData = [
    { 
      header: 'What is React?', 
      content: 'React is a JavaScript library for building user interfaces...' 
    },
    { 
      header: 'What are Props?', 
      content: 'Props are arguments passed into React components...' 
    },
    { 
      header: 'What is State?', 
      content: 'State is similar to props, but it is private and controlled...' 
    }
  ];

  return (
    <AccordionComponent dataSource={faqData}>
      <AccordionItemsDirective>
        {faqData.map((item, index) => (
          <AccordionItemDirective 
            key={index}
            header={item.header}
            content={item.content}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Advanced DataSource with Custom Fields

```jsx
const courseData = [
  {
    id: 1,
    courseTitle: 'React Basics',
    courseSummary: 'Learn the fundamentals of React...',
    instructor: 'John Doe',
    duration: '4 weeks',
    level: 'Beginner'
  },
  {
    id: 2,
    courseTitle: 'React Advanced',
    courseSummary: 'Master advanced React patterns...',
    instructor: 'Jane Smith',
    duration: '6 weeks',
    level: 'Advanced'
  }
];

export default function App() {
  return (
    <AccordionComponent dataSource={courseData}>
      <AccordionItemsDirective>
        {courseData.map((course) => (
          <AccordionItemDirective 
            key={course.id}
            header={course.courseTitle}
            content={() => (
              <div>
                <p>{course.courseSummary}</p>
                <p><strong>Instructor:</strong> {course.instructor}</p>
                <p><strong>Duration:</strong> {course.duration}</p>
                <p><strong>Level:</strong> {course.level}</p>
              </div>
            )}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Filtering DataSource

```jsx
import React, { useState, useMemo } from 'react';

export default function App() {
  const [searchTerm, setSearchTerm] = useState('');

  const allProducts = [
    { name: 'React Book', category: 'JavaScript' },
    { name: 'Vue Guide', category: 'JavaScript' },
    { name: 'CSS Mastery', category: 'Styling' }
  ];

  const filteredData = useMemo(() => {
    return allProducts.filter(item =>
      item.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [searchTerm]);

  return (
    <div>
      <input
        placeholder='Search products...'
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        style={{ marginBottom: '15px', padding: '8px', width: '200px' }}
      />

      <AccordionComponent dataSource={filteredData}>
        <AccordionItemsDirective>
          {filteredData.map((product, index) => (
            <AccordionItemDirective 
              key={index}
              header={product.name}
              content={product.category}
            />
          ))}
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### Grouping DataSource

```jsx
const data = [
  { group: 'Frontend', name: 'React', description: 'JavaScript library' },
  { group: 'Frontend', name: 'Vue', description: 'Progressive framework' },
  { group: 'Backend', name: 'Node.js', description: 'JavaScript runtime' },
  { group: 'Backend', name: 'Django', description: 'Python framework' }
];

export default function App() {
  const groupedByCategory = data.reduce((acc, item) => {
    if (!acc[item.group]) {
      acc[item.group] = [];
    }
    acc[item.group].push(item);
    return acc;
  }, {});

  return (
    <AccordionComponent expandMode='Multiple'>
      <AccordionItemsDirective>
        {Object.entries(groupedByCategory).map(([group, items]) => (
          <AccordionItemDirective 
            key={group}
            header={group}
            content={() => (
              <ul>
                {items.map((item, idx) => (
                  <li key={idx}>
                    <strong>{item.name}</strong> - {item.description}
                  </li>
                ))}
              </ul>
            )}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

## Loading from Data Source

Use arrays of objects to generate accordion items dynamically:

### Array-Based Loading

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const accordionData = [
    { header: 'HTML Basics', content: 'HTML provides the structure for web pages...' },
    { header: 'CSS Styling', content: 'CSS is used for visual styling and layouts...' },
    { header: 'JavaScript', content: 'JavaScript adds interactivity and dynamic behavior...' }
  ];

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        {accordionData.map((item, index) => (
          <AccordionItemDirective 
            key={index}
            header={item.header} 
            content={item.content} 
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### With Rich Objects

```jsx
const courseData = [
  {
    id: 1,
    title: 'React Fundamentals',
    description: 'Learn React basics...',
    duration: '4 weeks',
    level: 'Beginner'
  },
  {
    id: 2,
    title: 'React Advanced',
    description: 'Master React patterns...',
    duration: '6 weeks',
    level: 'Advanced'
  }
];

const courseContent = (item) => (
  <div>
    <p><strong>Duration:</strong> {item.duration}</p>
    <p><strong>Level:</strong> {item.level}</p>
    <p>{item.description}</p>
  </div>
);

<AccordionComponent>
  <AccordionItemsDirective>
    {courseData.map((course) => (
      <AccordionItemDirective 
        key={course.id}
        header={course.title}
        content={() => courseContent(course)}
      />
    ))}
  </AccordionItemsDirective>
</AccordionComponent>
```

## Loading via HTTP Requests

Fetch content from APIs and render in accordion panels:

### Basic API Loading

```jsx
import React, { useState, useEffect } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts?_limit=5')
      .then((res) => res.json())
      .then((data) => {
        setPosts(data);
        setLoading(false);
      })
      .catch((error) => {
        console.error('Error loading posts:', error);
        setLoading(false);
      });
  }, []);

  if (loading) return <div>Loading posts...</div>;

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        {posts.map((post) => (
          <AccordionItemDirective 
            key={post.id}
            header={post.title}
            content={() => <div>{post.body}</div>}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### With Loading State

```jsx
import React, { useState, useEffect } from 'react';

export default function App() {
  const [items, setItems] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const loadData = async () => {
      try {
        setLoading(true);
        const response = await fetch('/api/accordion-items');
        if (!response.ok) throw new Error('Failed to load');
        const data = await response.json();
        setItems(data);
        setError(null);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    loadData();
  }, []);

  if (loading) return <p>Loading items...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        {items.map((item) => (
          <AccordionItemDirective 
            key={item.id}
            header={item.name}
            content={() => <div>{item.details}</div>}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

## Loading via HTTP POST Requests

Send data to a server and load responses in accordion panels:

### Basic POST Request

```jsx
import React, { useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [items, setItems] = useState([]);
  const [loading, setLoading] = useState(false);

  const loadContentViaPost = async () => {
    setLoading(true);
    try {
      const response = await fetch('/api/accordion/content', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          requestType: 'accordion-items',
          format: 'json'
        })
      });

      const data = await response.json();
      setItems(data.items);
    } catch (error) {
      console.error('Error loading content:', error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div>
      <button onClick={loadContentViaPost} disabled={loading}>
        {loading ? 'Loading...' : 'Load Content'}
      </button>

      {items.length > 0 && (
        <AccordionComponent style={{ marginTop: '15px' }}>
          <AccordionItemsDirective>
            {items.map((item) => (
              <AccordionItemDirective 
                key={item.id}
                header={item.header}
                content={item.content}
              />
            ))}
          </AccordionItemsDirective>
        </AccordionComponent>
      )}
    </div>
  );
}
```

### POST with Form Data

```jsx
import React, { useState, useRef } from 'react';

export default function App() {
  const [items, setItems] = useState([]);
  const formRef = useRef(null);

  const submitForm = async (e) => {
    e.preventDefault();

    const formData = new FormData(formRef.current);

    try {
      const response = await fetch('/api/accordion/load', {
        method: 'POST',
        body: formData
      });

      const data = await response.json();
      setItems(data.items);
    } catch (error) {
      console.error('Error:', error);
    }
  };

  return (
    <div>
      <form ref={formRef} onSubmit={submitForm}>
        <input 
          type='text' 
          name='category' 
          placeholder='Enter category'
          required
        />
        <input 
          type='text' 
          name='search' 
          placeholder='Search term'
        />
        <button type='submit'>Search & Load</button>
      </form>

      {items.length > 0 && (
        <AccordionComponent style={{ marginTop: '15px' }}>
          <AccordionItemsDirective>
            {items.map((item) => (
              <AccordionItemDirective 
                key={item.id}
                header={item.header}
                content={item.content}
              />
            ))}
          </AccordionItemsDirective>
        </AccordionComponent>
      )}
    </div>
  );
}
```

### POST with Authentication

```jsx
import React, { useState, useEffect } from 'react';

export default function App() {
  const [items, setItems] = useState([]);
  const [token, setToken] = useState(null);

  useEffect(() => {
    const loadSecureContent = async () => {
      if (!token) return;

      try {
        const response = await fetch('/api/secured/accordion', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${token}`
          },
          body: JSON.stringify({
            userId: 123,
            dataType: 'accordion'
          })
        });

        if (response.status === 401) {
          console.log('Unauthorized - token expired');
          setToken(null);
          return;
        }

        const data = await response.json();
        setItems(data.items);
      } catch (error) {
        console.error('Error loading secure content:', error);
      }
    };

    loadSecureContent();
  }, [token]);

  const login = async () => {
    // Mock login
    const authToken = 'eyJhbGc...(JWT token)';
    setToken(authToken);
  };

  if (!token) {
    return <button onClick={login}>Login</button>;
  }

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        {items.map((item) => (
          <AccordionItemDirective 
            key={item.id}
            header={item.header}
            content={item.content}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### POST on Item Expansion

Load content via POST only when item is expanded:

```jsx
import React, { useState } from 'react';
import { ExpandedEventArgs } from '@syncfusion/ej2-navigations';

export default function App() {
  const [contentCache, setContentCache] = useState({});
  const [loading, setLoading] = useState({});

  const loadContentOnExpand = async (args: ExpandedEventArgs) => {
    if (args.isExpanded && !contentCache[args.index]) {
      setLoading(prev => ({ ...prev, [args.index]: true }));

      try {
        const response = await fetch('/api/content/load', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ itemIndex: args.index })
        });

        const data = await response.json();
        setContentCache(prev => ({
          ...prev,
          [args.index]: data.content
        }));
      } catch (error) {
        console.error('Error loading content:', error);
      } finally {
        setLoading(prev => ({ ...prev, [args.index]: false }));
      }
    }
  };

  const items = [
    { header: 'Item 1', index: 0 },
    { header: 'Item 2', index: 1 },
    { header: 'Item 3', index: 2 }
  ];

  return (
    <AccordionComponent expanded={loadContentOnExpand}>
      <AccordionItemsDirective>
        {items.map((item) => (
          <AccordionItemDirective 
            key={item.index}
            header={item.header}
            content={() => (
              <>
                {loading[item.index] && <p>Loading...</p>}
                {contentCache[item.index] && <p>{contentCache[item.index]}</p>}
              </>
            )}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Handling POST Errors

```jsx
import React, { useState } from 'react';

export default function App() {
  const [items, setItems] = useState([]);
  const [error, setError] = useState(null);
  const [retryCount, setRetryCount] = useState(0);
  const MAX_RETRIES = 3;

  const loadWithRetry = async () => {
    setError(null);

    try {
      const response = await fetch('/api/accordion', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ requestId: Date.now() })
      });

      if (!response.ok) {
        if (response.status === 429) {
          throw new Error('Too many requests - Please try again later');
        } else if (response.status === 500) {
          throw new Error('Server error - Please try again');
        } else {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
      }

      const data = await response.json();
      setItems(data.items);
      setRetryCount(0);
    } catch (error) {
      setError(error.message);

      // Auto-retry on network errors
      if (retryCount < MAX_RETRIES) {
        setTimeout(() => {
          setRetryCount(prev => prev + 1);
          loadWithRetry();
        }, 2000 * (retryCount + 1)); // Exponential backoff
      }
    }
  };

  return (
    <div>
      <button onClick={loadWithRetry}>Load Content</button>

      {error && (
        <div style={{ color: 'red', marginTop: '10px' }}>
          Error: {error}
          {retryCount > 0 && <p>Retry attempt {retryCount}...</p>}
        </div>
      )}

      {items.length > 0 && (
        <AccordionComponent style={{ marginTop: '15px' }}>
          <AccordionItemsDirective>
            {items.map((item) => (
              <AccordionItemDirective 
                key={item.id}
                header={item.header}
                content={item.content}
              />
            ))}
          </AccordionItemsDirective>
        </AccordionComponent>
      )}
    </div>
  );
}
```

## Template-Based Rendering

Use JSX templates for complex content layouts:

### Custom Template Content

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const productContent = () => (
    <div style={{ padding: '15px' }}>
      <div style={{ display: 'flex', gap: '15px' }}>
        <img 
          src='/product-image.jpg' 
          alt='Product' 
          style={{ width: '100px', height: '100px' }} 
        />
        <div>
          <h4>Product Name</h4>
          <p>Price: $99.99</p>
          <p>In Stock: Yes</p>
          <button>Add to Cart</button>
        </div>
      </div>
    </div>
  );

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          header='Featured Product' 
          content={productContent} 
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Dynamic Template with Data

```jsx
const items = [
  { id: 1, title: 'Item 1', price: 10, inStock: true },
  { id: 2, title: 'Item 2', price: 20, inStock: false }
];

const itemTemplate = (item) => (
  <div style={{ padding: '15px', borderTop: '1px solid #ddd' }}>
    <h4>{item.title}</h4>
    <p>Price: ${item.price}</p>
    <p>Status: {item.inStock ? '✓ In Stock' : '✗ Out of Stock'}</p>
  </div>
);

<AccordionComponent>
  <AccordionItemsDirective>
    {items.map((item) => (
      <AccordionItemDirective 
        key={item.id}
        header={item.title}
        content={() => itemTemplate(item)}
      />
    ))}
  </AccordionItemsDirective>
</AccordionComponent>
```

## Rendering Other React Components

Nest other React components inside accordion panels:

### Basic Component Nesting

```jsx
import React, { useState } from 'react';
import { AccordionComponent } from '@syncfusion/ej2-react-navigations';
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';
import { ButtonComponent } from '@syncfusion/ej2-react-buttons';

function UserForm() {
  const [name, setName] = useState('');

  return (
    <div style={{ padding: '15px' }}>
      <TextBoxComponent 
        placeholder='Enter name' 
        value={name}
        input={(e) => setName(e.value)}
      />
      <ButtonComponent style={{ marginTop: '10px' }}>
        Save
      </ButtonComponent>
    </div>
  );
}

export default function App() {
  return (
    <AccordionComponent>
      <div>
        <div><div>User Information</div></div>
        <div><div><UserForm /></div></div>
      </div>
    </AccordionComponent>
  );
}
```

### Multiple Syncfusion Components

```jsx
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';
import { CheckBoxComponent } from '@syncfusion/ej2-react-buttons';

function AdvancedSettings() {
  return (
    <div style={{ padding: '15px' }}>
      <div style={{ marginBottom: '15px' }}>
        <label>Select Date:</label>
        <DatePickerComponent />
      </div>

      <div style={{ marginBottom: '15px' }}>
        <label>Choose Option:</label>
        <DropDownListComponent dataSource={['Option 1', 'Option 2']} />
      </div>

      <div>
        <CheckBoxComponent label='Enable notifications' />
      </div>
    </div>
  );
}

<AccordionComponent>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      header='Settings' 
      content={() => <AdvancedSettings />}
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

## Dynamic Item Loading

Add or remove accordion items at runtime:

### Adding Items

```jsx
import React, { useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [items, setItems] = useState([
    { id: 1, header: 'Item 1', content: 'Content 1' },
    { id: 2, header: 'Item 2', content: 'Content 2' }
  ]);

  const addItem = () => {
    const newId = Math.max(...items.map(i => i.id)) + 1;
    setItems([
      ...items,
      { id: newId, header: `Item ${newId}`, content: `Content ${newId}` }
    ]);
  };

  return (
    <div>
      <button onClick={addItem} style={{ marginBottom: '15px' }}>
        Add Item
      </button>

      <AccordionComponent>
        <AccordionItemsDirective>
          {items.map((item) => (
            <AccordionItemDirective 
              key={item.id}
              header={item.header}
              content={item.content}
            />
          ))}
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### Removing Items

```jsx
const removeItem = (id) => {
  setItems(items.filter(item => item.id !== id));
};

const removeContent = (item) => (
  <div>
    <p>{item.content}</p>
    <button onClick={() => removeItem(item.id)}>Remove</button>
  </div>
);

<AccordionComponent>
  <AccordionItemsDirective>
    {items.map((item) => (
      <AccordionItemDirective 
        key={item.id}
        header={item.header}
        content={() => removeContent(item)}
      />
    ))}
  </AccordionItemsDirective>
</AccordionComponent>
```

## Lazy Loading Patterns

Load content only when panels expand:

### Lazy Load on Expand

```jsx
import React, { useState } from 'react';

export default function App() {
  const [contentCache, setContentCache] = useState({});

  const loadContent = async (id) => {
    if (contentCache[id]) return contentCache[id];

    const response = await fetch(`/api/content/${id}`);
    const data = await response.text();
    
    setContentCache(prev => ({ ...prev, [id]: data }));
    return data;
  };

  const handleExpand = async (id) => {
    await loadContent(id);
  };

  const content = (item) => (
    <div>
      {contentCache[item.id] ? (
        <p>{contentCache[item.id]}</p>
      ) : (
        <p>Loading...</p>
      )}
    </div>
  );

  return (
    <AccordionComponent onExpand={(e) => handleExpand(e.index)}>
      <AccordionItemsDirective>
        {/* accordion items */}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Progressive Loading

```jsx
import React, { useState, useEffect } from 'react';

const loadItemsInBatches = async (page = 1) => {
  const response = await fetch(`/api/items?page=${page}`);
  return response.json();
};

export default function App() {
  const [items, setItems] = useState([]);
  const [page, setPage] = useState(1);

  useEffect(() => {
    const loadMore = async () => {
      const newItems = await loadItemsInBatches(page);
      setItems(prev => [...prev, ...newItems]);
    };
    loadMore();
  }, [page]);

  return (
    <div>
      <AccordionComponent>
        <AccordionItemsDirective>
          {items.map((item) => (
            <AccordionItemDirective 
              key={item.id}
              header={item.name}
              content={item.description}
            />
          ))}
        </AccordionItemsDirective>
      </AccordionComponent>

      <button onClick={() => setPage(page + 1)}>Load More</button>
    </div>
  );
}
```

---

## Troubleshooting

**Issue: Content not displaying**
- Ensure `content` prop is provided for each item
- Check that content functions return valid JSX or strings
- Verify data is loaded before rendering

**Issue: HTTP content not loading**
- Check network tab in browser DevTools
- Verify API endpoint is correct and accessible
- Add error handling for failed requests
- Check CORS settings if fetching from different domain

**Issue: Components not re-rendering with new content**
- Ensure state updates trigger component re-render
- Use `key` prop in mapped accordion items
- Check for missing dependencies in useEffect

**Issue: Performance degradation with many items**
- Implement lazy loading for large datasets
- Use virtualization libraries for 100+ items
- Consider pagination instead of loading all items
