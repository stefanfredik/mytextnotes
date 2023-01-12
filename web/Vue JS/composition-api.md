# VUE COMPOSITION API

## Ref (ref)

Ref merupakan salah satu cara untuk membuat sebuah variabel/data menjadi reaktif.
ref hanya digunakan untuk tipe data primitif (int,boolean,string) dan tidak dapat digunakan untuk tipe data objek.

### Import

```javascript
<script>
  import {ref} from "vue"; export default{}
</script>
```

### Declarae Value

```javascript
<script>
     import {ref} from "vue"; export default{}
    export default{
        const text = ref("hello word");
    }

    return{
        text
    }
</script>
```

### Access Ref Value

```javascript
<script>
    export default{
        const text = ref("hello word");
    }

    setTimout(()=>{
        text.value = "Hallo Dunia";
    },2000);

    return{
        text
    }
</script>
```

## Reactive (reactive)

Reactive digunakan untuk membuat data object menjadi reaktif.

### Import

```javascript
<script>
  import {reactive} from "vue"; export default{}
</script>
```

### Declare

```javascript

<script>
    import {reactive} from "vue"; export default{}
    export default{
        const user = reactive({
            name: "Fredik Stefan",
            age: 28
        });
    }

    return{
        user
    }
</script>
```

## ToRefs (toRefs)

Digunakan untuk mendstruct tipe data object sehingga bisa di akses dan diubah nilainya. Dalam kata lain, torefs digunakan untuk mengconvert "reactive" menjadi "ref"

### Penggunaan

Sebelum menggunakan torefs

```javascript
<template>
    <p>{{user.nama}}</p>
    <p>{{user.age}}</p>
</template>
<script>
    import {reactive} from "vue"; export default{}
    export default{
        const user = reactive({
            name: "Fredik Stefan",
            age: 28
        });
    }

    return{
        user
    }
</script>
```

Setelah menggunakan toRefs

```javascript
<template>
    <p>{{nama}}</p>
    <p>{{age}}</p>
</template>

<script>
    import {reactive} from "vue"; export default{}
    export default{
        const user = reactive({
            name: "Fredik Stefan",
            age: 28
        });
    }

    return{
        ...toRefs(user)
    }
</script>
```

## Methods

Merupakan method/fungsi yang dapat digunakan untuk melakukan sesuatu. Method dapat dibuat menggunakan variable function.

### Decalare Method

```javascript
<template>
  <div>
    <p>Counter {{ counter }}</p>
    <button @click="add">Tambah</button>
  </div>
</template>
<script>
  export default{
      import {ref} from "vue";

      setup(){
          const counter = ref(0);

          const add = ()=>{
              counter.value++;
          }

          return {
              counter,add
          }
      }
  }
</script>
```

## Computed (computed)

Merupakan state atau kondisi yang terjadi ketika sebuah event sedang dikerjakan.

### Example

```javascript
<template>
  <div>
    <p>Counter : {{ nilai }}</p>
    <p>Result : {{ result }}</p>
    <button @click="add">Add</button>
  </div>
</template>

<script>
  import {reactive, toRefs, computed } from "vue";

  export default {
    setup() {

      const counter = reactive({
        nilai: 0,
      });

      const add = () => {
        counter.nilai++;
      };

      const addNum = ref(1);
      const result = computed(() => counter.nilai + addNum.value);

      return {
        ...toRefs(counter),
        add,
        result,
      };
    },
  };
</script>
```

Result akan terus ikut bertambah ketika Counter bertambah. Karena ketika sedang menalakukan penambahan counter, result juga akan ikut dikerjakan karena masuk dalam method computed.

### Computed Get dan Setter

Getter dan setter di gunakan untuk get dan set variable/data.

## Watch

Watch merupakan kondisi untuk memantau perubahan pada sebuah state. Ketika setup berjalan maka watch pun ikut berjalan sambil memantau perubahan yang terjadi.

### Penggunaan

```javascript
<template></template>
<script>
    export default{
        setup(){
            const counter = reactive({
                nilai: 0
            });

            const add = ()=>{
                counter.nilai++;
            }

            watchEffect(()=>{
                console.log(counter.foo);
            })
        }
    }
</script>
```
