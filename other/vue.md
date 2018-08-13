# vue表单组件开发(不使用vuex)

* 表单元素控制ui层面的变化及基础交互，当父组件需要通知子组件修改状态时，可传入props（子组件在created阶段复制props值至state），子组件通过监听props变化改变自身状态，而不是直接使用父组件props作为state。直接使用父组件props将使子组件不能控制自身状态，失去封装的特性。
* 子组件状态的改变可通过事件告知父组件并传入参数。
* 表单父组件可通过对象存储每个表单的信息、状态，甚至可以存储校验方法，从而实现Form对所有表单元素的控制。

最终结果：子组件能改变自身状态，父组件也可通过props（子组件监听该props）改变子组件状态。