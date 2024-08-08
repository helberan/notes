# React notes

## Basic commands

Create a new project using [Vite](https://vitejs.dev/guide/) and a TypeScript template
```
npm create vite@latest . -- --template react-ts
```
Run the app
```
npm run dev
```
Build the app
```
npm run build
```

## React Router

### Installation and import

Install dependencies to your project.
```
npm install react-router
npm install react-router-dom
```
Import BrowserRouter at the top of your components structure.
```ts
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
```

### Implementation example

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

### useNavigate

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

## Redux

## Security

## EXCEL data import, reading, export

More info at [SheetJS](https://docs.sheetjs.com/docs/getting-started/installation/frameworks/).

### Installation and import

Install dependency to your project.
```
npm install --save https://cdn.sheetjs.com/xlsx-0.20.2/xlsx-0.20.2.tgz
```
Import XLSX where you want to use it.
```ts
import * as XLSX from 'xlsx';
```

### IMPORT

I have created a button that lets you import a .xlsx document, once the document is selected, the function `handleImport` is called.

```ts
const handleImport = (e: React.ChangeEvent<HTMLInputElement>) => {
    //checks whether the user selected a file or not and if he selected multiple files, it selects the first one
    const file = e.target.files?.[0];

    if (file) {
      //a new FileReader object that is used to read the loaded file
      const reader = new FileReader();

      //here we read and extract the first sheet (through sheetName), sheetData and numberOfRows from the imported file
      reader.onload = (event: ProgressEvent<FileReader>) => {
        const workbook = XLSX.read(event.target?.result as string, { type: 'binary' });
        const sheetName = workbook.SheetNames[0];
        const sheet = workbook.Sheets[sheetName];
        const sheetData: Data[] = XLSX.utils.sheet_to_json(sheet);
        const numberOfRows = sheet['!ref'] ? XLSX.utils.decode_range(sheet['!ref']).e.r + 1 : 0;

        //store the sheetData to be used in the app
        setImportedData(sheetData);
      };

      //this function triggers the file read that we defined in the reader.onload
      reader.readAsBinaryString(file);
    }
  };

```

 
### EXPORT

I have created a button that lets you export data to a .xlsx document, once the button is clicked a `handleExport` function is called.

```ts
const handleExport = () => {
    //here we create a new, empty Excel workbook object
    let wb = XLSX.utils.book_new();

    //here we convert our stored JSON data into a format that represents an Excel worksheet
    let ws = XLSX.utils.json_to_sheet(data);

    //this line adds the worksheet (ws) to the workbook (wb) with the name 'sheet1'
    XLSX.utils.book_append_sheet(wb, ws, 'sheet1');

    //this writes the workbook (wb) to a file named 'export.xlsx' and triggers a download
    XLSX.writeFile(wb, 'export.xlsx');
  };
```




