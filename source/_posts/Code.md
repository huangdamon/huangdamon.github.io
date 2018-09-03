---
title: Code
categories: coding
tags:
    - coding
---
#Code

## upload and download

>  Vue.js and Node.js

```javascript
/// vuejs (UI)
import axios from axios;
axios.post(`*****`, {
    data: data,
}, {
    responseType: 'blob',
})
    .then(response -> {
    this.generateFile(response.data);
})

function generateFile(data) {
	const url = window.URL.createObjectURL(new Blob([data]))
    const link = document.createElement('a')
    link.href = url
    link.setAttribute('download', fileName)
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
}
```

In Node.js, ==axios== not support send file and other form data.

```javascript
const requestP = require('request-promise');
const formData = {
    files: {
        value: new Buffer(file.buffer),
        options: {
            filename: filename,
        },
    },
}

let response = await requestP({
    method: 'POST',
    url: '',
    formData: formData,
})
```

