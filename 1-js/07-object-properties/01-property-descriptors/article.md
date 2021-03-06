
# Các cờ và descriptor

Như ta đã biết, đối tượng có thể lưu các thuộc tính.

Đến lúc này, chúng ta chỉ biết chúng là một cặp "key-value" (phương thức là thuộc tính có `value` là hàm). Nhưng thuộc tính và phương thức của đối tượng thực sự linh hoạt và mạnh mẽ hơn nhiều.

Trong bài này, chúng ta sẽ học về các cấu hình bổ sung cho thuộc tính, và trong bài tiếp theo là cách để biến chúng thành các hàm getter và setter.

## Các "cờ" của thuộc tính

Các thuộc tính, ngoài giá trị `value` còn có ba đặc tính đặc biệt khác gọi là ba "cờ" hay "flag", đó là:

- **`writable`** -- nếu là `true`, thì giá trị chuộc tính có thể thay đổi, nếu không nó chỉ có thể đọc (read-only).
- **`enumerable`** -- nếu là `true`, thì thuộc tính được liệt kê trong các vòng lặp.
- **`configurable`** -- nếu là `true`, thì có thể xóa thuộc tính và sửa lại cờ của nó.

Chúng ta chưa từng thấy các cờ này, bởi thông thường chúng không hiện ra. Khi chúng ta tạo một thuộc tính theo cách thông thường, tất cả các cờ đều là `true`. Nhưng chúng ta cũng có thể thay đổi chúng bất cứ lúc nào.

Trước tiên, cùng xem cách để lấy các cờ trên:

Phương thức [Object.getOwnPropertyDescriptor](mdn:js/Object/getOwnPropertyDescriptor) cho phép lấy *toàn bộ* thông tin của một thuộc tính (gồm các cờ và giá trị).

Cú pháp là:
```js
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

`obj`
: Là đối tượng chứa thuộc tính.

`propertyName`
: Là tên thuộc tính.

Giá trị trả về là một đối tượng chứa giá trị và tất cả các cờ của thuộc tính. Đối tượng này còn được gọi là "property desciptor" (bộ mô tả thuộc tính) hay ngắn gọn là **descriptor**.

Ví dụ:

```js run
let user = {
  name: "Hùng"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* property descriptor:
{
  "value": "Hùng",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

Để thay đổi descriptor, chúng ta sử dụng [Object.defineProperty](mdn:js/Object/defineProperty).

Cú pháp là:

```js
Object.defineProperty(obj, propertyName, descriptor)
```

`obj`, `propertyName`
<<<<<<< HEAD
: Đối tượng và tên thuộc tính cần thay đổi.
=======
: The object and its property to apply the descriptor.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

`descriptor`
: Đối tượng "descriptor" sử dụng.

Nếu thuộc tính đã tồn tại, `defineProperty` cập nhật lại descriptor của nó. Nếu không, thuộc tính được tạo với giá trị và cờ được cung cấp bởi `descriptor`. Khi thuộc tính được tạo theo cách này, các cờ không được cung cấp sẽ nhận giá trị mặc định là `false`.

Ví dụ, thuộc tính `name` được tạo với tất cả các cờ là `false`:

```js run
let user = {};

*!*
Object.defineProperty(user, "name", {
  value: "Hùng"
});
*/!*

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": "Hùng",
*!*
  "writable": false,
  "enumerable": false,
  "configurable": false
*/!*
}
 */
```

So sánh nó với cách tạo `user.name` thông thường ở ví dụ trước: tất cả các cờ đều `false`. Nếu muốn các thuộc tính được tạo như bình thường chúng ta nên cài đặt tất cả cờ là `true` trong `descriptor`.

Bây giờ, cùng xem các cờ có tác dụng gì bởi các ví dụ.

## Thuộc tính "chỉ đọc"

Để tạo thuộc tính chỉ đọc ta đặt cờ `writable` là `false`:

```js run
let user = {
  name: "Hùng"
};

Object.defineProperty(user, "name", {
*!*
  writable: false
*/!*
});

*!*
<<<<<<< HEAD
user.name = "Mạnh"; // Lỗi: Không thể gán cho thuộc tính chỉ đọc 'name'...
=======
user.name = "Pete"; // Error: Cannot assign to read only property 'name'
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a
*/!*
```

Lúc này không thể thay đổi giá trị của thuộc tính nữa trừ khi đặt lại `writable` thành `true` bằng `defineProperty` một lần nữa.

<<<<<<< HEAD
```smart header="Lỗi chỉ xuất hiện trong chế độ nghiêm ngặt"
Nếu không sử dụng chế độ nghiệm ngặt (`"use strict"`) sẽ không có lỗi xảy ra khi thay đổi giá trị của thuộc tính chỉ đọc. Nhưng hành động này vẫn không thể thực hiện thành công.
=======
```smart header="Errors appear only in strict mode"
In the non-strict mode, no errors occur when writing to read-only properties and such. But the operation still won't succeed. Flag-violating actions are just silently ignored in non-strict.
>>>>>>> be342e50e3a3140014b508437afd940cd0439ab7
```

<<<<<<< HEAD
Cũng như trên, nhưng đây là trường hợp thuộc tính không tồn tại:
=======
Here's the same example, but the property is created from scratch:
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

```js run
let user = { };

Object.defineProperty(user, "name", {
*!*
<<<<<<< HEAD
  value: "Hùng",
  // đặt các cờ khác là true
=======
  value: "John",
  // for new properties need to explicitly list what's true
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a
  enumerable: true,
  configurable: true
*/!*
});

<<<<<<< HEAD
alert(user.name); // Hùng
user.name = "Ngọc"; // Lỗi
```


## Thuộc tính "không liệt kê"
=======
alert(user.name); // John
user.name = "Pete"; // Error
```

## Non-enumerable
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Giờ thay đổi phương thức `toString` trong `user`.

Bình thường, thuộc tính `toString` có sẵn của các đối tượng là thuộc tính không liệt kê và không xuất hiện trong vòng lặp `for..in`. Nhưng nếu ta tự tạo phương thức `toString`, thì mặc định nó lại xuất hiện trong `for..in` như minh họa sau:

```js run
let user = {
  name: "Hùng",
  toString() {
    return this.name;
  }
};

// Mặc định, cả hai thuộc tính đều được liệt kê
for (let key in user) alert(key); // name, toString
```

Nếu không muốn một thuộc tính nào đó được liệt kê, chúng ta đặt cờ `enumerable` là `false`. Sau đó nó không xuất hiện trong vòng lặp `for..in` nữa, giống hệt như phương thức `toString` có sẵn:

```js run
let user = {
  name: "Hùng",
  toString() {
    return this.name;
  }
};

Object.defineProperty(user, "toString", {
*!*
  enumerable: false
*/!*
});

*!*
// Giờ toString biến mất
*/!*
for (let key in user) alert(key); // name
```

Các thuộc tính không liệt kê cũng bị loại khỏi `Object.keys`:

```js
alert(Object.keys(user)); // name
```

## Thuộc tính "không thể cấu hình"

Cờ không thể cấu hình (`configurable:false`) thường được dùng cho các đối tượng và thuộc tính có sẵn.

Một thuộc tính *không thể cấu hình* sẽ không thể xóa được và không thể thay đổi cờ được.

Ví dụ, `Math.PI` là thuộc tính chỉ đọc, không thể cấu hình, không liệt kê:

```js run
let descriptor = Object.getOwnPropertyDescriptor(Math, 'PI');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": 3.141592653589793,
  "writable": false,
  "enumerable": false,
  "configurable": false
}
*/
```
Cho nên lập trình viên không thể xóa cũng như thay đổi giá trị `Math.PI`, cũng không có cách nào khiến nó xuất hiện trong vòng lặp:

```js run
Math.PI = 3; // Lỗi

// delete Math.PI cũng không làm việc
```

Một khi làm cho một thuộc tính thành không thể cấu hình, không có cách nào khôi phục nữa bởi `defineProperty` không làm việc với chúng.

 Ví dụ sau, chúng ta khiến cho `user.name` thành một hằng bị "niêm phong vĩnh viễn":

```js run
let user = { };

Object.defineProperty(user, "name", {
  value: "Hùng",
  writable: false,
  configurable: false
});

*!*
// không thể thay đổi user.name và cờ của nó
// tất cả đều không làm việc:
//   user.name = "Mạnh"
//   delete user.name
//   defineProperty(user, "name", ...)
Object.defineProperty(user, "name", {writable: true}); // Lỗi
*/!*
```

## Object.defineProperties

Phương thức [Object.defineProperties(obj, descriptors)](mdn:js/Object/defineProperties) cho phép định nghĩa descriptor cho hàng loạt thuộc tính cùng lúc.

Cú pháp là:

```js
Object.defineProperties(obj, {
  prop1: descriptor1,
  prop2: descriptor2
  // ...
});
```

Ví dụ:

```js
Object.defineProperties(user, {
  name: { value: "Hùng", writable: false },
  surname: { value: "Phùng", writable: false },
  // ...
});
```

Nghĩa là ta có thể cài đặt "property descriptor" cho nhiều thuộc tính cùng lúc.

## Object.getOwnPropertyDescriptors

Phương thức [Object.getOwnPropertyDescriptors(obj)](mdn:js/Object/getOwnPropertyDescriptors) cho phép lấy tất cả các descriptor của tất cả các thuộc tính.

Kết hợp với `Object.defineProperties` ta có thể nhân bản đối tượng bao gồm cả các cờ:

```js
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```

Thường khi nhân bản một đối tượng, chúng ta sử dụng lệnh gán để sao chép các thuộc tính:

```js
for (let key in user) {
  clone[key] = user[key]
}
```

...Nhưng nó không sao chép được cờ. Vậy nên muốn nhân bản "tốt hơn" thì nên dùng `Object.defineProperties`.

Một khác biệt nữa là `for..in` bỏ qua các thuộc tính symbol, nhưng `Object.getOwnPropertyDescriptors` trả về tất cả descriptor của mọi thuộc tính kể cả symbol.

## Niêm phong đối tượng

Sử dụng property descriptors chỉ giúp ta niêm phong từng thuộc tính.

Có một số phương thức giúp ta thực hiện trên toàn đối tượng:

[Object.preventExtensions(obj)](mdn:js/Object/preventExtensions)
: Cấm thêm các thuộc tính mới cho đối tượng.

[Object.seal(obj)](mdn:js/Object/seal)
: Cấm thêm/xóa các thuộc tính. Đặt `configurable: false` cho mọi thuộc tính.

[Object.freeze(obj)](mdn:js/Object/freeze)
: Cấm thêm/xóa/sửa các thuộc tính. Đặt `configurable: false, writable: false` cho mọi thuộc tính.
Và cũng có các phương thức kiểm tra:

[Object.isExtensible(obj)](mdn:js/Object/isExtensible)
: Trả về `false` nếu không thể thêm thuộc tính cho đối tượng, nếu không trả về `true`.

[Object.isSealed(obj)](mdn:js/Object/isSealed)
: Trả về `true` nếu không thể thêm/xóa thuộc tính, và mọi thuộc tính hiện có đều có cờ `configurable: false`.

[Object.isFrozen(obj)](mdn:js/Object/isFrozen)
: Trả về `true` nếu không thể thêm/xóa/sửa thuộc tính, và tất cả các thuộc tính hiện có đều có cờ `configurable: false, writable: false`.

Các phương thức này rất ít được sử dụng trong thực tế.
