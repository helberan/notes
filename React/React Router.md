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
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";

export const App = () => {
return (
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
   )
}
```

## useNavigate

`useNavigate` is a hook, which redirects the user to a different url just like `Link` component.

Compared to `Link`, `useNavigate` is used in a ts/js logic and not in JSX.

```ts
import { useNavigate } from "react-router-dom";

export const Home = () => {
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

## useParams

`useParams` is a hook, which is used to access parameters from the URL.

To use `useParams`, you need to set up the route with changeable parameters:
```ts
...
<Router>
   <Routes>
      {/* the route could look like this: "/posts/01" */}
      <Route path="/posts/:id" element={<Post />} />
   </Routes>
</Router>
```

```ts
import { useParams } from "react-router-dom";

export const Post = () => {
  const { id } = useParams();   //id = "01"

return (
  <div>
      <h2>Post ID: {id}</h2>
      {/* Fetch and display the post details based on the ID here */}
      <p>This is the content of post {id}.</p>
    </div>
   )
}
```

