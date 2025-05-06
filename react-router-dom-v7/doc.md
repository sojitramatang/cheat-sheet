# React Router v7

## Router

React Router v7 introduces a more streamlined approach to routing configuration, particularly for server-side rendering (SSR).

### Configuration

Routes are configured in `app/routes.ts` using the following imports:
```typescript
import {
  type RouteConfig,
  route,
  layout,
  index,
  prefix,
} from "@react-router/dev/routes";
```

Basic route configuration example:
```typescript
export default [
  index("./home.tsx"),
  route("about", "./about.tsx"),
  layout("./auth/layout.tsx", [
    route("login", "./auth/login.tsx"),
    route("register", "./auth/register.tsx"),
  ])
] satisfies RouteConfig;
```

### Route Types

1. **Basic Routes**: 
   - Defined using `route("path", "component-file")`
   - Example: `route("about", "./about.tsx")`

2. **Index Routes**:
   - Default child route rendered at parent's URL
   - Defined using `index("component-file")`
   - Cannot have children
   - Example: `index("./home.tsx")`

3. **Layout Routes**:
   - Create nesting without adding URL segments
   - Defined using `layout("component-file", [child-routes])`
   - Example: `layout("./auth/layout.tsx", [...])`

4. **Dynamic Routes**:
   - Use `:paramName` for URL parameters
   - Example: `route("users/:userId", "./user.tsx")`

5. **Optional Routes**:
   - Add `?` for optional segments
   - Example: `route(":lang?/categories", "./categories.tsx")`

6. **Splat Routes**:
   - Use `*` to catch all remaining segments
   - Example: `route("files/*", "./files.tsx")`

### Nested Routing

Routes can be nested in two ways:

1. **Direct Nesting**:
```typescript
route("dashboard", "./dashboard.tsx", [
  index("./dashboard-home.tsx"),
  route("settings", "./dashboard-settings.tsx")
])
```

2. **Using Prefixes**:
```typescript
prefix("projects", [
  index("./projects/home.tsx"),
  layout("./projects/project-layout.tsx", [
    route(":pid", "./projects/project.tsx"),
    route(":pid/edit", "./projects/edit-project.tsx")
  ])
])
```

## Route Module

Route modules are the foundation of React Router's framework features. They define various aspects of routing behavior including:

- Automatic code-splitting
- Data loading
- Actions
- Revalidation
- Error boundaries

### Available Exports

#### 1. Default Component Export

The main component that renders when the route matches:

```typescript
export default function RouteComponent({ 
  loaderData,  // Data from loader
  actionData,  // Data from action
  params,      // URL parameters
  matches      // All matches in current route tree
}: Route.ComponentProps) {
  return <div>/* Your component JSX */</div>;
}
```

#### 2. Loader

Server-side data loading before component render:

```typescript
export async function loader({ params }: Route.LoaderArgs) {
  const data = await fetchData(params.id);
  return data;
}
```

#### 3. Client Loader

Browser-specific data loading, can complement or replace server loader:

```typescript
export async function clientLoader({ serverLoader }) {
  // Optional: call server loader
  const serverData = await serverLoader();
  // Client-specific data fetching
  const clientData = await fetchClientData();
  return { ...serverData, ...clientData };
}

// Enable hydration support
clientLoader.hydrate = true as const;
```

#### 4. Action

Handles form submissions and data mutations:

```typescript
export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  // Process the form data
  return await saveData(formData);
}
```

#### 5. Error Boundary

Custom error handling component:

```typescript
export function ErrorBoundary({ error }: { error: Error }) {
  return (
    <div className="error">
      <h1>Error</h1>
      <p>{error.message}</p>
    </div>
  );
}
```

#### 6. Headers

Define HTTP headers for server responses:

```typescript
export function headers() {
  return {
    "Cache-Control": "max-age=300, s-maxage=3600",
  };
}
```

#### 7. Meta

Define metadata for the route:

```typescript
export function meta() {
  return [
    { title: "Page Title" },
    { name: "description", content: "Page description" },
  ];
}
```

#### 8. Links

Define `<link>` elements for the document head:

```typescript
export function links() {
  return [
    { rel: "stylesheet", href: "/styles.css" },
    { rel: "icon", href: "/favicon.png" },
  ];
}
```

#### 9. Handle

Custom data for route matching abstractions:

```typescript
export const handle = {
  // Custom data for useMatches()
  crumb: (data: any) => <Link to="/path">Name</Link>,
};
```

### Usage with TypeScript

Route modules can be strongly typed using the built-in types:

```typescript
import type { Route } from "@react-router/types";

export async function loader({ 
  params,
  request 
}: Route.LoaderArgs) {
  // Fully typed params and request
}

export default function Component({ 
  loaderData 
}: Route.ComponentProps<typeof loader>) {
  // Fully typed loaderData based on loader return type
}
```

## Rendering Strategies

React Router v7 supports three primary rendering strategies:

### 1. Client Side Rendering (CSR)

Routes are rendered on the client side as users navigate through the application. This is ideal for Single Page Applications (SPAs). To enable client-side only rendering, configure your app:

```typescript
import type { Config } from "@react-router/dev/config";

export default {
  ssr: false,
} satisfies Config;
```

### 2. Server Side Rendering (SSR)

Enables server-side rendering for improved initial page load and SEO. Individual routes can opt out using `clientLoader`. Enable SSR with:

```typescript
import type { Config } from "@react-router/dev/config";

export default {
  ssr: true,
} satisfies Config;
```

Note: SSR requires a deployment environment that supports server-side rendering.

### 3. Static Pre-rendering

Pre-renders routes at build time, generating static HTML and client navigation data payloads. This is particularly useful for:
- SEO optimization
- Performance improvements
- Deployments without server rendering capabilities

Configure static pre-rendering by specifying URLs to pre-render:

```typescript
import type { Config } from "@react-router/dev/config";

export default {
  async prerender() {
    return ["/", "/about", "/contact"];
  },
} satisfies Config;
```

During pre-rendering, route module loaders are executed at build time to fetch and include data in the static output.


## Data Loading

React Router v7 provides powerful data loading capabilities through loaders, which can run on both server and client. Data loading is handled declaratively and supports automatic serialization/deserialization.

### Loader Types

#### 1. Server Loader

The primary loader function runs on the server during SSR and handles client-side navigation requests:

```typescript
export async function loader({ 
  params,
  request 
}: Route.LoaderArgs) {
  const data = await fetchData(params.id);
  return data;
}
```

Server loaders are automatically removed from client bundles, allowing you to safely use server-only APIs.

#### 2. Client Loader

Used for client-specific data fetching:

```typescript
export async function clientLoader({ 
  serverLoader,
  params 
}: Route.ClientLoaderArgs) {
  // Optionally call server loader
  const serverData = await serverLoader();
  
  // Client-specific data fetching
  const clientData = await fetch(`/api/data/${params.id}`);
  return { ...serverData, ...await clientData.json() };
}
```

### Loading Strategies

#### 1. Server-Side Loading
- Default behavior when SSR is enabled
- `loader` function handles both initial page loads and client navigations
- Client navigations automatically fetch data through React Router

#### 2. Client-Side Loading
- Use `clientLoader` for browser-only data fetching
- Useful for pages that don't require server data
- Can complement server loader data

#### 3. Hybrid Loading
Combine both loaders for optimal performance:
```typescript
// Server data fetching
export async function loader({ params }: Route.LoaderArgs) {
  return await getServerData(params.id);
}

// Client-specific enhancements
export async function clientLoader({ serverLoader }: Route.ClientLoaderArgs) {
  const serverData = await serverLoader();
  const clientData = await getBrowserOnlyData();
  return { ...serverData, ...clientData };
}

// Force client loader during hydration
clientLoader.hydrate = true as const;

// Show while hydrating
export function HydrateFallback() {
  return <div>Loading...</div>;
}
```

### Using Loader Data

Access loader data in your components using the `loaderData` prop:

```typescript
export default function Component({ 
  loaderData 
}: Route.ComponentProps<typeof loader>) {
  return (
    <div>
      <h1>{loaderData.title}</h1>
      <p>{loaderData.description}</p>
    </div>
  );
}
```

### Static Data Loading

For pre-rendered routes, loaders are executed during the build process:

```typescript
// react-router.config.ts
export default {
  async prerender() {
    const routes = await getAllRoutes();
    return routes.map(route => route.path);
  },
} satisfies Config;
```

This enables:
- Data fetching at build time
- Static HTML generation
- Optimal performance for pre-rendered content
- SEO-friendly pages


## Actions

Actions are route-specific functions that handle data mutations in React Router. They provide a structured way to handle form submissions and other data updates.

### Action Types

#### 1. Server Actions

Server-only actions that run exclusively on the server and are removed from client bundles:

```typescript
export async function action({ 
  request,
  params 
}: Route.ActionArgs) {
  const formData = await request.formData();
  const title = formData.get("title");
  return await updateData(title);
}
```

#### 2. Client Actions

Browser-specific actions that take priority over server actions:

```typescript
export async function clientAction({ 
  request,
  serverAction 
}: Route.ClientActionArgs) {
  const formData = await request.formData();
  // Optionally call server action
  const serverResult = await serverAction();
  // Client-specific logic
  return await clientUpdate(formData);
}
```

### Calling Actions

There are three main ways to invoke actions:

#### 1. Using Forms

The declarative approach using the `<Form>` component:

```typescript
import { Form } from "react-router";

function ProjectForm() {
  return (
    <Form method="post">
      <input type="text" name="title" />
      <button type="submit">Save Project</button>
    </Form>
  );
}
```

#### 2. Using useSubmit

Programmatic submission using the `useSubmit` hook:

```typescript
import { useSubmit } from "react-router";

function ProjectManager() {
  const submit = useSubmit();

  const handleAutoSave = (data: FormData) => {
    submit(data, { 
      action: "/projects/save",
      method: "post"
    });
  };
}
```

##### Common Use Cases

1. **Auto-saving forms**:
```typescript
function AutoSaveForm() {
  const submit = useSubmit();
  const formRef = useRef<HTMLFormElement>(null);
  
  useEffect(() => {
    const interval = setInterval(() => {
      if (formRef.current) {
        submit(formRef.current, {
          action: "/auto-save",
          method: "post"
        });
      }
    }, 30000); // Auto-save every 30 seconds
    
    return () => clearInterval(interval);
  }, [submit]);

  return (
    <Form ref={formRef} method="post">
      <textarea name="content" />
    </Form>
  );
}
```

2. **Conditional submissions**:
```typescript
function ConditionalSubmit() {
  const submit = useSubmit();
  
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const value = e.target.value;
    
    if (value.length >= 3) {
      submit(
        { search: value },
        { 
          action: "/search",
          method: "get",
          replace: true // Replace current history entry
        }
      );
    }
  };

  return <input onChange={handleChange} />;
}
```

3. **Multi-step form submission**:
```typescript
function MultiStepForm() {
  const submit = useSubmit();
  const [step, setStep] = useState(1);
  const formData = useRef(new FormData());

  const handleStepSubmit = (newData: FormData) => {
    // Merge new data with existing
    for (const [key, value] of newData.entries()) {
      formData.current.set(key, value);
    }

    if (step === 3) { // Final step
      submit(formData.current, {
        action: "/complete-registration",
        method: "post"
      });
    } else {
      setStep(step + 1);
    }
  };
}
```

#### 3. Using Fetchers

Non-navigational submissions using `useFetcher`:

```typescript
import { useFetcher } from "react-router";

function TaskItem() {
  const fetcher = useFetcher();
  const busy = fetcher.state !== "idle";

  return (
    <fetcher.Form method="post" action="/tasks/update">
      <input type="text" name="title" />
      <button type="submit">
        {busy ? "Saving..." : "Save"}
      </button>
    </fetcher.Form>
  );
}
```

### Action Results

- Actions can return any serializable data
- Return data is available in components via `actionData` prop
- After action completion, all route loaders automatically revalidate
- Errors thrown in actions are caught by error boundaries

### TypeScript Support

Actions can be strongly typed using built-in types:

```typescript
import type { Route } from "@react-router/types";

export async function action({ 
  request 
}: Route.ActionArgs) {
  const formData = await request.formData();
  return { success: true };
}

export default function Component({ 
  actionData 
}: Route.ComponentProps<typeof action>) {
  // actionData is typed based on action return type
}
```

## Navigating

React Router v7 provides several ways to handle navigation in your application. Each method serves different use cases and provides specific features for both declarative and programmatic navigation.

### Link Component

The basic navigation component for client-side navigation:

```typescript
import { Link } from "react-router";

function Navigation() {
  return (
    <Link to="/dashboard">Dashboard</Link>
  );
}
```

### NavLink Component

Enhanced version of Link that supports active and pending states:

```typescript
import { NavLink } from "react-router";

function Navigation() {
  return (
    <NavLink 
      to="/tasks"
      className={({ isActive, isPending, isTransitioning }) => [
        isPending ? "pending" : "",
        isActive ? "active" : "",
        isTransitioning ? "transitioning" : "",
      ].join(" ")}
    >
      Tasks
    </NavLink>
  );
}
```

CSS styling example:
```css
a.active {
  color: red;
}

a.pending {
  animate: pulse 1s infinite;
}

a.transitioning {
  /* css transition styles */
}
```

### Form Navigation

Forms can be used for navigation with search parameters or data submission:

```typescript
import { Form } from "react-router";

function SearchForm() {
  return (
    <Form action="/search">
      <input type="text" name="q" />
      <button type="submit">Search</button>
    </Form>
  );
}
```

### Programmatic Navigation

#### 1. Using redirect

For server-side redirects in loaders and actions:

```typescript
import { redirect } from "react-router";

export async function action({ request }: Route.ActionArgs) {
  const project = await createProject(request);
  return redirect(`/projects/${project.id}`);
}
```

#### 2. Using useNavigate

For programmatic navigation in components:

```typescript
import { useNavigate } from "react-router";

function AutoLogout() {
  const navigate = useNavigate();

  useEffect(() => {
    const timer = setTimeout(() => {
      navigate("/logout");
    }, 30000);

    return () => clearTimeout(timer);
  }, [navigate]);

  return <div>You will be logged out after inactivity</div>;
}
```

### Navigation Best Practices

1. **Prefer Declarative Navigation**:
   - Use `<Link>` or `<NavLink>` for user-initiated navigation
   - Use `<Form>` for data submission with navigation

2. **When to Use Programmatic Navigation**:
   - Automated redirects (e.g., after inactivity)
   - Complex navigation logic
   - Timed transitions

3. **Server vs Client Navigation**:
   - Use `redirect()` in loaders/actions for server-side redirects
   - Use `useNavigate()` for client-side programmatic navigation

## Pending UI

React Router v7 provides several ways to show pending states during navigation and form submissions, improving user experience by providing immediate feedback.

### Global Pending Navigation

Use `useNavigation` hook to show pending states during route transitions:

```typescript
import { useNavigation, Outlet } from "react-router";

export default function Root() {
  const navigation = useNavigation();
  const isNavigating = Boolean(navigation.location);

  return (
    <div>
      {isNavigating && <GlobalSpinner />}
      <Outlet />
    </div>
  );
}
```

### Local Pending Navigation

`NavLink` components support pending states through render props:

```typescript
function Navigation() {
  return (
    <nav>
      <NavLink to="/home">
        {({ isPending }) => (
          <span>
            Home {isPending && <Spinner />}
          </span>
        )}
      </NavLink>

      <NavLink
        to="/about"
        style={({ isPending }) => ({
          color: isPending ? "gray" : "black",
        })}
      >
        About
      </NavLink>
    </nav>
  );
}
```

### Form Submission States

#### 1. Global Form Submission

Track form submissions using `useNavigation`:

```typescript
function NewProjectForm() {
  const navigation = useNavigation();
  
  return (
    <Form method="post" action="/projects/new">
      <input type="text" name="title" />
      <button type="submit">
        {navigation.formAction === "/projects/new"
          ? "Submitting..."
          : "Submit"
        }
      </button>
    </Form>
  );
}
```

#### 2. Local Form Submission

Use `useFetcher` for non-navigation form submissions:

```typescript
function TaskItem() {
  const fetcher = useFetcher();
  const busy = fetcher.state !== "idle";

  return (
    <fetcher.Form method="post">
      <input type="text" name="title" />
      <button type="submit">
        {busy ? "Saving..." : "Save"}
      </button>
    </fetcher.Form>
  );
}
```

### Optimistic UI

Implement optimistic updates using fetcher's formData:

```typescript
function Task({ task }) {
  const fetcher = useFetcher();
  
  // Optimistically determine completion status
  let isComplete = task.status === "complete";
  if (fetcher.formData) {
    isComplete = fetcher.formData.get("status") === "complete";
  }

  return (
    <div>
      <div>{task.title}</div>
      <fetcher.Form method="post">
        <button
          name="status"
          value={isComplete ? "incomplete" : "complete"}
        >
          {isComplete ? "Mark Incomplete" : "Mark Complete"}
        </button>
      </fetcher.Form>
    </div>
  );
}
```

This optimistic UI pattern provides immediate feedback while the actual server request is processing in the background.


## Testing

React Router v7 provides powerful testing utilities that make it easy to test components that use routing features. The main testing utility is `createRoutesStub`, which creates a routing context for testing components in isolation.

### Setting Up Tests

To test components that use React Router hooks and components, you'll need these dependencies:

```typescript
import { createRoutesStub } from "react-router";
import { render, screen, waitFor } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
```

### Testing Components with Router Context

Components that use router hooks (like `useLoaderData`, `useActionData`, `useNavigate`, etc.) need to be wrapped in a router context. Here's a complete example:

```typescript
// LoginForm.tsx
export function LoginForm() {
  const { errors } = useActionData();
  
  return (
    <Form method="post">
      <label>
        <input type="text" name="username" />
        {errors?.username && <div>{errors.username}</div>}
      </label>
      
      <label>
        <input type="password" name="password" />
        {errors?.password && <div>{errors.password}</div>}
      </label>
      
      <button type="submit">Login</button>
    </Form>
  );
}

// LoginForm.test.tsx
test("LoginForm renders error messages", async () => {
  const USER_MESSAGE = "Username is required";
  const PASSWORD_MESSAGE = "Password is required";

  const Stub = createRoutesStub([
    {
      path: "/login",
      Component: LoginForm,
      action() {
        return {
          errors: {
            username: USER_MESSAGE,
            password: PASSWORD_MESSAGE,
          },
        };
      },
    },
  ]);

  // Render the app stub at "/login"
  render(<Stub initialEntries={["/login"]} />);

  // Simulate user interactions
  userEvent.click(screen.getByText("Login"));
  
  // Assert error messages appear
  await waitFor(() => screen.findByText(USER_MESSAGE));
  await waitFor(() => screen.findByText(PASSWORD_MESSAGE));
});
```

### Testing Route Modules

When testing complete route modules, you can test all exports:

```typescript
import { createRoutesStub } from "react-router";
import * as routeModule from "./route";

test("route module loader", async () => {
  const Stub = createRoutesStub([
    {
      path: "/products/:id",
      ...routeModule,
    },
  ]);

  render(<Stub initialEntries={["/products/123"]} />);
  
  // Test loader data
  await waitFor(() => {
    expect(screen.getByText("Product 123")).toBeInTheDocument();
  });
});
```

### Testing Navigation

Test navigation behavior using `userEvent`:

```typescript
test("navigates to product detail", async () => {
  const Stub = createRoutesStub([
    {
      path: "/",
      Component: ProductList,
    },
    {
      path: "/products/:id",
      Component: ProductDetail,
    },
  ]);

  render(<Stub initialEntries={["/"]} />);

  // Click a product link
  userEvent.click(screen.getByText("View Product"));

  // Assert navigation occurred
  await waitFor(() => {
    expect(screen.getByText("Product Details")).toBeInTheDocument();
  });
});
```

### Testing Best Practices

1. **Isolated Tests**:
   - Use `createRoutesStub` to test components in isolation
   - Mock network requests in loaders and actions
   - Test one route module at a time

2. **Route Configuration**:
   - Test that routes are configured correctly
   - Verify path parameters are handled properly
   - Test nested routes behave as expected

3. **User Interactions**:
   - Use `userEvent` for simulating user interactions
   - Test form submissions and navigation
   - Verify loading and error states

4. **Data Flow**:
   - Test loader and action functions separately
   - Verify data is correctly passed to components
   - Test error handling and validation
