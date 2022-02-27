# moorepay.co.uk-data-extractor

```javascript

function exportToCSV(rows, filename) {

    var header = Object.getOwnPropertyNames(rows[0]);
    var csv = [header.join("\t")];
    console.log("HEADER: ", header);

    while (rows.length) {
        let row = rows.shift();
        let csvRow = [];
        for(var i=0; i<header.length; i++){
            csvRow.push(
                row.hasOwnProperty(header[i]) ? row[header[i]] : ""
            )
        }
        csv.push(csvRow.join("\t"));
    }

    downloadCSV(csv.join("\n"), filename);
}

function downloadCSV(csv, filename) {
    var csvFile;
    var downloadLink;
    csvFile = new Blob([csv], {type:"text/csv"});
    downloadLink = document.createElement("a");
    downloadLink.download = filename;
    downloadLink.href = window.URL.createObjectURL(csvFile);
    downloadLink.style.display = "none";
    document.body.appendChild(downloadLink);
    downloadLink.click();
}

let headers = Array.from(document.querySelectorAll('div.x-column-header.tablelabel span.x-column-header-text')).map(x => x.innerText).filter(x => x.replace(' ', '').length)
let data = Array.from(document.querySelectorAll('.x-grid-row')).map(xrow => Array.from(xrow.querySelectorAll('input')).map(inp => inp.value)).filter(ar => ar.length != 0);
let pays = [];
while (data.length) {
    let pay = {};
    let d = data.shift();
    for (var i=0; i<headers.length; i++) {
        pay[headers[i]] = d[i];
    }
    pays.push(pay);
}

exportToCSV(pays, 'z.tsv');


```
