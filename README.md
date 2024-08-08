# React notes

## Basic commands
```
npm create vite@latest . -- --template react-ts
```

```
npm run dev
```

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

      reader.onload = (event: ProgressEvent<FileReader>) => {
        const workbook = XLSX.read(event.target?.result as string, { type: 'binary' });
        const sheetName = workbook.SheetNames[0];
        const sheet = workbook.Sheets[sheetName];
        const sheetData: Data[] = XLSX.utils.sheet_to_json(sheet);

        const numberOfRows = sheet['!ref'] ? XLSX.utils.decode_range(sheet['!ref']).e.r + 1 : 0;
        console.log(`Number of rows: ${numberOfRows}`);

        setImportedData(sheetData);
      };

      reader.readAsBinaryString(file);
    }
  };

```

 
### EXPORT

```js
const handleExport = () => {
    console.log(companyData);
    let wb = XLSX.utils.book_new();
    let ws = XLSX.utils.json_to_sheet(companyData);


    XLSX.utils.book_append_sheet(wb, ws, 'sheet1');


    XLSX.writeFile(wb, 'export.xlsx');
  };
```




