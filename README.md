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

```
npm install --save https://cdn.sheetjs.com/xlsx-0.20.2/xlsx-0.20.2.tgz
```

```js
import * as XLSX from 'xlsx';
```

### IMPORT

```js
const handleImport = (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (file) {
      const reader = new FileReader();


      reader.onload = (event: ProgressEvent<FileReader>) => {
        const workbook = XLSX.read(event.target?.result as string, { type: 'binary' });
        const sheetName = workbook.SheetNames[0];
        const sheet = workbook.Sheets[sheetName];
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




