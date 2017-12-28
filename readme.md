## react vue 生命周期函数(可以认为是事件)


###一、初始化后dom未渲染

`vue -> created()`

在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。

`react -> constructor(props, context)`

构造函数，在创建组件的时候调用一次。

*实际是类初始化*

### 二、dom准备渲染

`vue -> beforeCreate()`

在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。

`react -> void componentWillMount()`

在组件挂载之前调用一次。如果在这个函数里面调用setState，本次的render函数可以看到更新后的state，并且只渲染一次。

### 三、beforeMount

在挂载开始之前被调用：相关的 render 函数首次被调用。


### 四、dom渲染后

`vue -> mounted`

el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。

注意 mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick 替换掉 mounted

`react -> void componentDidMount()`

在组件挂载之后调用一次。这个时候，子主键也都挂载好了，可以在这里使用refs。

### 五、数据变更dom渲染之前

`vue -> beforeUpdate`

数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。

你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

该钩子在服务器端渲染期间不被调用。

`react -> void componentWillUpdate(nextProps, nextState)`

shouldComponentUpdate返回true或者调用forceUpdate之后，componentWillUpdate会被调用。

### 六、数据变更dom渲染

`vue -> updated`

由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。

`react -> void componentDidUpdate()`

除了首次render之后调用componentDidMount，其它render结束之后都是调用componentDidUpdate。

***componentWillMount、componentDidMount和componentWillUpdate、componentDidUpdate可以对应起来。区别在于，前者只有在挂载的时候会被调用；而后者在以后的每次更新渲染之后都会被调用。***

### 七、vue -> activated

keep-alive 组件激活时调用。

### 八、vue -> deactivated

keep-alive 组件停用时调用。

### 九、vue -> beforeDestroy

实例销毁之前调用。在这一步，实例仍然完全可用。

### 十、vue -> destroyed

Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

### 十一、vue -> errorCaptured

当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上传播。

### 十二、react -> void componentWillReceiveProps(nextProps)

props是父组件传递给子组件的。父组件发生render的时候子组件就会调用componentWillReceiveProps（不管props有没有更新，也不管父子组件之间有没有数据交换）。

### 十三、react -> bool shouldComponentUpdate(nextProps, nextState)

组件挂载之后，每次调用setState后都会调用shouldComponentUpdate判断是否需要重新渲染组件。默认返回true，需要重新render。在比较复杂的应用里，有一些数据的改变并不影响界面展示，可以在这里做判断，优化渲染效率。

### 十四、react -> void componentWillUnmount()

组件被卸载的时候调用。一般在componentDidMount里面注册的事件需要在这里删除。