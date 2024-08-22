# React Router

## Installation and import

Install dependencies to your project.
```
npm install react-router
npm install react-router-dom
```
Import BrowserRouter at the top of your components structure.
```ts
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
```

## Implementation example

```ts
<Router>
   <header>
      <nav>
         <Link to="/">Home</Link>
         <Link to="/contact">Contact</Link>
      </nav>
   </header>
   <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/contact" element={<Contact />} />
      <Route path="*" element={<PageNotFound />} />
   </Routes>
</Router>
```

## useNavigate

`useNavigate` is a hook, which redirects the user to a different url just like `Link` component.
Compared to `Link`, `useNavigate` is used in a ts/js logic and not in JSX.

```ts
import { useNavigate } from "react-router-dom";

function MyComponent() {
  const navigate = useNavigate();

  function handleClick() {
    // Navigate to a specific route
    navigate('/contact');
  }

  return (
    <div>
      <button onClick={handleClick}>Go to Contact Page</button>
    </div>
  );
}
```