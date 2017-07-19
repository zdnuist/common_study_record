##### Object.freeze 函数
阻止修改现有属性的特性和值，并阻止添加新属性  
Object.freeze 函数执行下面的操作：
使对象不可扩展，这样便无法向其添加新属性。
为对象的所有属性将 configurable 特性设置为 false。在 configurable 为 false 时，无法更改属性的特性且无法删除属性。
为对象的所有数据属性将 writable 特性设置为 false。当 writable 为 false 时，无法更改数据属性值  
