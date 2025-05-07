# React 19

## Table of Contents
- [React 19](#react-19)
  - [Table of Contents](#table-of-contents)
  - [useTransition](#usetransition)
    - [Before Actions](#before-actions)
    - [After Actions](#after-actions)
    - [Explanation](#explanation)
    - [Additional Notes](#additional-notes)
  - [useActionState](#useactionstate)
    - [Explanation](#explanation-1)
    - [Additional Notes](#additional-notes-1)
  - [useFormStatus](#useformstatus)
    - [Explanation](#explanation-2)
  - [useOptimistic](#useoptimistic)
    - [Explanation](#explanation-3)
  - [use](#use)
    - [Explanation](#explanation-4)
    - [Additional Notes](#additional-notes-2)
    - [You can also read context with use, allowing you to read Context conditionally such as after early returns](#you-can-also-read-context-with-use-allowing-you-to-read-context-conditionally-such-as-after-early-returns)
    - [Explanation](#explanation-5)
  - [React DOM Static APIs](#react-dom-static-apis)
  - [Server Components](#server-components)
    - [Key Features](#key-features)
    - [Important Notes](#important-notes)
  - [Server Functions](#server-functions)
    - [Key Concepts](#key-concepts)
    - [Creating a Server Function from a Server Component](#creating-a-server-function-from-a-server-component)
    - [Importing Server Functions from Client Components](#importing-server-functions-from-client-components)
    - [Server Functions with Actions](#server-functions-with-actions)
    - [Server Functions with Form Actions](#server-functions-with-form-actions)
    - [Progressive Enhancement with useActionState](#progressive-enhancement-with-useactionstate)
  - [ref](#ref)
  - [Context Updates](#context-updates)
    - [Context as Provider](#context-as-provider)
    - [Context with use Hook](#context-with-use-hook)
  - [Cleanup functions for refs](#cleanup-functions-for-refs)
  - [useDeferredValue](#usedeferredvalue)
  - [Support for stylesheets](#support-for-stylesheets)
  - [Support for async scripts](#support-for-async-scripts)
  - [Support for preloading resources](#support-for-preloading-resources)
    - [Explanation](#explanation-6)

---

## useTransition

```javascript
import { useTransition } from 'react';
import { redirect } from 'react-router-dom';
```

### Before Actions
```javascript
function UpdateName({}) {
  const [name, setName] = useState("");
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);

  const handleSubmit = async () => {
    setIsPending(true);
    const error = await updateName(name);
    setIsPending(false);
    if (error) {
      setError(error);
      return;
    } 
    redirect("/path");
  };

  return (
    <div>
      <input value={name} onChange={(event) => setName(event.target.value)} />
      <button onClick={handleSubmit} disabled={isPending}>
        Update
      </button>
      {error && <p>{error}</p>}
    </div>
  );
}
```

### After Actions
```javascript
function UpdateName({}) {
  const [name, setName] = useState("");
  const [error, setError] = useState(null);
  const [isPending, startTransition] = useTransition();

  const handleSubmit = () => {
    startTransition(async () => {
      const error = await updateName(name);
      if (error) {
        setError(error);
        return;
      } 
      redirect("/path");
    })
  };

  return (
    <div>
      <input value={name} onChange={(event) => setName(event.target.value)} />
      <button onClick={handleSubmit} disabled={isPending}>
        Update
      </button>
      {error && <p>{error}</p>}
    </div>
  );
}
```

### Explanation
- **useTransition**: A hook for marking state updates as transitional for optimized rendering
- **startTransition**: A function to wrap state updates that should be prioritized lower 
- **isPending**: A boolean flag indicating if transition is in progress
- **Error Handling**: Error logic is wrapped in startTransition for efficient state updates

### Additional Notes
- Part of React's concurrent mode for better UI responsiveness
- Improves performance by letting React prioritize updates

---

## useActionState

```javascript
function ChangeName({ name, setName }) {
  const [error, submitAction, isPending] = useActionState(
    async (previousState, formData) => {
      const error = await updateName(formData.get("name"));
      if (error) {
        return error;
      }
      redirect("/path");
      return null;
    },
    null,
  );

  return (
    <form action={submitAction}>
      <input type="text" name="name" />
      <button type="submit" disabled={isPending}>Update</button>
      {error && <p>{error}</p>}
    </form>
  );
}
```

### Explanation
- **useActionState**: This hook is used to manage the state of an action, including error handling and loading states. It takes a function that performs the action and returns an array containing the error, a function to submit the action, and a boolean indicating if the action is pending.
- **submitAction**: This function is used to trigger the action. It takes the form data as an argument and returns a promise that resolves when the action is complete.
- **isPending**: This boolean indicates whether the action is still in progress. You can use it to disable buttons or show loading indicators.
- **Error Handling**: The error handling logic is included in the action function. If an error occurs, it is returned and can be displayed in the UI.
- **Redirect**: The redirect function is called after the action is complete, allowing you to navigate to a different route.
### Additional Notes
- React.useActionState was previously called ReactDOM.useFormState in the Canary releases, but we’ve renamed it and deprecated useFormState.

---

## useFormStatus

```javascript
function SubmitButton() {
  const { pending } = useFormStatus();
  return (
    <button type="submit" disabled={pending}>
      {pending ? 'Submitting...' : 'Submit'}
    </button>
  );
}

function Form() {
  return (
    <form action={async (formData) => {
      // Handle form submission
    }}>
      <input type="text" name="name" />
      <SubmitButton />
    </form>
  );
}
```

### Explanation
- **useFormStatus**: A React hook that provides form status information
- **pending**: Boolean indicating if form submission is in progress
- **data**: FormData object containing form values
- **method**: HTTP method being used for submission
- **action**: URL or function handling the submission
- Must be used within a `<form>` element with an action prop
- Commonly used to show loading states and disable controls

---

## useOptimistic

```javascript
function ChangeName({currentName, onUpdateName}) {
  const [optimisticName, setOptimisticName] = useOptimistic(currentName);

  const submitAction = async formData => {
    const newName = formData.get("name");
    setOptimisticName(newName);
    const updatedName = await updateName(newName);
    onUpdateName(updatedName);
  };

  return (
    <form action={submitAction}>
      <p>Your name is: {optimisticName}</p>
      <p>
        <label>Change Name:</label>
        <input
          type="text"
          name="name"
          disabled={currentName !== optimisticName}
        />
      </p>
    </form>
  );
}
```

### Explanation
- **useOptimistic**: Hook for managing optimistic UI updates
- **optimisticName**: Value displayed immediately while action is processing
- **setOptimisticName**: Function to update the optimistic value
- **submitAction**: Function that updates UI optimistically then performs actual update

---

## use

```javascript
import {use} from 'react';

function Comments({commentsPromise}) {
  // `use` will suspend until the promise resolves.
  const comments = use(commentsPromise);
  return comments.map(comment => <p key={comment.id}>{comment}</p>);
}

function Page({commentsPromise}) {
  // When `use` suspends in Comments,
  // this Suspense boundary will be shown.
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Comments commentsPromise={commentsPromise} />
    </Suspense>
  )
}
```
### Explanation
- **use**: This hook is used to suspend the component until the promise resolves. It allows you to handle asynchronous data fetching in a more declarative way.
- **commentsPromise**: This is a promise that resolves to the comments data. It can be passed as a prop to the `Comments` component.
- **Suspense**: This component is used to handle the loading state while the promise is being resolved. It takes a fallback prop that specifies what to render while the promise is pending.
- **comments**: This variable holds the resolved comments data. It can be used to render the comments in the UI.
- **Error Handling**: You can use error boundaries to handle errors that occur during the promise resolution. If an error occurs, the error boundary will catch it and allow you to display an error message or fallback UI.
- **Performance**: Using `use` can improve the performance of your application by allowing React to suspend rendering until the data is available, reducing the need for loading states and improving user experience.

### Additional Notes
- use does not support promises created in render. 
- If you try to pass a promise created in render to `use`, React will warn:
`A component was suspended by an uncached promise. Creating promises inside a Client Component or hook is not yet supported, except via a Suspense-compatible library or framework.`


### You can also read context with use, allowing you to read Context conditionally such as after early returns
```javascript
import {use} from 'react';
import ThemeContext from './ThemeContext'

function Heading({children}) {
  if (children == null) {
    return null;
  }
  
  // This would not work with useContext
  // because of the early return.
  const theme = use(ThemeContext);
  return (
    <h1 style={{color: theme.color}}>
      {children}
    </h1>
  );
}
```

### Explanation
- **ThemeContext**: This is a context that provides the theme data. It can be created using `React.createContext`.
- **use**: This hook is used to read the context value. It allows you to access the context data even if the component has early returns.

---

## React DOM Static APIs
- added two new APIs to react-dom/static for static site generation: `prerender` and `prerenderToNodeStream`.

These new APIs improve on renderToString by waiting for data to load for static HTML generation. They are designed to work with streaming environments like Node.js Streams and Web Streams. For example, in a Web Stream environment, you can prerender a React tree to static HTML with prerender:

```javascript
import { prerender } from 'react-dom/static';

async function handler(request) {
  const {prelude} = await prerender(<App />, {
    bootstrapScripts: ['/main.js']
  });
  return new Response(prelude, {
    headers: { 'content-type': 'text/html' },
  });
}
```

Prerender APIs will wait for all data to load before returning the static HTML stream. Streams can be converted to strings, or sent with a streaming response. They do not support streaming content as it loads, which is supported by the existing React DOM server rendering APIs.

---

## Server Components

Server Components are a new type of Component that renders ahead of time, before bundling, in an environment separate from your client app or SSR server. They can run either at build time or for each request using a web server.

```javascript
// Server Component
import db from './database';

async function Note({ id }) {
  // Loads during render
  const note = await db.notes.get(id);
  return (
    <div>
      <Author id={note.authorId} />
      <p>{note}</p>
    </div>
  );
}

async function Author({ id }) {
  const author = await db.authors.get(id);
  return <span>By: {author.name}</span>;
}
```

### Key Features

1. **Direct Data Access**: Server Components can directly access databases or filesystems without an API layer.

2. **Async Components Support**:
```javascript
// Server Component with async/await
async function Page({ id }) {
  // Will suspend the Server Component
  const note = await db.notes.get(id);
  // Not awaited, will start here and await on the client
  const commentsPromise = db.comments.get(note.id);
  
  return (
    <div>
      {note}
      <Suspense fallback={<p>Loading Comments...</p>}>
        <Comments commentsPromise={commentsPromise} />
      </Suspense>
    </div>
  );
}
```

3. **Combining with Client Components**:
```javascript
// Server Component
import Expandable from './Expandable';

async function Notes() {
  const notes = await db.notes.getAll();
  return (
    <div>
      {notes.map(note => (
        <Expandable key={note.id}>
          <p note={note} />
        </Expandable>
      ))}
    </div>
  );
}
```

### Important Notes

- There is no directive for Server Components (don't confuse with "use server" which is for Server Functions)
- Server Components cannot use client-side hooks like `useState`
- They can be used both with and without a server:
  - **Without Server**: Run at build time for static content
  - **With Server**: Run on each request for dynamic content
- The bundler combines data, rendered Server Components, and dynamic Client Components into a final bundle
- Server Components can be made dynamic by re-fetching from the server

This architecture combines the "request/response" model of Multi-Page Apps with the interactivity of Single-Page Apps, offering benefits of both approaches.

---

## Server Functions

React 19 introduces Server Functions, which allow Client Components to call asynchronous functions executed on the server. This feature enhances the interaction between client and server components, making it easier to manage server-side logic.

### Key Concepts

- **Server Functions**: Functions defined in Server Components that can be called from Client Components.
- **Directives**: 
  - `'use client'`: Indicates that the component is a Client Component.
  - `'use server'`: Indicates that the function is a Server Function.

### Creating a Server Function from a Server Component

To create a Server Function, define it within a Server Component using the `'use server'` directive.

```javascript
// Server Component
import Button from './Button';

function EmptyNote() {
  async function createNoteAction() {
    // Server Function
    'use server';
    await db.notes.create();
  }

  return <Button onClick={createNoteAction} />;
}
```

### Importing Server Functions from Client Components

Client Components can import Server Functions defined in files that use the `'use server'` directive.

```javascript
// Server Function
'use server';
export async function createNote() {
  await db.notes.create();
}

// Client Component
'use client';
import { createNote } from './actions';

function EmptyNote() {
  return <button onClick={() => createNote()}>Create Empty Note</button>;
}
```

### Server Functions with Actions

Server Functions can be called from Actions on the client, allowing for better state management.

```javascript
// Server Function
'use server';
export async function updateName(name) {
  if (!name) {
    return { error: 'Name is required' };
  }
  await db.users.updateName(name);
}

// Client Component
'use client';
import { updateName } from './actions';
import { useState, useTransition } from 'react';

function UpdateName() {
  const [name, setName] = useState('');
  const [error, setError] = useState(null);
  const [isPending, startTransition] = useTransition();

  const submitAction = async () => {
    startTransition(async () => {
      const { error } = await updateName(name);
      if (error) {
        setError(error);
      } else {
        setName('');
      }
    });
  };

  return (
    <form action={submitAction}>
      <input type="text" name="name" disabled={isPending} />
      {error && <span>Failed: {error}</span>}
    </form>
  );
}
```

### Server Functions with Form Actions

Server Functions can be directly linked to forms for automatic submission.

```javascript
// Client Component
'use client';
import { updateName } from './actions';

function UpdateName() {
  return (
    <form action={updateName}>
      <input type="text" name="name" />
    </form>
  );
}
```

### Progressive Enhancement with useActionState

You can enhance user experience by providing a permalink for redirection if the form is submitted before the JavaScript bundle loads.

```javascript
// Client Component
'use client';
import { updateName } from './actions';
import { useActionState } from 'react';

function UpdateName() {
  const [, submitAction] = useActionState(updateName, null, `/name/update`);

  return (
    <form action={submitAction}>
      <input type="text" name="name" />
    </form>
  );
}
```

---

## ref

```javascript
function MyInput({placeholder, ref}) {
  return <input placeholder={placeholder} ref={ref} />
}

//...
<MyInput ref={ref} />
```
- this allows you to use refs in function components without needing to wrap them in a forwardRef.
- New function components will no longer need forwardRef, and we will be publishing a codemod to automatically update your components to use the new ref prop. In future versions we will deprecate and remove forwardRef.

---

## Context Updates

### Context as Provider

```javascript
const ThemeContext = createContext('');

function App({children}) {
  return (
    <ThemeContext value="dark">
      {children}
    </ThemeContext>
  );  
}
```

### Context with use Hook

```javascript
import { use } from 'react';
import ThemeContext from './ThemeContext';

function Heading({children}) {
  if (children == null) {
    return null;
  }
  const theme = use(ThemeContext);
  return (
    <h1 style={{color: theme.color}}>
      {children}
    </h1>
  );
}
```

---

## Cleanup functions for refs

now support returning a cleanup function from ref callbacks:

```javascript
<input
  ref={(ref) => {
    // ref created

    // NEW: return a cleanup function to reset
    // the ref when element is removed from DOM.
    return () => {
      // ref cleanup
    };
  }}
/>
```

---

## useDeferredValue
```javascript
import { useState, useDeferredValue } from 'react';

function SearchPage() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  // ...
}
```
- Call useDeferredValue at the top level of your component to get a deferred version of that value.
- The deferred value will be updated at a lower priority than the original value, allowing React to keep the UI responsive while the deferred value is being updated.

```javascript
//  App.js
import { Suspense, useState } from 'react';
import SearchResults from './SearchResults.js';

export default function App() {
  const [query, setQuery] = useState('');
  return (
    <>
      <label>
        Search albums:
        <input value={query} onChange={e => setQuery(e.target.value)} />
      </label>
      <Suspense fallback={<h2>Loading...</h2>}>
        <SearchResults query={query} />
      </Suspense>
    </>
  );
}
```

```javascript
// SearchResults.js
import {use} from 'react';
import { fetchData } from './data.js';

export default function SearchResults({ query }) {
  if (query === '') {
    return null;
  }
  const albums = use(fetchData(`/search?q=${query}`));
  if (albums.length === 0) {
    return <p>No matches for <i>"{query}"</i></p>;
  }
  return (
    <ul>
      {albums.map(album => (
        <li key={album.id}>
          {album.title} ({album.year})
        </li>
      ))}
    </ul>
  );
}
```

---

## Support for stylesheets

```javascript
function ComponentOne() {
  return (
    <Suspense fallback="loading...">
      <link rel="stylesheet" href="foo" precedence="default" />
      <link rel="stylesheet" href="bar" precedence="high" />
      <article class="foo-class bar-class">
        {...}
      </article>
    </Suspense>
  )
}

function ComponentTwo() {
  return (
    <div>
      <p>{...}</p>
      <link rel="stylesheet" href="baz" precedence="default" />  <-- will be inserted between foo & bar
    </div>
  )
}
```

---

## Support for async scripts
included better support for async scripts by allowing you to render them anywhere in your component tree, inside the components that actually depend on the script, without having to manage relocating and deduplicating script instances.

```javascript
function MyComponent() {
  return (
    <div>
      <script async={true} src="..." />
      Hello World
    </div>
  )
}

function App() {
  <html>
    <body>
      <MyComponent>
      ...
      <MyComponent> // won't lead to duplicate script in the DOM
    </body>
  </html>
}
```
---

## Support for preloading resources

```javascript
<!-- Resources are prioritized by their utility to early loading, not call order -->
<html>
  <head>
    <link rel="prefetch-dns" href="https://...">
    <link rel="preconnect" href="https://...">
    <link rel="preload" as="font" href="https://.../path/to/font.woff">
    <link rel="preload" as="style" href="https://.../path/to/stylesheet.css">
    <script async="" src="https://.../path/to/some/script.js"></script>
  </head>
  <body>
    <!-- Component content -->
  </body>
</html>
```

### Explanation
- Resource preloading allows for optimized loading of assets
- Resources are automatically prioritized based on their loading utility
- Supports various preloading strategies: prefetch-dns, preconnect, and preload
- Works seamlessly with async scripts and stylesheets
