## Write data from JS

C Code:
```c
void consoleLog (char* offset, int len);

char inStr[20];
char outStr[20];

char* getInStrOffset () {
  return &inStr[0];
}

void toLowerCase() {
  for (int i = 0; i < 20; i++ ) {
    char c = inStr[i];
    if (c > 64 &&  c < 91) {
      c = c + 32;
    }
    outStr[i] = c;
  }
  consoleLog(&outStr[0], 20);
}
```

JS Code:
```js
var wasmModule = new WebAssembly.Module(wasmCode);
var wasmInstance = new WebAssembly.Instance(wasmModule, {
  env: {
    consoleLog (offset, len) {
      const strBuf = new Uint8Array(mem.buffer, offset, len);
      log(new TextDecoder().decode(strBuf));
    }
  }
});
const mem = wasmInstance.exports.memory;

function writeString (str, offset) {
  const strBuf = new TextEncoder().encode(str);
  const outBuf = new Uint8Array(mem.buffer, offset, strBuf.length);
  



      for (let i = 0; i < strBuf.length; i++) {
        outBuf[i] = strBuf[i];
      }
  }
}

writeString('Hello Web Assembly', wasmInstance.exports.getInStrOffset());
wasmInstance.exports.toLowerCase();
```