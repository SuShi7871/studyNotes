# vue

## 1.vue核心

### 1.初识vue

#### Vue模板语法：

​	1.插值语法：
​		功能：用于解析标签体内容。
​		写法：**{{xxx}}**，xxx是js表达式，且可以直接读取到data中的所有属性。
​	2.指令语法：
​		功能：用于解析标签（包括：标签属性、标签体内容、绑定事件.....）。
​		举例：**v-bind:href="xxx"** 或 简写为 **:href="xxx"**，xxx同样要写js表达式，且可以直接读取到**data中的所有属性**。

```js
<div id="root">
    <h1>hello{{name.toUpperCase()}},{{address}}</h1>
	 <a v-bind:href="url">百度</a>
	//简写形式
	//<a :href="url">百度</a>
</div>
<script>
    new Vue({
        el: '#root',
        data: {
            name: 'vue',
            address: '北京',
            url: 'http://www.baidu.com'
        }
    })
</script>
```

#### 数据绑定

Vue中有2种数据绑定的方式：
	1.单向绑定(**v-bind**)：数据只能从data流向页面。
	2.双向绑定(**v-model**)：数据不仅能从data流向页面，还可以从页面流向data。
备注：
	1.双向绑定一般都应用在表单类元素上（如：input、select等）
	2.**v-model:value** 可以简写为 v-model，因为v-model默认收集的就是value值。

```vue
<div id="model">
        单项数据绑定: <input class="one" type="text" v-bind:vaule="name">
        <br>
    	<!--普通写法-->
        <!-- 双向数据绑定: <input class="two" type="text" v-model:value="age"> -->
   		<!--简易写法-->
        双向数据绑定: <input class="two" type="text" v-model="age">
</div>
<script>
    new Vue({
        el: "#model",
        data: {
            name: '张三',
            age: 223
        }
    })
</script>
```

#### el与data的两种写法

el有2种写法
	 	.new Vue时候配置el属性。
		.先创建Vue实例，随后再通过**v.$mount('#root')**指定el的值。
data有2种写法
	(1).对象式
	(2).函数式
	如何选择：目前哪种写法都可以，以后学习到组件时，data必须使用函数式，否则会报错。
一个重要的原则：
	由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了。

```js
//el的第二种写法
    v.$mount('#model');
    //data的两种写法
    new Vue({
      el:'#root',
      //data的第一种写法：对象式
      /* data:{
        name:'尚硅谷
      } */
      //data的第二种写法：函数式
      data(){
        console.log('@@@',this) //此处的this是Vue实例对象
        return{
          name:'尚硅谷'
        }
      }
})
```

#### mvvm模型

MVVM模型

- M：模型(**Model**) ：**data中的数据**
- V：视图(**View**) ：**模板代码**
- VM：视图模型(**ViewModel**)：**Vue实例**
  观察发现：

- data中所有的属性，最后都出现在了vm身上。
- vm身上所有的属性及Vue原型上所有属性，在Vue模板中都可以直接使用。

```js
<div class="demo1">
        <h1>{{name}}</h1>
        <h1>{{address}}</h1>
</div>
<script>
    const mv = new Vue({
        el: '.demo1',
        data: {
            name: 'tom',
            address: 'America'
        }
    })
console.log(mv);//此处打印的是vue实例对象相关的内容信息
</script>
```

#### 数据代理

1.Vue中的数据代理：
	通过vm对象来代理data对象中属性的操作（读/写）
2.Vue中数据代理的好处：
	更加方便的操作data中的数据
3.基本原理：
	通过**Object.defineProperty()**把data对象中所有属性添加到vm上。为每一个添加到vm上的属性，都指定一个**getter/setter**。在getter/setter内部去操作（读/写）data中对应的属性。

```vue
<!-- 数据代理 -->
  <script>
        let number = 19
        let person = {
            name: '张三丰',
            sex: '男'
        }
        Object.defineProperty(person, 'age', {
            // value:18,
			// enumerable:true, //控制属性是否可以枚举，默认值是false
			// writable:true, //控制属性是否可以被修改，默认值是false
			// configurable:true //控制属性是否可以被删除，默认值是false
			//当有人读取person的age属性时，get函数(getter)就会被调用，且返回值就是age的值
			get(){
					console.log('有人读取age属性了')
					return number
			},
			//当有人修改person的age属性时，set函数(setter)就会被调用，且会收到修改的具体值
			set(value){
					console.log('有人修改了age属性，且值是',value)
					number = value
			}
        })
        console.log(person)
   </script>
```

如下写法获取vue对象中的数据，**vm._data.name**与**{{name}}**效果一样，都可获取到相应的数据

```vue
<div id="root">
        {{name}}
        <br>
        {{address}}
 </div>
<script>
		const vm = new Vue({
            el: '#root',
            data: {
                name: '京东',
                address: '北京'
            }
        })
        console.log(vm._data.name);
        console.log(vm._data.address)
</script>
```

#### 事件处理

事件的基本使用

- 使用**v-on:xxx** 或 **@xxx** 绑定事件，其中xxx是事件名；
- 事件的回调需要配置在**methods对象**中，最终会在vm上；
- methods中配置的函数，**不要用箭头函数！否则this就不是vm了**；
- methods中配置的函数，都是被Vue所管理的函数，this的指向是vm或组件实例对象；
- **@click="demo"** 和 **@click="demo($event)"** 效果一致，但后者可以传参；

```vue
<div id="root">
		<h2>欢迎来到{{name}}学习</h2>		
<!-- <button v-on:click="showInfo">点我提示信息</button> -->
		<button @click="showInfo1">点我提示信息1（不传参）</button>
		<button @click="showInfo2($event,66)">点我提示信息2（传参）</button>
</div>
<script type="text/javascript">
	Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
	const vm = new Vue({
		el:'#root',
		data:{
			name:'尚硅谷',
		},
		methods:{
			showInfo1(event){
				// console.log(event.target.innerText)
				// console.log(this) //此处的this是vm
				alert('同学你好！')
			},
			showInfo2(event,number){
				console.log(event,number)
				// console.log(event.target.innerText)
				// console.log(this) //此处的this是vm
				alert('同学你好！！')
			}
		}
	})
</script>
```

#### 事件修饰符

Vue中的事件修饰符：

- prevent：阻止默认事件（常用）；
- stop：阻止事件冒泡（常用）；
- once：事件只触发一次（常用）；
- capture：使用事件的捕获模式；
- self：只有event.target是当前操作的元素时才触发事件；
- passive：事件的默认行为立即执行，无需等待事件回调执行完毕；


```vue
 <div id="root">
        <!-- 在事件后面加上prevent可以阻止默认事件，此处会阻止页面进行跳转 -->
        <a href="http://www.baidu.com" @click.prevent="showInfo">点我提示信息</a>
        <!-- 阻止事件冒泡 
            在事件后面加上stop可以阻止事件冒泡，此处页面的提示信息只会显示一次
            注意：同时加上两个不同的修饰符，会同时产生作用，此处@click.stop.prevent会同时
            产生阻止冒泡和默认事件的效果
        -->
        <div class="demo" @click="showInfo">
            <a href="http://www.baidu.com" @click.stop="showInfo">阻止冒泡测试</a>
            <a href="http://www.baidu.com" @click.stop.prevent="showInfo">阻止冒泡和默认事件测试</a>
        </div>
</div>
<!-- 事件只触发一次（常用）此处只有在第一次点击按钮的时候才会有效果-->
<button @click.once="showInfo">点我提示信息</button>
<script>
    new Vue({
        el: '#root',
        methods: {
            showInfo() {
                alert('同学你好')
            }
        }
    })
</script>
```

#### 键盘事件

Vue中常用的按键别名：

```markdown
回车 => enter
删除 => delete (捕获“删除”和“退格”键)
退出 => esc
空格 => space
换行 => tab (特殊，必须配合keydown去使用)
上 => up
下 => down
左 => left
右 => right
```

2.Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为**kebab-case**（短横线命名）

3.系统修饰键（用法特殊）：ctrl、alt、shift、meta
	(1).配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发。
	(2).配合keydown使用：正常触发事件。

4.也可以使用keyCode去指定具体的按键（不推荐）

5.Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名

#### 计算属性

​	定义：要用的属性不存在，要通过已有属性计算得来。
​	原理：底层借助了Objcet.defineproperty方法提供的getter和setter。
​	get函数什么时候执行？
​		(1).初次读取时会执行一次。
​		(2).当依赖的数据发生改变时会被再次调用。
​	优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便。
​	备注：
​		1.计算属性最终会出现在vm上，直接读取使用即可。
​		2.如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变。

```vue
<div id="root">		
		姓：<input type="text" v-model="firstName"> <br/><br/>
		名：<input type="text" v-model="lastName"> <br/><br/>
		测试：<input type="text" v-model="x"> <br/><br/>
		全名：<span>{{fullName}}</span> <br/><br/>
</div>
<script>
const vm = new Vue({
			el:'#root',
			data:{
				firstName:'张',
				lastName:'三',
				x:'你好'
			},
			methods: {
				demo(){}
			},
			computed:{
				fullName:{
					//get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
					//get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时。
					get(){
						console.log('get被调用了')
						// console.log(this) //此处的this是vm
						return this.firstName + '-' + this.lastName
					},
					//set什么时候调用? 当fullName被修改时。
					set(value){
						console.log('set',value)
						const arr = value.split('-')
						this.firstName = arr[0]
						this.lastName = arr[1]
					}
				},
                //简写,计算属性一般只需要进行页面展示，不需要修改其中的数据，所以可以使用简易写法
				fullName(){
					console.log('get被调用了')
					return this.firstName + '-' + this.lastName
				}
			}
		})
</script>
```

#### 监视属性

**Vue 3 Snippets**这是用于自动补全vue代码的插件

1.当被监视的属性变化时, 回调函数自动调用, 进行相关操作
2.监视的属性必须存在，才能进行监视
3.监视的两种写法：
	new Vue时传入watch配置
	通过vm.$watch监视

```js
<div id="#demo1">	
<h2>今天天气很{{info}}</h2>
		<button @click="changeWeather">切换天气</button>
	</div>
<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		const vm = new Vue({
			el:'#root',
			data:{
				isHot:true,
			},
			computed:{
				info(){
					return this.isHot ? '炎热' : '凉爽'
				}
			},
			methods: {
				changeWeather(){
					this.isHot = !this.isHot
				}
			},
            //监视属性实现
			watch:{
				isHot:{
					immediate:true, //初始化时让handler调用一下
					//handler什么时候调用？当isHot发生改变时。
					handler(newValue,oldValue){
						console.log('isHot被修改了',newValue,oldValue)
					}
				}
			}
		})
		vm.$watch('isHot',{
			immediate:true, //初始化时让handler调用一下
			//handler什么时候调用？当isHot发生改变时。
			handler(newValue,oldValue){
				console.log('isHot被修改了',newValue,oldValue)
			}
		})
	</script>
```

##### 深度监视

​	(1).Vue中的**watch**默认不监测对象内部值的改变（一层）。
​	(2).配置**deep:true**可以监测对象内部值改变（多层）。
​	备注：
​	(1).Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以！
​	(2).使用watch时根据数据的具体结构，决定是否采用深度监视。

```vue
<script>	
watch:{
		isHot:{
			// immediate:true, //初始化时让handler调用一下
			//handler什么时候调用？当isHot发生改变时。
			handler(newValue,oldValue){
				console.log('isHot被修改了',newValue,oldValue)
				}
			},
			//监视多级结构中某个属性的变化
			/* 'numbers.a':{
				handler(){
				console.log('a被改变了')
				}
			} */
			//监视多级结构中所有属性的变化
			numbers:{
                //配置了deep属性之后就可以实现深度监视的效果
				deep:true,
				handler(){
					console.log('numbers改变了')
				}
			},
            //监视属性的简写形式
			isHot(newValue,oldValue){
					console.log('isHot被修改了',newValue,oldValue,this)
			}
	}
 </script>   
```

计算属性和监视属性的区别

computed和watch之间的区别：
	computed能完成的功能，watch都可以完成。
	watch能完成的功能，computed不一定能完成，

​	例如：**watch可以进行异步操作**。
两个重要的小原则：
​	**所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象。**
​	**所有不被Vue所管理的函数(定时器的回调函数、ajax的回调函数等、Promise的回调函数)，最好写成箭头函数，这样this的指向才是vm或组件实例对象**。

#### 绑定样式

1. class样式
	写法:**class="xxx" xxx**可以是字符串、对象、数组。
		字符串写法适用于：类名不确定，要动态获取。
		对象写法适用于：要绑定多个样式，个数不确定，名字也不确定。
		数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。
2. style样式
	**:style="{fontSize: xxx}"**其中xxx是动态值。
	**:style="[a,b]"**其中a、b是样式对象。

```vue
<div id="root">		
		<!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
		<div class="basic" :class="mood" @click="changeMood">{{name}}</div> <br/><br/>		
		<!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
		<div class="basic" :class="classArr">{{name}}</div> <br/><br/>
		<!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
		<div class="basic" :class="classObj">{{name}}</div> <br/><br/>
		<!-- 绑定style样式--对象写法 -->
		<div class="basic" :style="styleObj">{{name}}</div> <br/><br/>
		<!-- 绑定style样式--数组写法 -->
		<div class="basic" :style="styleArr">{{name}}</div>
	</div>
<script type="text/javascript">
		Vue.config.productionTip = false	
		const vm = new Vue({
			el:'#root',
			data:{
				name:'尚硅谷',
				mood:'normal',
				classArr:['atguigu1','atguigu2','atguigu3'],
				classObj:{
					atguigu1:false,
					atguigu2:false,
				},
				styleObj:{
					fontSize: '40px',
					color:'red',
				},
				styleObj2:{
					backgroundColor:'orange'
				},
				styleArr:[
					{
						fontSize: '40px',
						color:'blue',
					},
					{
						backgroundColor:'gray'
					}
				]
			},
			methods: {
				changeMood(){
					const arr = ['happy','sad','normal']
					const index = Math.floor(Math.random()*3)
					this.mood = arr[index]
				}
			},
		})
	</script>
```

#### 条件渲染

1.v-if
		写法：
				(1).**v-if="表达式"** 
				(2).**v-else-if="表达式"**
				(3).**v-else="表达式"**
		适用于：切换频率较低的场景。
		特点：不展示的DOM元素直接被移除。
		注意：v-if可以和:v-else-if、v-else一起使用，但要求结构不能被“打断”。
2.v-show
		写法：v-show="表达式"
		适用于：切换频率较高的场景。
		特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉
备注：**使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到。**

v-for指令:
	1.用于展示列表数据
	2.语法：**v-for="(item, index) in xxx" :key="yyy"**
	3.可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）

```vue
 		<!-- 遍历数组 -->
        <ul>
            <li>姓名--年龄--性别</li>
            <li v-for="(person ,index) in persons" :key="index">
                {{person.name}}--{{person.age}}--{{person.gender}}
            </li>
        </ul>
        <!-- 遍历对象 -->
        <ul>
            <li v-for="(value,k) of cars" :key="k">
                {{k}}-{{value}}
            </li>
        </ul>
<script>
new Vue({
				el:'#root',
				data:{
					persons:[
						{id:'001',name:'张三',age:18},
						{id:'002',name:'李四',age:19},
						{id:'003',name:'王五',age:20}
					],
					car:{
						name:'奥迪A8',
						price:'70万',
						color:'黑色'
					},
					str:'hello'
				}
})
</script>
```

#### 数据监视

Vue监视数据的原理：
 	1. vue会监视data中所有层次的数据。
 	2. 如何监测对象中的数据？
     通过setter实现监视，且要在new Vue时就传入要监测的数据。
     	(1).对象中后追加的属性，Vue默认不做响应式处理
     	(2).如需给后添加的属性做响应式，请使用如下API：
     		**Vue.set(target，propertyName/index，value)** 或 
     		**vm.$set(target，propertyName/index，value)**
 	3. 如何监测数组中的数据？
     通过包裹数组更新元素的方法实现，本质就是做了两件事：
     	(1).调用原生对应的方法对数组进行更新。
     	(2).重新解析模板，进而更新页面。

4.在Vue修改数组中的某个元素一定要用如下方法：
	1.使用这些API:**push()、pop()、shift()、unshift()、splice()、sort()、reverse()**
	2.**Vue.set()** 或 **vm.$set()**
特别注意：**Vue.set()** 和 vm.$set() 不能给vm或vm的根数据对象 添加属性

```vue
<div>			
		<h1>学生信息</h1>
		<button @click="student.age++">年龄+1岁</button> <br/>
		<button @click="addSex">添加性别属性，默认值：男</button> <br/>
		<button @click="student.sex = '未知' ">修改性别</button> <br/>
		<button @click="addFriend">在列表首位添加一个朋友</button> <br/>
		<button @click="updateFirstFriendName">修改第一个朋友的名字为：张三</button> <br/>
		<button @click="addHobby">添加一个爱好</button> <br/>
		<button @click="updateHobby">修改第一个爱好为：开车</button> <br/>
		<button @click="removeSmoke">过滤掉爱好中的抽烟</button> <br/>
		<h3>姓名：{{student.name}}</h3>
		<h3>年龄：{{student.age}}</h3>
		<h3 v-if="student.sex">性别：{{student.sex}}</h3>
		<h3>爱好：</h3>
		<ul>
			<li v-for="(h,index) in student.hobby" :key="index">
				{{h}}
			</li>
		</ul>
		<h3>朋友们：</h3>
		<ul>
			<li v-for="(f,index) in student.friends" :key="index">
				{{f.name}}--{{f.age}}
			</li>
		</ul>
</div>
<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		const vm = new Vue({
			el:'#root',
			data:{
				student:{
					name:'tom',
					age:18,
					hobby:['抽烟','喝酒','烫头'],
					friends:[
						{name:'jerry',age:35},
						{name:'tony',age:36}
					]
				}
			},
			methods: {
				addSex(){
					// Vue.set(this.student,'sex','男')
					this.$set(this.student,'sex','男')
				},
				addFriend(){
					this.student.friends.unshift({name:'jack',age:70})
				},
				updateFirstFriendName(){
					this.student.friends[0].name = '张三'
				},
				addHobby(){
					this.student.hobby.push('学习')
				},
				updateHobby(){
					// this.student.hobby.splice(0,1,'开车')
					// Vue.set(this.student.hobby,0,'开车')
					this.$set(this.student.hobby,0,'开车')
				},
				removeSmoke(){
					this.student.hobby = this.student.hobby.filter((h)=>{
						return h !== '抽烟'
					})
				}
			}
		})
	</script>
```

#### 收集表单数据

​	若：<input type="text"/>，则v-model收集的是value值，用户输入的就是value值。
​	若：<input type="radio"/>，则v-model收集的是value值，且要给标签配置value值。
​	若：<input type="checkbox"/>
​		1.没有配置input的value属性，那么收集的就是**checked**（勾选 or 未勾选，是布尔值）
​		2.配置input的value属性:
​				(1)v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
​				(2)v-model的初始值是数组，那么收集的的就是value组成的**数组**
​	备注：v-model的三个修饰符：
​		**lazy**：失去焦点再收集数据
​		**number**：输入字符串转为有效的数字
​		**trim**：输入首尾空格过滤

```vue
<form @submit.prevent="demo">
    //trim的作用是去除空格
    账号：<input type="text" v-model.trim="userInfo.account"> <br/><br/>
    //number规定提交的数据格式为number    
    年龄：<input type="number" v-model.number="userInfo.age"> <br/><br/>
    //单选框必须得规定一下统一的name属性，否则无法达到单选的效果
    性别：
		男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
		女<input type="radio" name="sex" v-model="userInfo.sex" value="female"> <br/><br/>
    //复选框和单选框类似，也需要给name绑定相同的属性，否则也无法在vue实例中获取到
	爱好：
		学习<input type="checkbox" v-model="userInfo.hobby" value="study">
		打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
		吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
		<br/><br/>
    //选择器的绑定事件需要加在select标签上
	所属校区
		<select v-model="userInfo.city">
			<option value="">请选择校区</option>
			<option value="beijing">北京</option>
			<option value="shanghai">上海</option>
			<option value="shenzhen">深圳</option>
			<option value="wuhan">武汉</option>
		</select>
</from>  
<script>
	new Vue({
			el:'#root',
			data:{
				userInfo:{
					account:'',
					password:'',
					age:18,
					sex:'female',
                    //这种爱好类似的数据形式，需要使用数据来包裹，然后打印出来
					hobby:[],
					city:'beijing',
					other:'',
					agree:''
				}
			},
			methods: {
				demo(){
                    //将数据用json字符串的形式返回
					console.log(JSON.stringify(this.userInfo))
				}
			}
	})
</script>
```

#### 过滤器

​	定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。
​	语法：
​		1.注册过滤器：**Vue.filter(name,callback)** 或 **new Vue{filters:{}}**
​		2.使用过滤器：**{{ xxx | 过滤器名}}**  或  **v-bind:属性 = "xxx | 过滤器名"**
​	备注：
​		1.过滤器也可以接收额外参数、多个过滤器也可以串联
​		2.并没有改变原本的数据, 是产生新的对应的数据

#### 内置指令

##### v-text

​		1.作用：向其所在的节点中渲染文本内容。
​		2.与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会。

```vue
<h2 v-text="name"></h2>
<h2 v-text="str"></h2>
data: {
        name: 'james',
        str: "<h2>你好啊</h2>"
}
```

##### v-html

​	1.作用：向指定节点中渲染包含html结构的内容。
​	2.与插值语法的区别：
​		(1).**v-html**会替换掉节点中所有的内容，**{{xx}}**则不会。
​		(2).**v-html**可以识别html结构。
​	3.严重注意：**v-html有安全性问题！！！！**
​		(1).在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击。
​		(2).一定要在可信的内容上使用v-html，永不要用在用户提交的内容上！

```vue
<h2 v-html=“str”></h2> //可以解析标签结构
```

##### v-cloak指令

​	1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。
​	2.使用css配合v-cloak可以解决**网速慢时页面展示出{{xxx}}的问题**。

##### v-once指令

​	1.**v-once**所在节点在初次动态渲染后，就视为静态内容了。
​	2.以后数据的改变不会引起**v-once**所在结构的更新，可以用于优化性能。

```vue
<h2 v-once>初始化的n值是:{{n}}</h2>		
<h2>当前的n值是:{{n}}</h2>
<button @click="n++">点我n+1</button>
```

##### v-pre指令

​	1.跳过其所在节点的编译过程。
​	2.可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译。

```vue
<h2 v-pre>Vue其实很简单</h2>
<h2 >当前的n值是:{{n}}</h2>
<button @click="n++">点我n+1</button>
```

#### 自定义指令

​	一、定义语法：
​		(1).局部指令：

```vue
<script>new Vue({															
	directives:{指令名:配置对象}   
})	
//或 
new Vue({
	directives{指令名:回调函数}
})
</script>
```

​			(2).全局指令：
​			**Vue.directive(指令名,配置对象)**或**Vue.directive(指令名,回调函数)**
​	二、配置对象中常用的3个回调：

- **bind**：指令与元素成功绑定时调用。
- **inserted**：指令所在元素被插入页面时调用。
- **update**：指令所在模板结构被重新解析时调用。

​	三、备注：

- 指令定义时不加v-，但使用时要加v-；
- 指令名如果是多个单词，**要使用kebab-case命名方式，不要用camelCase命名。**

```vue
<div id="root">
			<h2>{{name}}</h2>
			<h2>当前的n值是：<span v-text="n"></span> </h2>
			<!-- <h2>放大10倍后的n值是：<span v-big-number="n"></span> </h2> -->
			<h2>放大10倍后的n值是：<span v-big="n"></span> </h2>
			<button @click="n++">点我n+1</button>
			<hr/>
			<input type="text" v-fbind:value="n">
</div>
<script>
		//定义全局指令
		/* Vue.directive('fbind',{
			//指令与元素成功绑定时（一上来）
			bind(element,binding){
				element.value = binding.value
			},
			//指令所在元素被插入页面时
			inserted(element,binding){
				element.focus()
			},
			//指令所在的模板被重新解析时
			update(element,binding){
				element.value = binding.value
			}
		}) */
		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷',
				n:1
			},
			directives:{
				//big函数何时会被调用？1.指令与元素成功绑定时（一上来）。2.指令所在的模板被重新解析时。
				/* 'big-number'(element,binding){
					// console.log('big')
					element.innerText = binding.value * 10
				}, */
				big(element,binding){
					console.log('big',this) //注意此处的this是window
					// console.log('big')
					element.innerText = binding.value * 10
				},
				fbind:{
					//指令与元素成功绑定时（一上来）
					bind(element,binding){
						element.value = binding.value
					},
					//指令所在元素被插入页面时
					inserted(element,binding){
						element.focus()
					},
					//指令所在的模板被重新解析时
					update(element,binding){
						element.value = binding.value
					}
				}
			}
		})
</script>
```

#### 生命周期

常用的生命周期钩子：
		1.**mounted**: 发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】。
		2.**beforeDestroy**: 清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。
	关于销毁Vue实例
		1.销毁后借助Vue开发者工具看不到任何信息。
		2.销毁后自定义事件会失效，但原生DOM事件依然有效。
		3.一般不会在**beforeDestroy**操作数据，因为即便操作数据，也不会再触发更新流程了。

### 2.组件化

#### 1.组件的使用

Vue中使用组件的三大步骤：
	一、定义组件(创建组件)
	二、注册组件
	三、使用组件(写组件标签)

一、如何定义一个组件？
	使用**Vue.extend(options)**创建，其中**options**和**new Vue(options)**时传入的那个options几乎一样，但也有点区别；
	区别如下：
			1.el不要写，为什么？ ——— 最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器。
			2.data必须写成函数，为什么？ ———— 避免组件被复用时，数据存在引用关系。
	备注：使用**template**可以配置组件结构。

二、如何注册组件？
	1.局部注册：靠**new Vue**的时候传入components选项
	2.全局注册：靠**Vue.component('组件名',组件)**

```vue
  <div id="root">
        <hello></hello>
        <!-- 使用组件 -->
        <school></school>
        <hr>
        <student></student>

    </div>
    <div id="root2">
        <hello></hello>
    </div>
</div>
<script>
        //定义组件
        const school = Vue.extend({
            template: `
            <div>
                <h2>{{name}}</h2>  
                <h2>{{address}}</h2>  
            </div>`,
            data() {
                return {
                    name: '尚硅谷',
                    address: '北京'
                }
            }
        })
        const student = Vue.extend({
            template: `
            <div>
                <h2>{{name}}</h2>  
                <h2>{{age}}</h2>  
            </div>`,
            data() {
                return {
                    name: 'tom',
                    age: 17
                }
            }
        })
        //创建hello组件
        const hello = Vue.extend({
            template: `
				<div>	
					<h2>你好啊！{{name}}</h2>
				</div>`,
            data() {
                return {
                    name: 'Tom'
                }
            }
        })
        //注册全局组件
        Vue.component('hello', hello)
        //注册组件
        new Vue({
            el: "#root",
            components: {
                school: school,
                student: student
            }
        })
        new Vue({
            el: "#root2"
        })
    </script>
```

#### 2.注意点

1.关于组件名:
		一个单词组成：
			第一种写法(首字母小写)：school
			第二种写法(首字母大写)：School
		多个单词组成：
			第一种写法(kebab-case命名)：**my-school**
			第二种写法(CamelCase命名)：MySchool (**需要Vue脚手架支持**)
		备注：
			(1).组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行。
			(2).可以使用name配置项指定组件在开发者工具中呈现的名字。
	2.关于组件标签:
		第一种写法：<school></school>
		第二种写法：<school/>
		备注：不用使用脚手架时，**<school/>会导致后续组件不能渲染。**
	3.一个简写方式：
		**const school = Vue.extend(options) 可简写为：const school = options**

#### 3.组件嵌套

组件之间可以进行相互嵌套，

```vue
<script>
//注意，子组件必须写在父组件之前，否则，父组件无法解析子组件的内容
	const student = Vue.extend({
            template: `
            <div>
                <h2>{{name}}</h2>  
                <h2>{{age}}</h2>  
            </div>
            `,
            data() {
                return {
                    name: 'tom',
                    age: 17
                }
            }
        })
        //定义组件
        const school = Vue.extend({
            template: `
            <div>
                <h2>{{name}}</h2>  
                <h2>{{address}}</h2>
                <student></student>  
            </div>
            `,
            data() {
                return {
                    name: '尚硅谷',
                    address: '北京'
                }
            },
            //注册组件
            components: {
                student: student
            }
 		})
        new Vue({
            el: "#root",
            components: {
                school: school
            }
        })
</script>
```

另外，为了方便管理以及增强可读性，一般在项目中建一个app组件，用来存放其他组件信息，提高开发效率。

//定义app组件

```vue
    const app = Vue.extend({
	//一般来说，模板信息里面配置的都是组件标签，而不用写出具体的HTML代码，html代码可以在各自的组件模块进行编写
      template: `<div>
        <school></school>
        <hello></hello>  
      </div>`,
      components: {
        school: school,
        hello: hello
      }
    })
```

#### VueComponent

​	1.school组件本质是一个名为**VueComponent的构造函数**，且不是程序员定义的，是**Vue.extend**生成的。
​	2.我们只需要写<school/>或<school></school>，Vue解析时会帮我们创建school组件的实例对象，
​		即Vue帮我们执行的：**new VueComponent(options)**。
​	3.特别注意：每次调用Vue.extend，返回的都是一个全新的**VueComponent**
​	4.关于this指向：
​		(1).组件配置中：
​					data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【VueComponent实例对象】。
​		(2).new Vue(options)配置中：
​					**data函数、methods中的函数、watch中的函数、computed**中的函数 它们的this均是【Vue实例对象】。
​	5.VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）。
​		Vue的实例对象，以后简称vm。

### 3.Vue 脚手架

#### 1.使用脚手架

官方文档

```cmd
https://cli.vuejs.org/zh/
```

第一步（仅第一次执行）：全局安装@vue/cli。 

```cmd
npm install -g @vue/cli 
```

第二步：切换到你要创建项目的目录，然后使用命令创建项目 

```cmd
vue create xxxx 
```

第三步：启动项目 

```cmd
npm run serve
```

备注： 

- 如出现下载缓慢请配置 npm 淘宝镜像：npm config set registry https://registry.npm.taobao.org
- Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体的 webpack配置， 请执行：vue inspect > output.js

#### 2.脚手架的文件结构

```cmd
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   │── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
├── package-lock.json：包版本控制文件
```

#### 3.vue的版本问题

1. vue.js与vue.runtime.xxx.js的区别：
   1. vue.js是完整版的Vue，包含：核心功能 + 模板解析器。
   2. **vue.runtime.xxx.js是运行版的Vue**，只包含：核心功能；没有模板解析器。
2. 因为vue.runtime.xxx.js没有模板解析器，所以不能使用template这个配置项，需要使用render函数接收到的createElement函数去指定具体内容。

vue.config.js

1. 使用vue inspect > output.js可以查看到Vue脚手架的默认配置。
2. 使用vue.config.js可以对脚手架进行个性化定制，详情见：https://cli.vuejs.org/zh

#### 4.vue脚手架项目入门

一般来说，public底下的文件一般不用动，主要有图标信息和页面信息，其中在页面的一下地方用来作为页面的展示区域，其他文件只需要找到id为app的地方就可以

```vue
<div id="app">
        <!-- built files will be auto injected -->
</div>
</body>
```

src底下是我们项目功能代码主要编写的地方，其中app.vue里面配置了其他组件信息

```vue
<template>
  	<div>
  		<School/>
  		<School/>
 	</div>
</template>
<script>
//导入school组件
import School from '../src/components/School.vue'
export default{
 	name:"App",
 	components:{
  		School
 	}
}
</script>
```

components文件夹底下主要是不同类型的组件信息，可以写不同的组件


```vue
<template>
<div class="school">
    <h2>学校名称：{{name}}</h2>
    <h2>学校地址：{{address}}</h2>
</div>
</template>
<script>
    export default{
        name:'School',
        data(){
            return{
                name:"xx大学",
                address : "北京市"
            }
        }
    }
</script>
<style>
.school{
    background-color: aqua;
}
</style>
```
main.js是项目的入口文件，引入了许多不同的外部资源

```vue
<script>
//导入vue的js文件
import Vue from 'vue';
import App from './App.vue';
Vue.config.productionTip=false;
new Vue({
 	el:"#app",
 	render:h=>h(App)
})
</script>
```

#### 5.ref属性

1. 被用来给元素或子组件注册引用信息（id的替代者）
2. 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
3. 使用方式：
   1. 打标识：```<h1 ref="xxx">.....</h1>``` 或 ```<School ref="xxx"></School>```
   2. 获取：```this.$refs.xxx```

```vue
<div>
    <h2 v-text="msg" ref="title"></h2>
    <button @click="showBtn" ref="btn">点我输出上方元素</button>
    <School ref="sch"></School>
    <Student sex="男" :age="12"></Student>
  </div>
<script>
methods:{
 showBtn(){
   // console.log("123")
   console.log(this.$refs.btn);
   console.log(this.$refs.sch);
   console.log(this.$refs.title);
  }
 }
</script>
```

#### 6.props配置项

1. 功能：让组件接收外部传过来的数据

2. 传递数据：```<Demo name="xxx"/>```

3. 接收数据：

   1. 第一种方式（只接收）：```props:['name'] ```

   2. 第二种方式（限制类型）：```props:{name:String}```

   3. 第三种方式（限制类型、限制必要性、指定默认值）：

      ```js
      props:{
      	name:{
      	type:String, //类型
      	required:true, //必要性
      	default:'老王' //默认值
      	}
      }
      ```

   备注：props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据。

```vue
<h1>{{msg }}</h1>
 <h2>学生姓名:{{ name }}</h2>
  <h2>学生性别:{{ sex }}</h2>
 <h2>学生年龄:{{ myAge }}</h2>
  <button @click="addAge">点我增加年龄</button>
<script>
name:'Student',
  data(){
    return{
      msg:"我是一个学生",
      name:"赵云",
      myAge:this.age//可以给需要修改类容的属性起一个别名以达到效果
    }
  },
  // props:['sex','age'],
  props:{
    sex:String,
    age:Number
  },
  methods:{
    addAge(){
      this.myAge++
    }
  }
</script>
```

#### 7.mixin(混入)

1. 功能：可以把多个组件共用的配置提取成一个混入对象

2. 使用方式：

   第一步定义混合：

   ```vue
   {
       data(){....},
       methods:{....}
       ....
   }
   ```

   第二步使用混入：

   ​	全局混入：```Vue.mixin(xxx)```
   ​	局部混入：```mixins:['xxx']	```

#### 8.插件

1. 功能：用于增强Vue

2. 本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。

3. 定义插件：

   ```js
   对象.install = function (Vue, options) {
       // 1. 添加全局过滤器
       Vue.filter(....)
       // 2. 添加全局指令
       Vue.directive(....)
       // 3. 配置全局混入(合)
       Vue.mixin(....)
       // 4. 添加实例方法
       Vue.prototype.$myMethod = function () {...}
       Vue.prototype.$myProperty = xxxx
   }
   ```

4. 使用插件：```Vue.use()```

#### 9.组件的自定义事件

1. 一种组件间通信的方式，适用于：<strong style="color:red">子组件 ===> 父组件</strong>

2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（<span style="color:red">事件的回调在A中</span>）。

3. 绑定自定义事件：

   1. 第一种方式，在父组件中：```<Demo @atguigu="test"/>```  或 ```<Demo v-on:atguigu="test"/>```

      ```vue
      //App.vue
      <!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据（第一种写法，使用@或v-on） -->
      <Student @atguigu="getStudentName"/>
      <!--//若想让自定义事件只能触发一次，可以使用once修饰符，或$once方法。-->
      <Student @atguigu.once="getStudentName"/>
      <!--组件上也可以绑定原生DOM事件，需要使用native修饰符。-->
      <Student @click.native="getStudentName"/>
      <script>
      	export default{
                methods:{
          		getStudentName(name){
            			console.log("app收到了学生的名字",name)
          	  	}
        		  },
          }
      </script>
      ```

      ```vue
      //Student.vue
      <template>
      <div>
      	<h2>学生姓名:{{ name }}</h2>
        	<h2>学生性别:{{ sex }}</h2>
        	<button @click="addAge">点我增加年龄</button>
        	<button @click="sendStudentlName">把学生名给App</button>
          <button @click="unbind">解绑atguigu事件</button>
      </div>
      </template>
      <script>
      export default{
        methods:{
          addAge(){
            this.myAge++
          },
          sendStudentlName(){
              //触发自定义事件：this.$emit('atguigu',数据)		
            this.$emit('atguigu',this.name)
              
          },
           unbind(){
      		this.$off('atguigu') //解绑一个自定义事件
      		// this.$off(['atguigu','demo']) //解绑多个自定义事件
      		// this.$off() //解绑所有的自定义事件
      	},
        }
      }
      </script>
      ```

   2. 第二种方式，在父组件中：

      通过父组件给子组件绑定一个自定义事件实现：子给父传递数据（第二种写法，使用ref）

      ```vue
      <Student ref="student"/>
      <script>
       methods:{
        	getStudentName(name){
         			console.log("app收到了学生的名字",name)
        	}
       },
       mounted(){
        	this.$refs.student.$on('atguigu',this.getStudentName)
           //绑定自定义事件（一次性）
           //this.$refs.student.$once('atguigu',this.getStudentName) 
       }
      </script>
      ```

      注意：

      通过```this.$refs.xxx.$on('atguigu',回调)```绑定自定义事件时，回调<span style="color:red">要么配置在methods中</span>，<span style="color:red">要么用箭头函数</span>，否则this指向会出问题！

#### 10.全局事件总线（GlobalEventBus）

1. 一种组件间通信的方式，适用于<span style="color:red">任意组件间通信</span>。

2. 安装全局事件总线：

   ```vue
   new Vue({
   	......
   	beforeCreate() {
   		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
   	},
       ......
   }) 
   ```

3. 使用事件总线：

   1. 接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的<span style="color:red">回调留在A组件自身。</span>

      ```js
      methods(){
        demo(data){......}
      }
      ......
      mounted() {
        this.$bus.$on('xxxx',this.demo)
      }
      ```

   2. 提供数据：```this.$bus.$emit('xxxx',数据)```

   3. 最好在beforeDestroy钩子中，用$off去解绑<span style="color:red">当前组件所用到的</span>事件。


代码示例：

```js
//main.js
new Vue({
 	el:"#app",
 	render:h=>h(App),
 	beforeCreate(){
  		Vue.prototype.$bus=this//安装全局事件总线
 	}
})
```

```vue
//school.vue
<script>
	mounted(){
      	this.$bus.$on('hello',(data)=>{
        	console.log('我是school组件,收到了数据',data)
      	})
    },
    beforeDestroy() {
        this.$bus.$off('hello')
    },
</script>         
```

```vue
//student.vue
<button @click="sendStudentName">把学生名称给school组件</button>
<script>
export default{
    name:'Student',
    data(){
        return{
            name:"赵云"
        }
    },
    methods:{
        sendStudentName(){
            this.$bus.$emit('hello',this.name)
        }
    }
}
</script>
```

#### 11.过度与动画

### 4.Vue 中的 ajax

#### 方法一

​	在vue.config.js中添加如下配置：

```js
devServer:{
  proxy:"http://localhost:5000"
}
```

说明：

1. 优点：配置简单，请求资源时直接发给前端（8080）即可。
2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理。
3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器 （优先匹配前端资源）

#### 方法二

​	编写vue.config.js配置具体代理规则：

```js
module.exports = {
	devServer: {
      proxy: {
      '/api1': {// 匹配所有以 '/api1'开头的请求路径
        target: 'http://localhost:5000',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api1': ''}
      },
      '/api2': {// 匹配所有以 '/api2'开头的请求路径
        target: 'http://localhost:5001',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api2': ''}
      }
    }
  }
}
/*
   changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
   changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
   changeOrigin默认值为true
*/
```

说明：

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理。
2. 缺点：配置略微繁琐，请求资源时必须加前缀。

#### 插槽

1. 作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于 <strong style="color:red">父组件 ===> 子组件</strong> 。

2. 分类：默认插槽、具名插槽、作用域插槽

3. 使用方式：

   1. 默认插槽：

      ```vue
      父组件中：
              <Category>
                 <div>html结构1</div>
              </Category>
      子组件中：
              <template>
                  <div>
                     <!-- 定义插槽 -->
                     <slot>插槽默认内容...</slot>
                  </div>
              </template>
      ```

   2. 具名插槽：

      ```vue
      父组件中：
              <Category>
                  <template slot="center">
                    <div>html结构1</div>
                  </template>
                  <template v-slot:footer>
                     <div>html结构2</div>
                  </template>
              </Category>
      子组件中：
              <template>
                  <div>
                     <!-- 定义插槽 -->
                     <slot name="center">插槽默认内容...</slot>
                     <slot name="footer">插槽默认内容...</slot>
                  </div>
              </template>
      ```

   3. 作用域插槽：

      1. 理解：<span style="color:red">数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定。</span>（games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定）

      2. 具体编码：

         ```vue
         //父组件中：
         		<Category>
         			<template scope="scopeData">
         				<!-- 生成的是ul列表 -->
         				<ul>
         					<li v-for="g in scopeData.games" :key="g">{{g}}</li>
         				</ul>
         			</template>
         		</Category>
         
         		<Category>
         			<template slot-scope="scopeData">
         				<!-- 生成的是h4标题 -->
         				<h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
         			</template>
         		</Category>
         //子组件中：
                 <template>
                     <div>
                         <slot :games="games"></slot>
                     </div>
                 </template>
         <script>
                     export default {
                         name:'Category',
                         props:['title'],
                         //数据在子组件自身
                         data() {
                             return {
                                 games:['红色警戒','穿越火线','劲舞团','超级玛丽']
                             }
                         },
                     }
          </script>
         ```

### 5.vuex

​		在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信，多个组件需要共享数据时使用。

在[vue2](https://so.csdn.net/so/search?q=vue2&spm=1001.2101.3001.7020)中，要用vuex的3版本

```cmd
npm#
npm install vuex@3 --save
Yarn#
yarn add vuex@3 --save
在[vue3](https://so.csdn.net/so/search?q=vue3&spm=1001.2101.3001.7020)中，要用vuex的4版本
```

#### 1.搭建vuex环境

1. 创建文件：```src/store/index.js```

   ```js
   //引入Vue核心库
   import Vue from 'vue'
   //引入Vuex
   import Vuex from 'vuex'
   //应用Vuex插件
   Vue.use(Vuex)
   //准备actions对象——响应组件中用户的动作
   const actions = {}
   //准备mutations对象——修改state中的数据
   const mutations = {}
   //准备state对象——保存具体的数据
   const state = {}
   //创建并暴露store
   export default new Vuex.Store({
   	actions,
   	mutations,
   	state
   })
   ```

2. 在```main.js```中创建vm时传入```store```配置项

   ```js
   //引入Vue
   import Vue from 'vue'
   //引入App
   import App from './App.vue'
   //引入插件
   import vueResource from 'vue-resource'
   //引入store
   import store from './store'
   //关闭Vue的生产提示
   Vue.config.productionTip = false
   //使用插件
   Vue.use(vueResource)
   //创建vm
   new Vue({
   	el:'#app',
   	render: h => h(App),
   	store,
   	beforeCreate() {
   		Vue.prototype.$bus = this
   	}
   })
   ```

####    2.基本使用

1. 初始化数据、配置```actions```、配置```mutations```，操作文件```store.js```

   ```js
   //引入Vue核心库
   import Vue from 'vue'
   //引入Vuex
   import Vuex from 'vuex'
   //引用Vuex
   Vue.use(Vuex)
   //准备actions对象——响应组件中用户的动作
   const actions = {
       //响应组件中加的动作
   	jia(context,value){
   		// console.log('actions中的jia被调用了',miniStore,value)
   		context.commit('JIA',value)
   	},
   }
   //准备mutations对象——修改state中的数据
   const mutations = {
       //执行加
   	JIA(state,value){
   		// console.log('mutations中的JIA被调用了',state,value)
   		state.sum += value
   	}
   }
   //初始化数据
   const state = {
      sum:0
   }
   //创建并暴露store
   export default new Vuex.Store({
   	actions,
   	mutations,
   	state,
   })
   ```

2. 组件中读取vuex中的数据：```$store.state.sum```

   ```vue
   <h1>当前求和为：{{$store.state.sum}}</h1>
   ```

3. 组件中修改vuex中的数据：```$store.dispatch('action中的方法名',数据)``` 或 ```$store.commit('mutations中的方法名',数据)```

   ```vue
   <script>
   	export default {
   		name:'Count',
   		data() {
   			return {
   				n:1, //用户选择的数字			
   			}
   		},
   		methods: {
   			increment(){
   				//使用vuex实现求和的操作
   				this.$store.commit('JIA',this.n)
   			},
   			decrement(){
   				// this.sum -= this.n
   				this.$store.commit('JIAN',this.n)
   			},
   			incrementOdd(){
   				// if(this.sum % 2){
   				// 	this.sum += this.n
   				// }
   				if(this.$store.state.sum%2){
   					this.$store.commit('JIA',this.n)
   				}
   			},
   			incrementWait(){
   				setTimeout(()=>{
   					// this.sum += this.n
   					this.$store.commit('JIA',this.n)
   				},500)
   			},
   		},
   	}
   </script>
   ```

   **备注：若没有网络请求或其他业务逻辑，组件中也可以越过actions，即不写```dispatch```，直接编写```commit```**

#### 3.getters的使用

1. 概念：当state中的数据需要经过加工后再使用时，可以使用getters加工。

2. 在```store.js```中追加```getters```配置

   ```js
   const getters = {
   	bigSum(state){
   		return state.sum * 10
   	}
   }
   //创建并暴露store
   export default new Vuex.Store({
   	......
   	getters
   })
   ```

3. 组件中读取数据：```$store.getters.bigSum```

```vue
<h2>当前的数值放大十倍之后的为：{{$store.getters.bigSum}}</h2>
```

#### 4.四个map方法的使用

四个map方法的重要作用为简化代码的编写，不用重复多次写同样的内容

1. <strong>mapState方法：</strong>用于帮助我们映射```state```中的数据为计算属性

   ```js
   computed: {
       //借助mapState生成计算属性：sum、school、subject（对象写法）
        ...mapState({sum:'sum',school:'school',subject:'subject'}),
       //借助mapState生成计算属性：sum、school、subject（数组写法）
       ...mapState(['sum','school','subject']),
   },
   ```

2. <strong>mapGetters方法：</strong>用于帮助我们映射```getters```中的数据为计算属性

   ```js
   computed: {
       //借助mapGetters生成计算属性：bigSum（对象写法）
       ...mapGetters({bigSum:'bigSum'}),
       //借助mapGetters生成计算属性：bigSum（数组写法）
       ...mapGetters(['bigSum'])
   },
   ```

   代码示例


```vue
import { mapActions, mapGetters, mapMutations, mapState } from 'vuex'
<script>
	computed:{
      // sum(){
      //  return this.$store.state.sum
      // },
      // bigSum(){
      //  return this.$store.getters.bigSum
      // },
      // school(){
      //  return this.$store.state.school
      // },
      // subject(){
      //  return  this.$store.state.subject
      // }
      ...mapState(['sum','school','subject']),
      ...mapGetters(['bigSum'])
    }
</script>
```

<strong>mapActions方法：</strong>用于帮助我们生成与```actions```对话的方法，即：包含```$store.dispatch(xxx)```的函数

```js
methods:{
    //靠mapActions生成：incrementOdd、incrementWait（对象形式）
    ...mapActions({increment:'JIA',decrement:'JIAN'})
    //靠mapActions生成：incrementOdd、incrementWait（数组形式）
    ...mapActions(['JIA','JIAN'])
}
```

<strong>mapMutations方法：</strong>用于帮助我们生成与```mutations```对话的方法，即：包含```$store.commit(xxx)```的函数

```js
methods:{
    //靠mapActions生成：increment、decrement（对象形式）
    ...mapMutations({increment:'JIA',decrement:'JIAN'}),
    //靠mapMutations生成：JIA、JIAN（对象形式）
    ...mapMutations(['JIA','JIAN']),
}
```

**备注：mapActions与mapMutations使用时，若需要传递参数需要：在模板中绑定事件时传递好参数，否则参数是事件对象。**

```vue
<button @click="increment(n)">+</button>
<button @click="decrement(n)">-</button>
```

#### 5.模块化和命名空间（跳）

 ### 6.路由

1. 理解： 一个路由（route）就是一组映射关系（key - value），多个路由需要路由器（router）进行管理。
2. 前端路由：key是路径，value是组件。

#### 1.基本使用

1. 安装vue-router，命令：```npm i vue-router```

   注意：vue2应当安装的router版本是3版本

2. 应用插件：```Vue.use(VueRouter)```

3. 编写router配置项:

   一般来说，在src的目录底下新建一个router的文件夹，并在里面创建一个index.js文件用来配置router

```js
import VueRouter from "vue-router";
//引入组件
import About from '../components/About'
import Home  from '../components/Home'
//创建并暴露一个路由
export default new VueRouter({
  routes:[
    {
      path:'/about',
      component:About
    },
    {
      path:'/Home',
      component:Home
    }
  ]
})
```

4.在页面上引入配置的router配置，一般是在app.vue里面

```vue
<!-- Vue中借助router-link标签实现路由的切换 -->
<router-link class="list-group-item" active-class="active" to="/about">About</router-link>
<router-link class="list-group-item" active-class="active" to="/home">Home</router-link>
<div class="panel-body">
	<!-- 指定组件的呈现位置 -->
    <router-view></router-view>
</div>
```

注意：

- 路由组件通常存放在```pages```文件夹，一般组件通常存放在```components```文件夹。
- 通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
- 每个组件都有自己的```$route```属性，里面存储着自己的路由信息。
- 整个应用只有一个router，可以通过组件的```$router```属性获取到。

#### 2.多级路由

1. 配置路由规则，使用children配置项：

   /router/index.js

   ```js
   routes:[
   	{
   		path:'/about',
   		component:About,
   	},
   	{
   		path:'/home',
   		component:Home,
   		children:[ //通过children配置子级路由
   			{
   				path:'news', //此处一定不要写：/news
   				component:News
   			},
   			{
   				path:'message',//此处一定不要写：/message
   				component:Message
   			}
   		]
   	}
   ]
   ```

2. 跳转（要写完整路径）：

   ```vue
   //在home文件中需要配置要跳转的二级路由路径，在news和message文件里面只需要写出页面内容即可
   <ul class="nav nav-tabs">
   	<li>
   		<router-link class="list-group-item" active-class="active" to="/home/news">News</router-link>
   	</li>
   	<li>
   		<router-link class="list-group-item" active-class="active" to="/home/message">Message</router-link>
   	</li>
   </ul>
   ```

#### 3.路由的query参数

在message底下定义一个detatil用来查看详情，需要在message里面配置router信息，并传递参数。

```vue
<li v-for="m in messageList" :key="m.id">
 	<router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{m.title}}</router-link>
    <!-- 跳转并携带query参数，to的对象写法 -->
	<router-link 
		:to="{
			path:'/home/message/detail',
			query:{
		   		id:666,
            	title:'你好'
			}
		}"
</li>
<hr>
//向页面展示并渲染数据
<router-view></router-view>
<script>
    data(){
            return{
                messageList:[
                	{id:"001",title:"你好啊01"},
                	{id:"002",title:"你好啊02"},
                	{id:"003",title:"你好啊03"},
            	]
            }
            
 }
</script>

```

detail里面用来查询数据接收参数，并且渲染给页面

```vue
<template>  
	<ul>
  		<li>消息编号：{{$route.query.id}}</li>
    	<li>消息标题：{{$route.query.title}}</li>
   </ul>
</template>
<script>
 export default {
    name:'Detail',
    mounted() {
      console.log(this.$route)
   },
  }
</script>
```

#### 4.路由的params参数

1. 配置路由，声明接收params参数

   router/index.js文件

   ```js
   {
   	path:'/home',
   	component:Home,
   	children:[
   		{
   			path:'news',
   			component:News
   		},
   		{
   			component:Message,
   			children:[
   				{
   					name:'xiangqing',
   					path:'detail/:id/:title', //使用占位符声明接收params参数
   					component:Detail
   				}
   			]
   		}
   	]
   }
   ```

2. 传递参数

   ```vue
   <!-- 跳转并携带params参数，to的字符串写法 -->
   <router-link :to="/home/message/detail/666/你好">跳转</router-link>
   <!-- 跳转并携带params参数，to的对象写法 -->
   <router-link 
   	:to="{
   		name:'xiangqing',
   		params:{
   		   id:666,
               title:'你好'
   		}
   	}"
   >跳转</router-link>
   ```

   **特别注意：路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置！**

3. 接收参数：

   detail.vue里面的内容，用来接受参数

   ```vue
   <template>
   	<ul>
   		<li>消息编号：{{$route.params.id}}</li>
   		<li>消息标题：{{$route.params.title}}</li>
   	</ul>
   </template>
   ```

#### 5.路由的props配置

​	作用：让路由组件更方便的收到参数

router/index.js文件

```js
{
	name:'xiangqing',
	path:'detail/:id',
	component:Detail,
	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
	// props:{a:900}
	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
	// props:true
	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
	props(route){
		return {
			id:route.query.id,
			title:route.query.title
		}
	}
}
```

detail.vue里面接收参数

```vue
<template>
  <ul>
   	<li>消息编号：{{id}}</li>
    <li>消息标题：{{title}}</li>
  </ul>
</template>
<script>
  export default {
    name:'Detail',
    props:['id','title'],
    mounted() {
      console.log(this.$route)
    },
  }
</script>
```

#### 6.replace属性

- 作用：控制路由跳转时操作浏览器历史记录的模式
- 浏览器的历史记录有两种写入方式：分别为```push```和```replace```，```push```是追加历史记录，```replace```是替换当前记录。路由跳转时候默认为```push```
- 如何开启```replace```模式：```<router-link replace .......>News</router-link>```

#### 7.路由守卫

1. 作用：对路由进行权限控制

2. 分类：全局守卫、独享守卫、组件内守卫

3. 全局守卫:

   ```js
   //全局前置守卫：初始化时执行、每次路由切换前执行
   router.beforeEach((to,from,next)=>{
   	console.log('beforeEach',to,from)
   	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
   		if(localStorage.getItem('school') === 'atguigu'){ //权限控制的具体规则
   			next() //放行
   		}else{
   			alert('暂无权限查看')
   			// next({name:'guanyu'})
   		}
   	}else{
   		next() //放行
   	}
   })
   //全局后置守卫：初始化时执行、每次路由切换后执行
   router.afterEach((to,from)=>{
   	console.log('afterEach',to,from)
   	if(to.meta.title){ 
   		document.title = to.meta.title //修改网页的title
   	}else{
   		document.title = 'vue_test'
   	}
   })
   ```

4. 独享守卫:

   ```js
   beforeEnter(to,from,next){
   	console.log('beforeEnter',to,from)
   	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
   		if(localStorage.getItem('school') === 'atguigu'){
   			next()
   		}else{
   			alert('暂无权限查看')
   			// next({name:'guanyu'})
   		}
   	}else{
   		next()
   	}
   }
   ```

5. 组件内守卫：

   ```js
   //进入守卫：通过路由规则，进入该组件时被调用
   beforeRouteEnter (to, from, next) {
   },
   //离开守卫：通过路由规则，离开该组件时被调用
   beforeRouteLeave (to, from, next) {
   }
   ```
