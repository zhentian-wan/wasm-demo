#include <math.h>

```C
void consoleLog (float num);

float getSqrt (float num) {
  consoleLog(num);
  return sqrt(num);
}
```

```bash
(module
  (type $FUNCSIG$vf (func (param f32)))
  (type $FUNCSIG$ff (func (param f32) (result f32)))
  (import "env" "consoleLog" (func $consoleLog (param f32)))
  (table 0 anyfunc)
  (memory $0 1)
  (export "memory" (memory $0))
  (export "getSqrt" (func $getSqrt))
  (func $getSqrt (param $0 f32) (result f32)
    (call $consoleLog
      (get_local $0)
    )
    (f32.sqrt
      (get_local $0)
    )
  )
)
```

```html
<!doctype>
<html>
<header>
    <title>
        WASM
    </title>
    <script>

        function fetchAndInstantiateWasm(url, imports) {
            return fetch(url)
                .then((res) => {
                    if (res.ok) {
                        return res.arrayBuffer();
                    }
                    throw new Error('Unable to fetch WASM')
                })
                .then((bytes) => {
                    return WebAssembly.compile(bytes);
                })
                .then(module => {
                    return WebAssembly.instantiate(module, imports || {});
                })
                .then(instance => instance.exports);
        }

        fetchAndInstantiateWasm('./program.wasm', {
            env: {
                consoleLog: (num) => console.log(num) 
            }
        })
            .then(m => {
                window.getSqrt = m.getSqrt;
            });
    </script>
</header>
</html>
```



