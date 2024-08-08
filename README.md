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



## Redux

## Security

## EXCEL data import, reading, export

More info at [https://docs.sheetjs.com/docs/getting-started/installation/frameworks/](https://docs.sheetjs.com/docs/getting-started/installation/frameworks/).

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




