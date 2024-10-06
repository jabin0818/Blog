# js基础

## 闭包

- 最简单闭包示例：

  ```js
  function test() {
    const a = "111";
    function fun() {
      console.log(a);
    }
    return fun;
  }

  const test1 = test();
  test1();
  console.log("a");
  ```

  ​

